---
title: 使用 Azure Active Directory - Azure Database for MySQL
description: 了解如何设置 Azure Active Directory (Azure AD) 以向 Azure Database for MySQL 进行身份验证
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: how-to
origin.date: 07/23/2020
ms.date: 09/28/2020
ms.openlocfilehash: 43d0e816c743815c090e8f0f733ff4abd74de1ba
ms.sourcegitcommit: 71953ae66ddfc07c5d3b4eb55ff8639281f39b40
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2020
ms.locfileid: "91395580"
---
# <a name="use-azure-active-directory-for-authentication-with-mysql"></a>使用 Azure Active Directory 进行 MySQL 的身份验证

本文将分步介绍如何使用 Azure Database for MySQL 配置 Azure Active Directory 访问权限以及如何使用 Azure AD 令牌进行连接。

> [!IMPORTANT]
> Azure Active Directory 身份验证仅适用于 MySQL 5.7 和更高版本。

## <a name="setting-the-azure-ad-admin-user"></a>设置 Azure AD 管理员用户

只有 Azure AD 管理员用户才能为基于 Azure AD 的身份验证创建/启用用户。 若要创建 Azure AD 管理员用户，请按照以下步骤操作

1. 在 Azure 门户中，选择要为 Azure AD 启用的 Azure Database for MySQL 实例。
2. 在“设置”下，选择“Active Directory 管理员”：

![设置 Azure AD 管理员][2]

3. 在客户租户中选择一个有效的 Azure AD 用户作为 Azure AD 管理员。

> [!IMPORTANT]
> 设置管理员时，将向具有完全管理员权限的 Azure Database for MySQL 服务器添加新用户。

每个 MySQL 服务器只能创建一个 Azure AD 管理员，选择另一个管理员将覆盖为服务器配置的现有 Azure AD 管理员。

在将来的版本中，我们将支持指定 Azure AD 组，而不是让单个用户拥有多个管理员，但目前尚不支持此功能。

配置管理员后，现在就可以登录了：

## <a name="connecting-to-azure-database-for-mysql-using-azure-ad"></a>使用 Azure AD 连接到 Azure Database for MySQL

以下概要关系图概述了将 Azure AD 身份验证与Azure Database for MySQL 配合使用的工作流：

![身份验证流][1]

我们设计了 Azure AD 集成以便使用常见的 MySQL 工具（例如 mysql CLI），这些工具不能识别 Azure AD，并且仅支持在连接到 MySQL 时指定用户名和密码。 我们会将 Azure AD 令牌作为密码传递，如上图所示。

目前已测试以下客户端：

- MySQLWorkbench 
- Mysql CLI

我们还测试了最常用的应用程序驱动程序，你可以在本页面末尾查看详细信息。

以下是用户/应用程序使用 Azure AD 进行身份验证所需的步骤：

### <a name="prerequisites"></a>先决条件

请确保已[安装 Azure CLI](/clazure/install-azure-cli)。

### <a name="step-1-authenticate-with-azure-ad"></a>步骤 1：使用 Azure AD 进行身份验证

首先使用 Azure CLI 工具进行 Azure AD 的身份验证。

```
az login
```

该命令将启动浏览器窗口，以显示 Azure AD 身份验证页。 这一操作需要提供 Azure AD 的用户 ID 和密码。

### <a name="step-2-retrieve-azure-ad-access-token"></a>步骤 2：检索 Azure AD 访问令牌

调用 Azure CLI 工具，获取步骤 1 中经过 Azure AD 身份验证的用户的访问令牌，以访问 Azure Database for MySQL。

示例：

```azurecli
az account get-access-token --resource https://ossrdbms-aad.database.chinacloudapi.cn
```

上述资源值必须完全按所示方式指定。 可以使用以下命令来查看资源值：

```azurecli
az cloud show
```

对于 Azure CLI 版本 2.0.71 和更高版本，可在以下更方便的版本中指定该命令：

```azurecli
az account get-access-token --resource-type oss-rdbms
```

身份验证成功后，Azure AD 将返回访问令牌：

```json
{
  "accessToken": "TOKEN",
  "expiresOn": "...",
  "subscription": "...",
  "tenant": "...",
  "tokenType": "Bearer"
}
```

该令牌是一个 Base 64 字符串，该字符串对有关经过身份验证的用户的所有信息进行编码，并且针对的是 Azure Database for MySQL 服务。

> [!NOTE]
> 访问令牌的有效期为 5 - 60 分钟。 建议在即将启动 Azure Database for MySQL 登录之前获取访问令牌。

### <a name="step-3-use-token-as-password-for-logging-in-with-mysql"></a>步骤 3：使用令牌作为 MySQL 登录的密码

