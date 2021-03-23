---
title: 为 Azure 托管磁盘启用共享磁盘
description: 为 Azure 托管磁盘配置共享磁盘，以便可以跨多个 VM 共享它
ms.service: virtual-machines
ms.topic: how-to
origin.date: 09/30/2020
author: rockboyfor
ms.date: 03/29/2021
ms.testscope: yes
ms.testdate: 11/09/2020
ms.author: v-yeche
ms.subservice: disks
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 7fba79ada206d99dc0231a4cbb68a9af16dd8610
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104765671"
---
<!--Verified successfully for PG notification-->
# <a name="enable-shared-disk"></a>启用共享磁盘

本文介绍了如何为 Azure 托管磁盘启用共享磁盘功能。 Azure 共享磁盘是 Azure 托管磁盘的一项新功能，可同时将托管磁盘附加到多个虚拟机 (VM)。 通过将托管磁盘附加到多个 VM，可以向 Azure 部署新的群集应用程序或迁移现有的群集应用程序。 

如果你正在查找有关已启用共享磁盘的托管磁盘的概念信息，请参阅 [Azure 共享磁盘](disks-shared.md)。

## <a name="limitations"></a>限制

[!INCLUDE [virtual-machines-disks-shared-limitations](../../includes/virtual-machines-disks-shared-limitations.md)]

## <a name="supported-operating-systems"></a>支持的操作系统

共享磁盘支持多个操作系统。 有关支持的操作系统，请参阅概念文章的 [Windows](./disks-shared.md#windows) 和 [Linux](./disks-shared.md#linux) 部分。

## <a name="disk-sizes"></a><a name="disk-sizes"></a>磁盘大小

[!INCLUDE [virtual-machines-disks-shared-sizes](../../includes/virtual-machines-disks-shared-sizes.md)]

## <a name="deploy-shared-disks"></a>部署共享磁盘

### <a name="deploy-a-premium-ssd-as-a-shared-disk"></a>将高级 SSD 部署为共享磁盘

若要部署启用了共享磁盘功能的托管磁盘，请使用新属性 `maxShares` 并定义大于 1 的值。 这会使该磁盘可在多个 VM 之间共享。

> [!IMPORTANT]
> 仅当从所有 VM 中卸载了某个磁盘时，才能设置或更改 `maxShares` 的值。 有关 `maxShares` 的允许值，请参阅[磁盘大小](#disk-sizes)。

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az disk create -g myResourceGroup -n mySharedDisk --size-gb 1024 -l chinaeast --sku Premium_LRS --max-shares 2
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
$dataDiskConfig = New-AzDiskConfig -Location 'chinanorth' -DiskSizeGB 1024 -AccountType Premium_LRS -CreateOption Empty -MaxSharesCount 2

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $dataDiskConfig
```

# <a name="resource-manager-template"></a>[资源管理器模板](#tab/azure-resource-manager)

使用以下模板之前，请使用你自己的值替换 `[parameters('dataDiskName')]`、`[resourceGroup().location]`、`[parameters('dataDiskSizeGB')]` 和 `[parameters('maxShares')]`。

[高级 SSD 共享磁盘模板](https://aka.ms/SharedPremiumDiskARMtemplate)

---

<!--NOT AVAILABLE ON ### Deploy an ultra disk as a shared disk-->


## <a name="using-azure-shared-disks-with-your-vms"></a>将 Azure 共享磁盘与 VM 配合使用

使用 `maxShares>1` 部署共享磁盘后，可以将该磁盘装载到一个或多个 VM。

<!--NOT AVAILABLE on FEATURE ultra disk-->

```powershell

$resourceGroup = "myResourceGroup"
$location = "chinanorth"

$vm = New-AzVm -ResourceGroupName $resourceGroup -Name "myVM" -Location $location -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress"

$dataDisk = Get-AzDisk -ResourceGroupName $resourceGroup -DiskName "mySharedDisk"

$vm = Add-AzVMDataDisk -VM $vm -Name "mySharedDisk" -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 0

update-AzVm -VM $vm -ResourceGroupName $resourceGroup
```

## <a name="supported-scsi-pr-commands"></a>支持的 SCSI PR 命令

将共享磁盘装载到群集中的 VM 后，可以使用 SCSI PR 建立仲裁以及在磁盘中进行读取/写入操作。 使用 Azure 共享磁盘时，可以使用以下 PR 命令：

若要与磁盘进行交互，请从 persistent-reservation-action 列表开始：

```
PR_REGISTER_KEY 

PR_REGISTER_AND_IGNORE 

PR_GET_CONFIGURATION 

PR_RESERVE 

PR_PREEMPT_RESERVATION 

PR_CLEAR_RESERVATION 

PR_RELEASE_RESERVATION 
```

使用 PR_RESERVE、PR_PREEMPT_RESERVATION 或 PR_RELEASE_RESERVATION 时，请提供下列 persistent-reservation-type 之一：

```
PR_NONE 

PR_WRITE_EXCLUSIVE 

PR_EXCLUSIVE_ACCESS 

PR_WRITE_EXCLUSIVE_REGISTRANTS_ONLY 

PR_EXCLUSIVE_ACCESS_REGISTRANTS_ONLY 

PR_WRITE_EXCLUSIVE_ALL_REGISTRANTS 

PR_EXCLUSIVE_ACCESS_ALL_REGISTRANTS 
```

使用 PR_RESERVE、PR_REGISTER_AND_IGNORE、PR_REGISTER_KEY、PR_PREEMPT_RESERVATION、PR_CLEAR_RESERVATION 或 PR_RELEASE-RESERVATION 时，还需要提供 persistent-reservation-key。

## <a name="next-steps"></a>后续步骤

如果希望使用 Azure 资源管理器模板来部署磁盘，可使用以下示例模板：
- [高级·SSD](https://aka.ms/SharedPremiumDiskARMtemplate)

<!--NOT AVAILABLE on FEATURE ultra disks-->
<!--NOT AVAILABLE on [Regional ultra disks](https://aka.ms/SharedUltraDiskARMtemplateRegional)-->
<!--NOT AVAILABLE on [Zonal ultra disks](https://aka.ms/SharedUltraDiskARMtemplateZonal)-->
<!--Update_Description: update meta properties, wording update, update link-->