---
title: 在 Azure Key Vault 中创建和检索密钥的属性 - Azure PowerShell
description: 本快速入门展示了如何使用 Azure PowerShell 在 Azure Key Vault 中设置和检索密钥
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
origin.date: 03/30/2020
ms.date: 11/27/2020
ms.author: v-tawe
ms.openlocfilehash: 73b87cd4dc176f4176e4832469d3beb689059050
ms.sourcegitcommit: 87b6bb293f39c5cfc2db6f38547220a13816d78f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96431059"
---
# <a name="quickstart-set-and-retrieve-a-key-from-azure-key-vault-using-azure-powershell"></a>快速入门：使用 Azure PowerShell 在 Azure Key Vault 中设置和检索密钥

在本快速入门中，你将使用 Azure PowerShell 在 Azure Key Vault 中创建一个密钥保管库。 Azure Key Vault 是一项云服务，用作安全的机密存储。 可以安全地存储密钥、密码、证书和其他机密。 有关 Key Vault 的详细信息，可以参阅[概述](../general/overview.md)。 Azure PowerShell 用于通过命令或脚本创建和管理 Azure 资源。 完成该操作后，你将存储密钥。

如果没有 Azure 订阅，请在开始前创建一个[试用订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

<!-- [!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)] -->

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 1.0.0 或更高版本。 键入 `$PSVersionTable.PSVersion` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。 如果在本地运行 PowerShell，则还需运行 `Login-AzAccount -EnvironmentName AzureChinaCloud` 来创建与 Azure 的连接。

```azurepowershell
Login-AzAccount -EnvironmentName AzureChinaCloud
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```azurepowershell
New-AzResourceGroup -Name ContosoResourceGroup -Location ChinaNorth
```

## <a name="create-a-key-vault"></a>创建密钥保管库

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

## <a name="add-a-key-to-key-vault"></a>向密钥保管库中添加密钥

只需再执行几个步骤便可向保管库中添加密钥。 此密钥可供应用程序使用。 

键入以下命令，创建名为 ExampleKey  的密钥：

```azurepowershell
Add-AzKeyVaultKey -VaultName 'Contoso-Vault2' -Name 'ExampleKey' -Destination 'Software'
```

现在，可以通过 URI 来引用已添加到 Azure Key Vault 的此密钥。 使用“https://Contoso-Vault2.vault.azure.cn/keys/ExampleKey”获取当前版本。 

若要查看以前存储的密钥，请使用以下命令：

```azurepowershell
Get-AzKeyVaultKey -VaultName 'Contoso-Vault2' -KeyName 'ExampleKey'
```

现在，你已创建了一个密钥保管库，存储了一个密钥并检索了该密钥。

## <a name="clean-up-resources"></a>清理资源

本系列中的其他快速入门和教程是在本快速入门的基础上制作的。 如果打算继续使用后续的快速入门和教程，则可能需要保留这些资源。
如果不再需要资源组和所有相关资源，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。 可以删除资源，如下所示：

```azurepowershell
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你创建了一个密钥保管库并在其中存储了一个证书。 若要详细了解 Key Vault 以及如何将其与应用程序集成，请继续阅读以下文章。

- 阅读 [Azure Key Vault 概述](../general/overview.md)
- 请参阅 [Azure PowerShell Key Vault cmdlet](https://docs.microsoft.com/powershell/module/az.keyvault/) 参考
- 查看 [Azure Key Vault 最佳做法](../general/best-practices.md)