在连接时，需要将访问令牌用作 MySQL 用户密码。 使用 GUI 客户端（如 MySQLWorkbench）时，可以使用上面的方法来检索令牌。 

使用 CLI 时，可以使用下面的方法快速连接： 

**示例 (Linux/macOS)：**
```
mysql -h mydb.mysql.database.chinacloudapi.cn \ 
      --user user@tenant.partner.onmschina.cn.com@mydb \ 
  --enable-cleartext-plugin \ 
  --password=`az account get-access-token --resource-type oss-rdbms --output tsv --query accessToken`
```

连接时的重要注意事项如下：

* `user@tenant.partner.onmschina.cn` 是你尝试要以其身份连接的 Azure AD 用户或组的名称
* 在 Azure AD 用户/组名称的后面始终要追加服务器名称（例如 `@mydb`）
* 请确保使用 Azure AD 用户或组名称的确切拼写方式
* Azure AD 的用户名和组的名称区分大小写
* 在作为组进行连接时，请只使用组名称（例如 `GroupName@mydb`）
* 如果名称包含空格，请在每个空格前使用 `\` 对该空格进行转义

请注意“enable-cleartext-plugin”设置 - 需要对其他客户端使用类似的配置，以确保令牌在不进行哈希处理的情况下发送到服务器。

现在可以使用 Azure AD 身份验证向 MySQL 服务器进行身份验证了。

## <a name="creating-azure-ad-users-in-azure-database-for-mysql"></a>在 Azure Database for MySQL 中创建 Azure AD 用户

若要向 Azure Database for MySQL 数据库添加 Azure AD 用户，请在连接后执行以下步骤（请参阅下文中介绍连接方法的小节）：

1. 首先确保 Azure AD 用户 <user>@yourtenant.partner.onmschina.cn 是 Azure AD 租户中的有效用户。
2. 以 Azure AD 管理员用户身份登录到 Azure Database for MySQL 实例。
3. 在 Azure Database for MySQL 中创建用户 `<user>@yourtenant.partner.onmschina.cn`。

**示例：**

```sql
CREATE AADUSER 'user1@yourtenant.partner.onmschina.cn';
```

如果用户名超过 32 个字符，建议改用别名，以供连接时使用： 

示例：

```sql
CREATE AADUSER 'userWithLongName@yourtenant.partner.onmschina.cn' as 'userDefinedShortName'; 
```

> [!NOTE]
> 通过 Azure AD 对用户进行身份验证时，不会授权用户访问 Azure Database for MySQL 数据库中的对象。 必须手动向用户授予所需的权限。

## <a name="creating-azure-ad-groups-in-azure-database-for-mysql"></a>在 Azure Database for MySQL 中创建 Azure AD 组

若要启用 Azure AD 组以访问数据库，请使用与用户相同的机制，只不过要指定组名称：

**示例：**

```sql
CREATE AADUSER 'Prod_DB_Readonly';
```

登录时，组成员将使用其个人访问令牌，但使用指定为用户名的组名称进行签名。

## <a name="token-validation"></a>令牌验证

Azure Database for MySQL 中的 Azure AD 身份验证可确保 MySQL 服务器中存在用户，并通过验证令牌的内容来检查令牌的有效性。 执行以下令牌验证步骤：

- 令牌是否由 Azure AD 签名，并且未被篡改
- 是否由 Azure AD 为与服务器关联的租户颁发令牌
- 令牌是否未过期
- 令牌是否用于 Azure Database for MySQL 资源（而不是其他 Azure 资源）

## <a name="compatibility-with-application-drivers"></a>与应用程序驱动程序的兼容性

大部分驱动程序均受支持，但请确保使用以明文形式发送密码的设置，这样，无需进行修改就能发送令牌。

* C/C++
  * libmysqlclient：支持
  * mysql-connector-c++：支持
* Java
  * 连接器/J (mysql-connector-java)：支持，必须使用 `useSSL` 设置
* Python
  * 连接器/Python：支持
* Ruby
  * mysql2：支持
* .NET
  * mysql-connector-net：支持，需要为 mysql_clear_password 添加插件
  * mysql-net/MySqlConnector：支持
* Node.js
  * mysqljs：不支持（不以明文形式发送无修补程序的令牌）
  * node-mysql2：支持
* Perl
  * DBD::mysql：支持
  * Net::MySQL：不支持
* Go
  * go-sql-driver：支持，将 `?tls=true&allowCleartextPasswords=true` 添加到连接字符串

## <a name="next-steps"></a>后续步骤

* 查看[使用 Azure Active Directory 向 Azure Database for MySQL 进行身份验证](concepts-azure-ad-authentication.md)的总体概念

<!--Image references-->

[1]: ./media/concepts-azure-ad-authentication/authentication-flow.png
[2]: ./media/concepts-azure-ad-authentication/set-azure-ad-admin.png
