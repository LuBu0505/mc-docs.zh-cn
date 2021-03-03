---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/26/2021
ms.author: v-junlch
ms.openlocfilehash: 322f7973e0a0574695c635bdb1e350e578af1c38
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750872"
---
## <a name="create-supporting-azure-resources-for-your-function"></a>创建函数的支持性 Azure 资源

在将函数代码部署到 Azure 之前，需要创建三个资源：

- 一个[资源组](../articles/azure-resource-manager/management/overview.md)：相关资源的逻辑容器。
- 一个[存储帐户](../articles/storage/common/storage-account-create.md)：用于维护有关函数的状态和其他信息。
- 一个函数应用：提供用于执行函数代码的环境。 函数应用映射到本地函数项目，可让你将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。

使用以下命令创建这些项。 支持 Azure CLI 和 PowerShell。

1. 请登录到 Azure（如果尚未这样做）：

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
    ```azurecli
    az login
    ```

    使用 [az login](/cli/reference-index#az_login) 命令登录到 Azure 帐户。

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell) 
    ```azurepowershell
    Connect-AzAccount -Environment AzureChinaCloud
    ```

    使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 登录到 Azure 帐户。

    ---

1. 在 `chinanorth2` 区域中创建名为 `AzureFunctionsQuickstart-rg` 的资源组：

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
    
    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location chinanorth2
    ```
 
    [az group create](/cli/group#az_group_create) 命令可创建资源组。 通常，你会在从 `az account list-locations` 命令返回的、离你近的某个可用区域中创建资源组和资源。

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

    ```azurepowershell
    New-AzResourceGroup -Name AzureFunctionsQuickstart-rg -Location chinanorth2
    ```

    [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令可创建资源组。 通常，你会在从 [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) cmdlet 返回的、离你近的某个可用区域中创建资源组和资源。

    ---

1. 在资源组和区域中创建常规用途存储帐户：

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

    ```azurecli
    az storage account create --name <STORAGE_NAME> --location chinanorth2 --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```

    [az storage account create](/cli/storage/account#az_storage_account_create) 命令可创建存储帐户。 

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

    ```azurepowershell
    New-AzStorageAccount -ResourceGroupName AzureFunctionsQuickstart-rg -Name <STORAGE_NAME> -SkuName Standard_LRS -Location chinanorth2
    ```

    [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet 可创建存储帐户。

    ---

    在上一个示例中，将 `<STORAGE_NAME>` 替换为适合你且在 Azure 存储中唯一的名称。 名称只能包含 3 到 24 个数字和小写字母字符。 `Standard_LRS` 指定 [Functions 支持](../articles/azure-functions/storage-considerations.md#storage-account-requirements)的常规用途帐户。
