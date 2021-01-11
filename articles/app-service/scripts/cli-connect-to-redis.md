---
title: CLI：将应用连接到 Redis
description: 了解如何使用 Azure CLI 自动部署和管理应用服务应用。 此示例演示如何将应用连接到 Azure Cache for Redis。
author: msangapu-msft
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.devlang: azurecli
ms.topic: sample
origin.date: 12/11/2017
ms.date: 12/21/2020
ms.author: v-tawe
ms.custom: mvc, seodec18
ms.openlocfilehash: 0c9e579d3aac3f014fba737cd8f6880247e7e160
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022943"
---
# <a name="connect-an-app-service-app-to-an-azure-cache-for-redis-using-cli"></a>使用 CLI 将应用服务应用连接到 Azure Redis 缓存

此示例脚本创建一个 Azure Redis 缓存和一个应用服务应用。 然后，它使用应用设置将 Azure Redis 缓存链接到应用。
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，需要 Azure CLI 2.0 版或更高版本。 要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Variables
appName="webappwithredis$RANDOM"
location="chinanorth"

# Create a Resource Group 
az group create --name myResourceGroup --location $location

# Create an App Service Plan
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --location $location

# Create a Web App
az webapp create --name $appName --plan myAppServicePlan --resource-group myResourceGroup 

# Create a Redis Cache
redis=($(az redis create --name $appName --resource-group myResourceGroup \
--location $location --vm-size C0 --sku Basic --query [hostName,sslPort] --output tsv))

# Get access key
key=$(az redis list-keys --name $appName --resource-group myResourceGroup \
--query primaryKey --output tsv)

# Assign the connection string to an App Setting in the Web App
az webapp config appsettings set --name $appName --resource-group myResourceGroup \
 --settings "REDIS_URL=${redis[0]}" "REDIS_PORT=${redis[1]}" "REDIS_KEY=$key"
``` 
 
[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、应用服务应用、Azure Redis 缓存和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | 创建应用服务计划。 |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | 创建应用服务应用。 |
| [`az redis create`](https://docs.azure.cn/cli/redis#az_redis_create) | 创建新的用于 Redis 的 Azure 缓存实例。 |
| [`az redis list-keys`](https://docs.azure.cn/cli/redis#az_redis_list_keys) | 列出用于 Redis 的 Azure 缓存实例的访问密钥。 |
| [`az webapp config appsettings set`](https://docs.azure.cn/cli/webapp/config/appsettings#az_webapp_config_appsettings_set) | 创建或更新应用服务应用的应用设置。 应用设置将作为应用的环境变量公开。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli)。

可以在 [Azure 应用服务文档](../samples-cli.md)中找到其他应用服务 CLI 脚本示例。
