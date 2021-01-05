---
title: 使用 Azure CLI 创建和加密 Windows VM
description: 本快速入门介绍如何使用 Azure CLI 创建和加密 Windows 虚拟机
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: quickstart
origin.date: 05/17/2019
author: rockboyfor
ms.date: 01/04/2021
ms.testscope: yes
ms.testdate: 08/31/2020
ms.author: v-yeche
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8d9239cb16bc81891a01ba4dd293bcdcecefa390
ms.sourcegitcommit: b4fd26098461cb779b973c7592f951aad77351f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2021
ms.locfileid: "97857122"
---
# <a name="quickstart-create-and-encrypt-a-windows-vm-with-the-azure-cli"></a>快速入门：使用 Azure CLI 创建和加密 Windows VM

Azure CLI 用于从命令行或脚本创建和管理 Azure 资源。 本快速入门演示如何使用 Azure CLI 创建和加密 Windows Server 2016 虚拟机 (VM)。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- 本文需要 Azure CLI 版本 2.0.30 或更高版本。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](https://docs.azure.cn/cli/group#az_group_create) 命令创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。 以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组：

```azurecli
az group create --name myResourceGroup --location chinaeast
```

## <a name="create-a-virtual-machine"></a>创建虚拟机

使用 [az vm create](https://docs.azure.cn/cli/vm#az_vm_create) 创建 VM。 以下示例创建一个名为 myVM 的 VM。 此示例使用 azureuser 作为管理用户名，使用 myPassword12 作为密码。

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

创建 VM 和支持资源需要几分钟时间。 以下示例输出表明 VM 创建操作已成功。

```console
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "chinaeast",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>创建为加密密钥配置的密钥保管库

Azure 磁盘加密将其加密密钥存储在 Azure 密钥保管库中。 使用 [az keyvault create](https://docs.azure.cn/cli/keyvault#az_keyvault_create) 创建密钥保管库。 要使密钥保管库能够存储加密密钥，请使用 --enabled-for-disk-encryption 参数。
> [!Important]
> 每个密钥保管库必须具有唯一的名称。 以下示例创建名为“myKV”的密钥保管库，但你必须将其命名为不同的名称。

```azurecli
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location chinaeast --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>加密虚拟机

使用 [az vm encryption](https://docs.azure.cn/cli/vm/encryption) 加密 VM，为 --disk-encryption-keyvault 参数提供唯一的密钥保管库名称。

```azurecli
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

可以使用 [az vm show](https://docs.azure.cn/cli/vm/encryption#az_vm_encryption_show) 验证 VM 是否已启用加密

```azurecli
az vm encryption show --name MyVM -g MyResourceGroup
```

将在返回的输出中看到以下内容：

```console
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组、VM 和密钥保管库，可以使用 [az group delete](https://docs.azure.cn/cli/group#az-group-delete) 命令将其删除。

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你创建了一个虚拟机，创建了一个启用加密密钥的密钥保管库，并对 VM 进行了加密。  请继续学习下一篇文章，详细了解 IaaS VM 的 Azure 磁盘加密先决条件。

> [!div class="nextstepaction"]
> [Azure 磁盘加密概述](disk-encryption-overview.md)

<!-- Update_Description: update meta properties, wording update, update link -->