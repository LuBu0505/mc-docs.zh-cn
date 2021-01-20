---
title: 使用 VNet 对等互连连接虚拟网络 - Azure CLI
description: 在本文中，你将学习如何使用 Azure CLI 通过虚拟网络对等互连来连接虚拟网络。
services: virtual-network
documentationcenter: virtual-network
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
origin.date: 03/13/2018
author: rockboyfor
ms.date: 01/18/2021
ms.testscope: yes
ms.testdate: 08/10/2020
ms.author: v-yeche
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0983c44ca6ff5d9ea86f7241392ddfda6da01200
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98230212"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>通过 Azure CLI 使用虚拟网络对等互连连接虚拟网络

可以使用虚拟网络对等互连将虚拟网络互相连接。 将虚拟网络对等互连后，两个虚拟网络中的资源将能够以相同的延迟和带宽相互通信，就像这些资源位于同一个虚拟网络中一样。 在本文中，学习如何：

* 创建两个虚拟网络
* 使用虚拟网络对等互连连接两个虚拟网络。
* 将虚拟机 (VM) 部署到每个虚拟网络
* VM 之间进行通信

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- 本文需要 Azure CLI 2.0.28 或更高版本。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="create-virtual-networks"></a>创建虚拟网络

创建虚拟网络之前，必须为虚拟网络创建资源组以及本文中创建的所有其他资源。 使用 [az group create](https://docs.azure.cn/cli/group#az-group-create) 创建资源组。 以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组。

```azurecli 
az group create --name myResourceGroup --location chinaeast
```

使用 [az network vnet create](https://docs.azure.cn/cli/network/vnet#az-network-vnet-create) 创建虚拟网络。 以下示例创建地址前缀为 *10.0.0.0/16* 且名为 *myVirtualNetwork1* 的虚拟网络。

```azurecli 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

使用地址前缀 *10.1.0.0/16* 创建一个名为 *myVirtualNetwork2* 的虚拟网络：

```azurecli 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>将虚拟网络对等互连

对等互连是在虚拟网络 ID 之间建立的，因此必须先使用 [az network vnet show](https://docs.azure.cn/cli/network/vnet#az-network-vnet-show) 获取每个虚拟网络的 ID，并将 ID 存储在变量中。

```azurecli
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

使用 [az network vnet peering create](https://docs.azure.cn/cli/network/vnet/peering#az-network-vnet-peering-create) 创建从 *myVirtualNetwork1* 到 *myVirtualNetwork2* 的对等互连。 如果未指定 `--allow-vnet-access` 参数，将建立对等互连，但没有通信可以通过它进行。

```azurecli
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet $vNet2Id \
  --allow-vnet-access
```

在前一个命令执行后返回的输出中，可以看到 **peeringState** 为“已启动”。 对等互连将保持 *Initiated* 状态，直到你创建从 *myVirtualNetwork2* 到 *myVirtualNetwork1* 的对等互连。 创建从 *myVirtualNetwork2* 到 *myVirtualNetwork1* 的对等互连。 

```azurecli
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet $vNet1Id \
  --allow-vnet-access
```

在前一个命令执行后返回的输出中，可以看到 **peeringState** 为“已连接”。 Azure 还将 *myVirtualNetwork1-myVirtualNetwork2* 对等互连的对等互连状态更改为 *Connected*。 使用 [az network vnet peering show](https://docs.azure.cn/cli/network/vnet/peering#az-network-vnet-peering-show) 确认 *myVirtualNetwork1-myVirtualNetwork2* 对等互连的对等互连状态是否已更改为“已连接”。

```azurecli
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

在两个虚拟网络中的对等互连的 **peeringState** 为“已连接”之前，在一个虚拟网络中的资源无法与另一个虚拟网络中的资源通信。 

## <a name="create-virtual-machines"></a>创建虚拟机

在稍后的步骤中，会在每个虚拟网络中创建一个 VM，以便可以在它们之间进行通信。

### <a name="create-the-first-vm"></a>创建第一个 VM

使用 [az vm create](https://docs.azure.cn/cli/vm#az-vm-create) 创建 VM。 以下示例在 *myVirtualNetwork1* 虚拟网络中创建一个名为 *myVm1* 的 VM。 如果默认密钥位置中尚不存在 SSH 密钥，该命令会创建它们。 若要使用特定的一组密钥，请使用 `--ssh-key-value` 选项。 `--no-wait` 选项会在后台创建 VM，因此可继续执行下一步。

```azurecli
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>创建第二个 VM

在 *myVirtualNetwork2* 虚拟网络中创建 VM。

```azurecli
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

创建 VM 需要几分钟时间。 创建 VM 之后，Azure CLI 将显示类似于以下示例的信息： 

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "chinaeast",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

记下 publicIpAddress。 在后面的步骤中会使用此地址通过 Internet 访问 VM。

## <a name="communicate-between-vms"></a>VM 之间进行通信

使用以下命令来与 *myVm2* VM 建立 SSH 会话。 将 `<publicIpAddress>` 替换为 VM 的公共 IP 地址。 在前一示例中，公共 IP 地址为 *13.90.242.231*。

```bash
ssh <publicIpAddress>
```

对 *myVirtualNetwork1* 中的 VM 执行 Ping 操作。

```bash
ping 10.0.0.4 -c 4
```

会收到四条回复。 

关闭与 *myVm2* VM 的 SSH 会话。 

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组及其包含的所有资源，可以使用 [az group delete](https://docs.azure.cn/cli/group#az-group-delete) 将其删除。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

在本文中，你已学习了如何使用虚拟网络对等互连来连接同一 Azure 区域中的两个网络。 还可以将不同的[受支持区域](virtual-network-manage-peering.md#cross-region)和[不同的 Azure 订阅](create-peering-different-subscriptions.md#cli)中的虚拟网络对等互连。 若要详细了解虚拟网络对等互连，请参阅[虚拟网络对等互连概述](virtual-network-peering-overview.md)和[管理虚拟网络对等互连](virtual-network-manage-peering.md)。

<!--Not Available on [hub and spoke network designs](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fvirtual-network%2ftoc.json#vnet-peering)-->

可以通过 VPN [将自己的计算机连接到虚拟网络](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fvirtual-network%2ftoc.json)，并可与虚拟网络或对等虚拟网络中的资源进行交互。 有关用来完成虚拟网络文章中涉及的许多任务的可重用脚本，请参阅[脚本示例](cli-samples.md)。

<!-- Update_Description: update meta properties, wording update, update link -->