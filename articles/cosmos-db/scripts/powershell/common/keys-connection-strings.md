---
title: PowerShell 脚本：获取 Azure Cosmos DB 帐户的密钥和连接字符串操作
description: Azure PowerShell 脚本示例 - Azure Cosmos DB 帐户的帐户密钥和连接字符串操作
ms.service: cosmos-db
ms.topic: sample
origin.date: 03/18/2020
author: rockboyfor
ms.date: 02/08/2021
ms.testscope: yes|no
ms.testdate: 08/17/2020null
ms.author: v-yeche
ms.openlocfilehash: 7b0a3ce5f0cd55d21a12921fb5abb537a598686e
ms.sourcegitcommit: 0232a4d5c760d776371cee66b1a116f6a5c850a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99580481"
---
<!--Verified successfully-->
# <a name="connection-string-and-account-key-operations-for-an-azure-cosmos-db-account-using-powershell"></a>使用 PowerShell 的 Azure Cosmos DB 帐户的连接字符串和帐户密钥操作
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

本示例需要 Azure PowerShell Az 5.4.0 或更高版本。 运行 `Get-Module -ListAvailable Az`，查看已安装哪些版本。
如果需要安装，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

运行 [Connect-AzAccount -Environment AzureChinaCloud](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) 以登录到 Azure。

## <a name="sample-script"></a>示例脚本

> [!NOTE]
> 此示例演示如何使用 SQL API 帐户。 若要将此示例用于其他 API，请复制相关属性，并将其应用于 API 特定的脚本

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# List an account's connection strings and keys; regenerate a key.
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "myaccount" # Must be all lower case
$keyKind = "primary" # Other key kinds: secondary, primaryReadOnly, secondaryReadOnly
# --------------------------------------------------

Write-Host "List connection strings"
Get-AzCosmosDBAccountKey -ResourceGroupName $resourceGroupName `
    -Name $accountName -Type "ConnectionStrings"

Write-Host "List keys"
Get-AzCosmosDBAccountKey -ResourceGroupName $resourceGroupName `
    -Name $accountName -Type "Keys"

Write-Host "Reset key"
New-AzCosmosDBAccountKey  -ResourceGroupName $resourceGroupName `
    -Name $accountName -KeyKind $keyKind

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBAccountKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccountkey) | 获取 Cosmos DB 帐户的连接字符串或密钥（读写或只读）。 |
| [New-AzCosmosDBAccountKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccountkey) | 为 Cosmos DB 帐户重新生成指定的密钥。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

<!--Update_Description: update meta properties, wording update, update link-->