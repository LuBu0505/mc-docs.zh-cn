---
title: 虚拟机规模集的邻近放置组预览版
description: 了解如何在 Azure 中为 Windows 虚拟机规模集创建和使用邻近放置组。
author: cynthn
ms.author: v-junlch
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 02/19/2021
ms.reviewer: zivr
ms.custom: mimckitt
ms.openlocfilehash: 1a82fa17035009bd4b742d9f75de93f4a6b8d56f
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697660"
---
# <a name="preview-creating-and-using-proximity-placement-groups-using-powershell"></a>预览版：使用 PowerShell 创建和使用邻近放置组

为了使 VM 尽可能接近，以尽可能降低延迟，应该在[邻近放置组](../virtual-machines/co-location.md#proximity-placement-groups)中部署规模集。

邻近放置组是一种逻辑分组，用于确保 Azure 计算资源的物理位置彼此接近。 邻近放置组适用于要求低延迟的工作负荷。

> [!IMPORTANT]
> 邻近放置组目前为公共预览版。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/)。
>

## <a name="create-a-proximity-placement-group"></a>创建邻近放置组
使用 [New-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) cmdlet 创建邻近放置组。 

```azurepowershell
$resourceGroup = "myPPGResourceGroup"
$location = "China North 2"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>列出邻近放置组

可以使用 [Get-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/get-azproximityplacementgroup) cmdlet 列出所有邻近放置组。

```azurepowershell
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>创建规模集

使用 [New-AzVMSS](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) 创建规模集时，可使用 `-ProximityPlacementGroup $ppg.Id` 引用邻近放置组 ID，以便在邻近放置组中创建规模集。

```azurepowershell
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

可以使用 [Get-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/get-azproximityplacementgroup) 查看放置组中的实例。

```azurepowershell
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>后续步骤

也可使用 [Azure CLI](../virtual-machines/linux/proximity-placement-groups.md) 创建邻近放置组。
