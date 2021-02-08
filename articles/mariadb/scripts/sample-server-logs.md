---
title: CLI 脚本 - 下载慢查询日志 - Azure Database for MariaDB
description: 此示例 Azure CLI 脚本演示如何启用和下载 Azure Database for MariaDB 服务器的慢查询日志。
author: WenJason
ms.author: v-jay
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
origin.date: 12/02/2019
ms.date: 02/08/2021
ms.openlocfilehash: 6e93ed477c772236abef01ac54db525f9f5f49fa
ms.sourcegitcommit: 20bc732a6d267b44aafd953516fb2f5edb619454
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99503877"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>使用 Azure CLI 启用和下载 Azure Database for MariaDB 服务器的服务器慢查询日志
此示例 CLI 脚本可启用和下载单个 Azure Database for MariaDB 服务器的慢查询日志。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- 本文需要 Azure CLI 版本 2.0 或更高版本。

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。 将 `az monitor` 命令中的 &lt;log_file_name&gt; 替换自己的服务器日志文件名。

```azurecli
#!/bin/bash

# Create a resource group
az group create \
--name myresourcegroup  \
--location chinaeast2

# Create a MariaDB server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure
# Substitute the <server_admin_password> with your own value
az mariadb server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# List the configuration options for review
az mariadb server configuration list \
--resource-group myresourcegroup  \
--server mydemoserver

# Turn on statement level log
az mariadb server configuration set \
--name log_statement \
--resource-group myresourcegroup \
--server mydemoserver \
--value all

# Set log_min_duration_statement time to 10 sec
az mariadb server configuration set \
--name log_min_duration_statement \
--resource-group myresourcegroup \
--server mydemoserver \
--value 10000

# List the available log files and direct to a text file
az mariadb server-logs list \
--resource-group myresourcegroup \
--server mydemoserver > log_files_list.txt

# Download log file from Azure 
# Review log_files_list.txt to find the server log file name for the desired timeframe
# Substitute the <log_file_name> with your server log file name
# Creates the postgresql-<date>_000000.log file in the current command line path
az mariadb server-logs download \
--name <log_file_name> \
--resource-group myresourcegroup \
--server mydemoserver
```

## <a name="clean-up-deployment"></a>清理部署
运行脚本示例后，请使用以下命令删除资源组以及与其关联的所有资源。 

```azurecli
#!/bin/bash
az group delete --name myresourcegroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az mariadb server create](https://docs.microsoft.com/cli/azure/mariadb/server#az-mariadb-server-create) | 创建用于托管数据库的 MariaDB 服务器。 |
| [az mariadb server configuration list](https://docs.microsoft.com/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | 列出服务器的配置值。 |
| [az mariadb server configuration set](https://docs.microsoft.com/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | 更新服务器的配置。 |
| [az mariadb server-logs list](https://docs.microsoft.com/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) | 列出服务器的日志文件。 |
| [az mariadb server-logs download](https://docs.microsoft.com/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) | 下载日志文件。 |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。
- 请尝试其他脚本：[Azure Database for MariaDB 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
