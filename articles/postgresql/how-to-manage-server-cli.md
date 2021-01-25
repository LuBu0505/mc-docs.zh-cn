---
title: 管理服务器 - Azure CLI - Azure Database for PostgreSQL
description: 了解如何通过 Azure CLI 管理 Azure Database for PostgreSQL 服务器。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: how-to
origin.date: 9/22/2020
ms.date: 01/18/2021
ms.openlocfilehash: 79c03b15e450a5f14fdbe6491bbabee52ed8ceda
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98230665"
---
# <a name="manage-an-azure-database-for-postgresql-single-server-using-the-azure-cli"></a>使用 Azure CLI 管理 Azure Database for PostgreSQL 单一服务器

本文介绍如何管理 Azure 中部署的单一服务器。 管理任务包括计算和存储缩放、管理员密码重置，以及查看服务器详细信息。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。 本文要求在本地运行 Azure CLI 2.0 或更高版本。 若要查看安装的版本，请运行 `az --version` 命令。 如果需要进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

你将需要使用 [az login](/cli/reference-index#az-login) 命令登录到你的帐户。 请注意 id 属性，该属性指的是 Azure 帐户的订阅 ID。

```azurecli
az login
```

使用 [az account set](/cli/account) 命令选择帐户下的特定订阅。 记下 az login 输出中的 id 值，以用作命令中订阅参数的值。 如果有多个订阅，请选择应计费的资源所在的相应订阅。 若要获取所有订阅，请使用 [az account list](/cli/account#az-account-list)。

```azurecli
az account set --subscription <subscription id>
```

如果尚未创建服务器，请参考此[快速入门](quickstart-create-server-database-azure-cli.md)创建一个。

## <a name="scale-compute-and-storage"></a>缩放计算和存储

可以使用以下命令轻松地纵向扩展定价层、计算和存储。 可以参阅 [az postgres server 概述](/cli/mysql/server)，了解可执行的所有服务器操作

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --sku-name GP_Gen5_4 --storage-size 6144
```

下面是上述参数的详细信息：

**设置** | **示例值** | **说明**
---|---|---
name | mydemoserver | 输入 Azure Database for PostgreSQL 服务器的唯一名称。 服务器名称只能包含小写字母、数字和连字符 (-) 字符。 必须包含 3 到 63 个字符。
resource-group | myresourcegroup | 提供 Azure 资源组的名称。
sku-name|GP_Gen5_2|输入定价层和计算配置的名称。 请遵循简写约定 {pricing tier} _{compute generation}_ {vCores}。 有关详细信息，请参阅[定价层](./concepts-pricing-tiers.md)。
storage-size | 6144 | 服务器的存储容量（以 MB 为单位）。 最小值为 5120，以 1024 为增量递增。

> [!Important]
> - 存储可以纵向扩展（但不能纵向缩减）
> - 不支持从“基本”定价层纵向扩展到“常规用途”或“内存优化”定价层。 可以[使用 bash 脚本](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/upgrade-from-basic-to-general-purpose-or-memory-optimized-tiers/ba-p/830404)或[使用 PostgreSQL Workbench](https://techcommunity.microsoft.com/t5/azure-database-support-blog/how-to-scale-up-azure-database-for-mysql-from-basic-tier-to/ba-p/369134) 进行手动纵向扩展


## <a name="manage-postgresql-databases-on-a-server"></a>管理服务器上的 PostgreSQL 数据库。
可以使用以下任何命令来创建、删除、列出和查看服务器上数据库的数据库属性。

| Cmdlet | 使用情况| 说明 |
| --- | ---| --- |
|[az postgres db create](/cli/sql/db#az-mysql-db-create)|```az postgres db create -g myresourcegroup -s mydemoserver -n mydatabasename``` |创建数据库|
|[az postgres db delete](/cli/sql/db#az-mysql-db-delete)|```az postgres db delete -g myresourcegroup -s mydemoserver -n mydatabasename```|从服务器中删除数据库。 此命令不会删除服务器。 |
|[az postgres db list](/cli/sql/db#az-mysql-db-list)|```az postgres db list -g myresourcegroup -s mydemoserver```|列出服务器上的所有数据库|
|[az postgres db show](/cli/sql/db#az-mysql-db-show)|```az postgres db show -g myresourcegroup -s mydemoserver -n mydatabasename```|显示数据库的更多详细信息|

## <a name="update-admin-password"></a>更新管理员密码
可以使用此命令更改管理员角色的密码
```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!Important]
>  请确保密码至少有 8 个字符，至多有 128 个字符。
> 密码必须包含以下类别中的三个类别的字符：英文大写字母、英文小写字母、数字和非字母数字字符。

## <a name="delete-a-server"></a>删除服务器
如果只想删除 PostgreSQL 单一服务器，可运行 [az postgres server delete](/cli/mysql/server#az-mysql-server-delete) 命令。

```azurecli
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>后续步骤
- [重启服务器](howto-restart-server-cli.md)
- [还原处于错误状态的服务器](howto-restore-server-cli.md)
- [监视和优化服务器](concepts-monitoring.md)
