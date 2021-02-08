---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/11/2021
title: JDBC 和 ODBC 驱动程序以及配置参数 - Azure Databricks
description: 了解如何获取连接到 Azure Databricks 群集和 SQL 终结点所需的 JDBC 和 ODBC 驱动程序和配置参数。
ms.openlocfilehash: 52cc0a9c25f3fb544f87f009ecc717f72cda606d
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058983"
---
# <a name="jdbc-and-odbc-drivers-and-configuration-parameters"></a>JDBC 和 ODBC 驱动器以及配置参数

可以将商业智能 (BI) 工具连接到 Azure Databricks 工作区群集和 SQL Analytics SQL 终结点，以查询表中的数据。 本文介绍了如何获取连接到群集和 SQL 终结点所需的 JDBC 和 ODBC 驱动程序以及配置参数。 有关特定于工具的连接说明，请参阅[商业智能工具](index.md)。

## <a name="permission-requirements"></a>权限要求

使用 JDBC 或 ODBC 访问计算资源所需的权限取决于你是连接到群集还是连接到 SQL 终结点。

### <a name="workspace-cluster-requirements"></a>工作区群集要求

若要访问[群集](../../clusters/index.md)，你必须拥有“可附加到”[权限](../../security/access-control/cluster-acl.md)。

如果你连接到已终止的群集，并且拥有“可重启”权限，则群集会重启。

### <a name="sql-analytics-sql-endpoint-requirements"></a>SQL Analytics SQL 终结点要求

若要访问 [SQL 终结点](../../sql/admin/sql-endpoints.md)，你必须拥有“可以使用”[权限](../../sql/user/security/access-control/sql-endpoint-acl.md)。

如果你连接到已停止的终结点，并且拥有“可以使用”权限，则 SQL 终结点会启动。

## <a name="step-1-download-and-install-a-jdbc-or-odbc-driver"></a><a id="driver"> </a><a id="step-1-download-and-install-a-jdbc-or-odbc-driver"> </a>步骤 1：下载并安装 JDBC 或 ODBC 驱动程序

对于所有 BI 工具，你都需要一个 JDBC 或 ODBC 驱动程序来连接到 Azure Databricks 计算资源。

