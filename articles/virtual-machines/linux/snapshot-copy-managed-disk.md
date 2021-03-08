---
title: 使用 Azure CLI 创建 VHD 快照 | Azure
description: 了解如何在 Azure 中创建 VHD 的副本作为备份或用于解决问题。
author: Johnnytechn
manager: twooley
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 03/04/2021
ms.author: v-johya
ms.subservice: disks
ms.openlocfilehash: 4235992b95d1994619bb7dae1f824d4978364a9c
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197651"
---
# <a name="create-a-snapshot-using-the-portal-or-azure-cli"></a>使用门户或 Azure CLI 创建快照

创建 OS 或数据磁盘的快照作为备份，或用于解决 VM 问题。 快照是 VHD 的完整只读副本。 

## <a name="use-azure-cli"></a>使用 Azure CLI 

以下示例需要已安装 Azure CLI。

<!-- Not Available on [Cloud Shell](https://shell.azure.com/bash)-->

以下步骤说明如何使用带有 **--source-disk** 参数的 **az snapshot create** 命令创建快照。 以下示例假设 *myResourceGroup* 资源组中存在名为 *myVM* 的 VM。

使用 [az vm show](/cli/vm#az-vm-show) 获取磁盘 ID。

```azurecli
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

使用 [az snapshot create](/cli/snapshot#az-snapshot-create) 创建名为 *osDisk-backup* 的快照。

```azurecli
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

<!-- Not Available on Availability zones -->

可以使用 [az snapshot list](/cli/snapshot#az-snapshot-list) 查看快照列表。

```azurecli
az snapshot list \
   -g myResourceGroup \
   -o table
```
<!--Notice: global cmdlet missing -o-->

## <a name="use-azure-portal"></a>使用 Azure 门户 

1. 登录 [Azure 门户](https://portal.azure.cn)。
2. 首先在左上角单击“创建资源”并搜索“快照”。 从搜索结果中选择“快照”。
3. 在“快照”边栏选项卡中，单击“创建” 。
4. 输入快照的 **名称** 。
5. 选择现有的资源组，或键入新资源组的名称。 
7. 对于 **源磁盘**，选择要获取其快照的托管磁盘。
8. 选择用于存储快照的“帐户类型”。 使用 **Standard HDD**，除非需要将其存储在高性能 SSD 上。
9. 单击“创建”。


## <a name="next-steps"></a>后续步骤

 通过从快照创建托管磁盘，然后将新的托管磁盘附加为 OS 磁盘来从快照创建虚拟机。 有关详细信息，请参阅[从快照创建 VM](https://docs.microsoft.com/previous-versions/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot?toc=%2fcli%2fmodule%2ftoc.json) 脚本。

<!-- Update_Description: update link, wording update, update meta properties -->
