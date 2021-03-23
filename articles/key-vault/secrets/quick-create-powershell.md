---
title: 快速入门 - 使用 PowerShell 在 Key Vault 中设置和检索机密
description: 本快速入门介绍如何使用 Azure PowerShell 在 Azure Key Vault 中创建、检索和删除机密。
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc, devx-track-azurepowershell
ms.date: 03/10/2021
ms.author: v-chazhou
ms.openlocfilehash: 07ee35beb4335394c22f808a0489a8af0dd7aab8
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104765297"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-powershell"></a>快速入门：使用 PowerShell 在 Azure Key Vault 中设置和检索机密

Azure Key Vault 是一项云服务，用作安全的机密存储。 可以安全地存储密钥、密码、证书和其他机密。 有关 Key Vault 的详细信息，可以参阅[概述](../general/overview.md)。 在本快速入门中，请使用 PowerShell 创建一个密钥保管库， 然后即可在新创建的保管库中存储机密。

如果没有 Azure 订阅，请在开始前创建一个[试用订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

<!-- [!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)] -->

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 5.0.0 版或更高版本。 键入 `$PSVersionTable.PSVersion` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 如果在本地运行 PowerShell，则还需运行 `Login-AzAccount -EnvironmentName AzureChinaCloud` 来创建与 Azure 的连接。

```azurepowershell
Login-AzAccount -EnvironmentName AzureChinaCloud
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```azurepowershell
New-AzResourceGroup -Name ContosoResourceGroup -Location ChinaNorth
```

## <a name="create-a-key-vault"></a>创建 key vault

接下来创建 Key Vault。 执行此步骤时，需要一些信息：

虽然在本快速入门中我们始终使用“Contoso KeyVault2”作为密钥保管库的名称，但你必须使用唯一的名称。

- **保管库名称** Contoso-Vault2。
- 资源组名称 **ContosoResourceGroup**。
- **位置**：中国北部。

```azurepowershell
New-AzKeyVault -Name 'Contoso-Vault2' -ResourceGroupName 'ContosoResourceGroup' -Location 'China North'
```

此 cmdlet 的输出显示新创建的密钥保管库的属性。 请记下下面列出的两个属性：

* **保管库名称**：在本示例中为 **Contoso-Vault2**。 将在其他密钥保管库 cmdlet 中使用此名称。
* **保管库 URI**：在本示例中为 https://Contoso-Vault2.vault.azure.cn/ 。 通过其 REST API 使用保管库的应用程序必须使用此 URI。

创建保管库以后，你的 Azure 帐户是唯一能够对这个新的保管库执行任何操作的帐户。

## <a name="give-your-user-account-permissions-to-manage-secrets-in-key-vault"></a>向用户帐户授予管理 Key Vault 中的机密的权限

使用 Azure PowerShell Set-AzKeyVaultAccessPolicy cmdlet 来更新 Key Vault 访问策略并向用户帐户授予机密权限。

```azurepowershell
Set-AzKeyVaultAccessPolicy -VaultName 'Contoso-Vault2' -UserPrincipalName 'user@domain.com' -PermissionsToSecrets get,set,delete
```

## <a name="adding-a-secret-to-key-vault"></a>向 Key Vault 添加机密

只需执行几个步骤即可向保管库添加机密。 在此示例中，请添加可供应用程序使用的密码。 此密码名为 **ExamplePassword**，其中存储的值为 **hVFkk965BuUv**。

先键入以下内容，将 **hVFkk965BuUv** 值转换成安全字符串：

```azurepowershell
$secretvalue = ConvertTo-SecureString "hVFkk965BuUv" -AsPlainText -Force
```

然后键入以下 PowerShell 命令，在 Key Vault 中创建名为 **ExamplePassword** 且值为 **hVFkk965BuUv** 的机密：


```azurepowershell
$secret = Set-AzKeyVaultSecret -VaultName 'Contoso-Vault2' -Name 'ExamplePassword' -SecretValue $secretvalue
```

## <a name="retrieve-a-secret-from-key-vault"></a>从 Key Vault 检索机密

若要查看机密中包含的纯文本形式的值，请执行以下命令：

```azurepowershell
$secret = Get-AzKeyVaultSecret -VaultName 'Contoso-Vault2' -Name 'ExamplePassword'
$ssPtr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secret.SecretValue)
try {
   $secretValueText = [System.Runtime.InteropServices.Marshal]::PtrToStringBSTR($ssPtr)
} finally {
   [System.Runtime.InteropServices.Marshal]::ZeroFreeBSTR($ssPtr)
}
Write-Output $secretValueText
```

现在，你已创建 Key Vault 并存储和检索了机密。

## <a name="clean-up-resources"></a>清理资源

 本系列中的其他快速入门和教程是在本快速入门的基础上制作的。 如果打算继续使用其他快速入门和教程，则可能需要保留这些资源。

如果不再需要资源组、Key Vault 和所有相关的资源，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```azurepowershell
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你创建了 Key Vault 并在其中存储了一个机密。 若要详细了解 Key Vault 以及如何将其与应用程序集成，请继续阅读以下文章。

- 阅读 [Azure Key Vault 概述](../general/overview.md)
- 请参阅 [Azure PowerShell Key Vault cmdlet](https://docs.microsoft.com/powershell/module/az.keyvault/#key_vault) 参考
- 请参阅 [Key Vault 安全性概述](../general/security-overview.md)
