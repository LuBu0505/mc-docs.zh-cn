---
title: Azure CLI 脚本示例 - 轮换存储帐户访问密钥 | Microsoft Docs
description: 创建 Azure 存储帐户，然后检索并轮换其帐户访问密钥。
services: storage
author: WenJason
ms.service: storage
ms.subservice: blobs
ms.devlang: cli
ms.topic: sample
origin.date: 10/20/2020
ms.date: 01/18/2021
ms.author: v-jay
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4eda414587e13ba190d90fc81bfd2cda132a752d
ms.sourcegitcommit: f086abe8bd2770ed10a4842fa0c78b68dbcdf771
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98163066"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>创建存储帐户并轮换其帐户访问密钥

此脚本创建一个 Azure 存储帐户，显示新存储帐户的访问密钥，然后更新（轮换）密钥。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create a resource group
az group create --name myResourceGroup --location chinaeast

# Create a general-purpose standard storage account
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location chinaeast \
    --sku Standard_RAGRS \
    --encryption-services blob

# List the storage account access keys
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount 

# Renew (rotate) the PRIMARY access key
az storage account keys renew \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --key primary

# Renew (rotate) the SECONDARY access key
az storage account keys renew \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --key secondary
```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、存储帐户和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建存储帐户并检索和轮换其访问密钥。 表中的每一项均链接到命令特定的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group) | 创建用于存储所有资源的资源组。 |
| [az storage account create](https://docs.azure.cn/cli/storage/account) | 在指定资源组中创建 Azure 存储帐户。 |
| [az storage account keys list](https://docs.azure.cn/cli/storage/account/keys) | 显示指定帐户的存储帐户访问密钥。 |
| [az storage account keys renew](https://docs.azure.cn/cli/storage/account/keys) | 重新生成主或辅助存储帐户访问密钥。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

有关其他存储 CLI 脚本示例，可参阅 [Azure Blob 存储的 Azure CLI 示例](../blobs/storage-samples-blobs-cli.md)。