1. 转到 [Databricks JDBC/ODBC 驱动程序下载页面](https://databricks.com/spark/odbc-driver-download/)。
2. 填写表格并提交。 页面将更新并包含指向多个下载选项的链接。
3. 选择一个驱动程序并下载它。
4. 安装驱动程序。 对于 JDBC，提供了不需要安装的 JAR。 对于 ODBC，将为所选平台提供一个安装包。

## <a name="step-2-collect-jdbc-or-odbc-connection-information"></a>步骤 2：收集 JDBC 或 ODBC 连接信息

若要配置 JDBC 或 ODBC 驱动程序，必须从 Azure Databricks 收集连接信息。 以下是 JDBC 或 ODBC 驱动程序可能需要的一些参数：

| 参数                             | 值                                                                                                       |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Authentication                        | 请参阅[获取身份验证凭据](#authentication)。                                                      |
| 主机、端口、HTTP 路径、JDBC URL       | 请参阅[获取服务器主机名、端口、HTTP 路径和 JDBC URL](#get-server-hostname-port-http-path-and-jdbc-url)。 |

对于 ODBC 的 JDBC 和 DSN 配置，通常会在 ``httpPath`` 中指定以下内容：

| 参数                             | 值                                                  |
|---------------------------------------|--------------------------------------------------------|
| Spark 服务器类型                     | Spark Thrift 服务器                                    |
| 架构/数据库                       | default                                                |
| 身份验证机制 ``AuthMech`` | 请参阅[获取身份验证凭据](#authentication)。 |
| Thrift 传输                      | http                                                   |
| SSL                                   | 1                                                      |

### <a name="get-authentication-credentials"></a><a id="authentication"> </a><a id="get-authentication-credentials"> </a>获取身份验证凭据

本部分介绍了如何收集受支持的凭据，以便向 Azure Databricks 计算资源证明 BI 工具的身份。

#### <a name="username-and-password-authentication"></a>用户名和密码身份验证

你可以使用 Azure Databricks 个人访问令牌或 Azure Active Directory (Azure AD) 令牌。 Databricks JDBC 和 ODBC 驱动程序不支持 Azure Active Directory 用户名和密码身份验证。

* **用户名**：``token``
* **密码**：用户生成的令牌。 检索用户名和密码身份验证的令牌的过程取决于你使用的是 Azure Databricks 工作区群集还是 SQL Analytics SQL 终结点。
  * 工作区：
    * Azure Databricks 个人访问令牌：请按照[生成个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)中的说明获取个人访问令牌。
    * Azure Active Directory 令牌：Databricks JDBC 和 ODBC 驱动程序 2.6.15 及更高版本支持使用 [Azure Active Directory](../../dev-tools/api/latest/aad/index.md) 令牌进行的身份验证。 请按照[获取访问令牌](../../dev-tools/api/latest/aad/app-aad-token.md#get-token)中的说明获取令牌。
  * SQL Analytics 个人访问令牌：请按照[生成个人访问令牌](../../sql/user/security/personal-access-tokens.md#generate)中的说明获取个人访问令牌。

### <a name="get-server-hostname-port-http-path-and-jdbc-url"></a>获取服务器主机名、端口、HTTP 路径和 JDBC URL

检索 JDBC 和 ODBC 参数的过程取决于你使用的是 Azure Databricks 工作区群集还是 SQL Analytics SQL 终结点。

#### <a name="in-this-section"></a>本部分内容：

* [工作区群集](#workspace-cluster)
* [SQL Analytics SQL 终结点](#sql-analytics-sql-endpoint)

#### <a name="workspace-cluster"></a>工作区群集

1. 单击 ![“群集”图标](../../_static/images/icons/clusters-icon.png) “模型”图标。
2. 单击某个群集。
3. 单击“高级选项”开关。
4. 单击“JDBC/ODBC”选项卡。

   > [!div class="mx-imgBorder"]
   > ![JDBC-ODBC 选项卡](../../_static/images/third-party-integrations/jdbc-odbc-tab-azure.png)

5. 复制 BI 工具所需的参数。

#### <a name="sql-analytics-sql-endpoint"></a>SQL Analytics SQL 终结点

1. 单击 ![“终结点”图标](../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 单击一个终结点。
3. 单击“连接详细信息”选项卡。

   > [!div class="mx-imgBorder"]
   > ![连接详细信息](../../_static/images/sql/endpoint-connection-details.png)

4. 复制 BI 工具所需的参数。

## <a name="step-3-configure-jdbc-url"></a><a id="jdbc"> </a><a id="step-3-configure-jdbc-url"> </a>步骤 3：配置 JDBC URL

配置 JDBC URL 的步骤取决于你使用的是 Azure Databricks 工作区群集还是 SQL Analytics SQL 终结点。

### <a name="in-this-section"></a>本部分内容：

* [工作区群集](#workspace-cluster)
* [SQL Analytics SQL 终结点](#sql-analytics-sql-endpoint)

### <a name="workspace-cluster"></a>工作区群集

#### <a name="username-and-password-authentication"></a>用户名和密码身份验证

将 ``<personal-access-token>`` 替换为你在[获取身份验证凭据](#authentication)中创建的令牌。 例如：

```
jdbc:spark://<server-hostname>:443/default;transportMode=http;ssl=1;httpPath=sql/protocolv1/o/0/xxxx-xxxxxx-xxxxxxxx;AuthMech=3;UID=token;PWD=<personal-access-token>
```

#### <a name="azure-active-directory-token-authentication"></a>Azure Active Directory 令牌身份验证

1. 删除现有的身份验证参数 (``AuthMech, UID, PWD``)：

   ```
   AuthMech=3;UID=token;PWD=<personal-access-token>
   ```

2. 使用在[获取身份验证凭据](#authentication)中获取的 Azure AD 令牌添加以下参数。

   ```
   AuthMech=11;Auth_Flow=0;Auth_AccessToken=<Azure AD token>
   ```

另请参阅[刷新 Azure Active Directory 令牌](#refresh-azure-active-directory-token)。

### <a name="sql-analytics-sql-endpoint"></a>SQL Analytics SQL 终结点

将 ``<personal-access-token>`` 替换为你在[获取身份验证凭据](#authentication)中创建的令牌。 例如：

```
jdbc:spark://<server-hostname>:443/default;transportMode=http;ssl=1;httpPath=sql/protocolv1/o/0/xxxx-xxxxxx-xxxxxxxx;AuthMech=3;UID=token;PWD=<personal-access-token>
```

## <a name="configure-for-native-query-syntax"></a>针对原生查询语法进行配置

JDBC 和 ODBC 驱动程序接受 ANSI SQL-92 方言中的 SQL 查询，并将查询转换为 Spark SQL。 如果应用程序直接生成 Spark SQL，或者应用程序使用特定于 Azure Databricks 的任何非 ANSI SQL-92 标准 SQL 语法，Databricks 建议你将 ``;UseNativeQuery=1`` 添加到连接配置中。 有了该设置，驱动程序就会将 SQL 查询逐字传递到 Azure Databricks。

## <a name="configure-odbc-data-source-name-for-the-simba-odbc-driver"></a><a id="configure-odbc-data-source-name-for-the-simba-odbc-driver"> </a><a id="odbc"> </a>配置 Simba ODBC 驱动程序的 ODBC 数据源名称

数据源名称 (DSN) 配置包含用于与特定数据库通信的参数。 BI 工具（如 Tableau）通常会提供一个用户界面，用于输入这些参数。 如果你必须自行安装和管理 Simba ODBC 驱动程序，则可能需要创建配置文件，并允许驱动程序管理器（Windows 上的 [ODBC 数据源管理器](https://docs.microsoft.com/sql/database-engine/configure-windows/open-the-odbc-data-source-administrator)和 Unix 上的 [unixODBC](http://www.unixodbc.org/)/[iODBC](http://www.iodbc.org/dataspace/doc/iodbc/wiki/iodbcWiki/WelcomeVisitors)）来访问这些配置文件。 创建两个文件：``/etc/odbc.ini`` 和 ``/etc/odbcinst.ini``。

### <a name="in-this-section"></a>本部分内容：

* [``/etc/odbc.ini``](#etcodbcini)
* [``/etc/odbcinst.ini``](#etcodbcinstini)
* [配置 ODBC 配置文件的路径](#configure-paths-of-odbc-configuration-files)

### ``/etc/odbc.ini``

#### <a name="username-and-password-authentication"></a>用户名和密码身份验证

1. 将 ``/etc/odbc.ini`` 的内容设置为：

   ```ini
   [Databricks-Spark]
   Driver=Simba
   Server=<server-hostname>
   HOST=<server-hostname>
   PORT=<port>
   SparkServerType=3
   Schema=default
   ThriftTransport=2
   SSL=1
   AuthMech=3
   UID=token
   PWD=<personal-access-token>
   HTTPPath=<http-path>
   ```

2. 将 ``<personal-access-token>`` 设置为你在[获取身份验证凭据](#authentication)中检索到的令牌。
3. 将服务器、端口和 HTTP 参数设置为你在[获取服务器主机名、端口、HTTP 路径和 JDBC URL](#get-server-hostname-port-http-path-and-jdbc-url) 中检索到的参数。

#### <a name="azure-active-directory-token-authentication"></a>Azure Active Directory 令牌身份验证

1. 将 ``/etc/odbc.ini`` 的内容设置为：

   ```ini
   [Databricks-Spark]
   Driver=Simba
   Server=<server-hostname>
   HOST=<server-hostname>
   PORT=443
   HTTPPath=<http-path>
   SparkServerType=3
   Schema=default
   ThriftTransport=2
   SSL=1
   AuthMech=11
   Auth_Flow=0
   Auth_AccessToken=<Azure AD token>
   ```

2. 将 ``<Azure AD token>`` 设置为你在[获取身份验证凭据](#authentication)中检索到的 Azure Active Directory 令牌。
3. 将服务器、端口和 HTTP 参数设置为你在[获取服务器主机名、端口、HTTP 路径和 JDBC URL](#get-server-hostname-port-http-path-and-jdbc-url) 中检索到的参数。

另请参阅[刷新 Azure Active Directory 令牌](#refresh-azure-active-directory-token)。

### ``/etc/odbcinst.ini``

将 ``/etc/odbcinst.ini`` 的内容设置为：

```ini
[ODBC Drivers]
Simba = Installed
[Simba Spark ODBC Driver 64-bit]
Driver = <driver-path>
```

根据在步骤 1 下载驱动程序时选择的操作系统设置 ``<driver-path>``：

* MacOs ``/Library/simba/spark/lib/libsparkodbc_sbu.dylib``
* Linux (64-bit) ``/opt/simba/spark/lib/64/libsparkodbc_sb64.so``
* Linux (32-bit) ``/opt/simba/spark/lib/32/libsparkodbc_sb32.so``

### <a name="configure-paths-of-odbc-configuration-files"></a>配置 ODBC 配置文件的路径

在环境变量中指定两个文件的路径，以便驱动程序管理器可以使用它们：

```ini
export ODBCINI=/etc/odbc.ini
export ODBCSYSINI=/etc/odbcinst.ini
export SIMBASPARKINI=<simba-ini-path>/simba.sparkodbc.ini # (Contains the configuration for debugging the Simba driver)
```

其中，``<simba-ini-path>`` 为

* MacOS ``/Library/simba/spark/lib``
* Linux (64-bit) ``/opt/simba/sparkodbc/lib/64``
* Linux (32-bit) ``/opt/simba/sparkodbc/lib/32``

## <a name="refresh-an-azure-active-directory-token"></a><a id="refresh-an-azure-active-directory-token"> </a><a id="refresh-azure-active-directory-token"> </a>刷新 Azure Active Directory 令牌

Azure Active Directory 令牌在 1 小时后过期。 如果未使用 ``refresh_token`` 来刷新访问令牌，则必须手动将该令牌替换为新令牌。 本部分介绍了如何在不中断连接的情况下以编程方式刷新现有会话的令牌。

1. 请按照[刷新访问令牌](../../dev-tools/api/latest/aad/app-aad-token.md#refresh-an-access-token)中的步骤进行操作。
2. 按照下面的方式更新令牌：
   * **JDBC**：使用 ``Auth_AccessToken`` 的新值调用 ``java.sql.Connection.setClientInfo``。
   * **ODBC**：使用 ``Auth_AccessToken`` 的新值针对 ``SQL_ATTR_CREDENTIALS`` 调用 ``SQLSetConnectAttr``。

## <a name="troubleshooting"></a>疑难解答

请参阅[排查 JDBC 和 ODBC 连接问题](/azure/databricks/kb/bi/jdbc-odbc-troubleshooting)。