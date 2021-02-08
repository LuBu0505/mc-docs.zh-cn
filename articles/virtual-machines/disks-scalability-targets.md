---
title: VM 磁盘的可伸缩性和性能目标
description: 了解附加到 VM 的虚拟机磁盘的可伸缩性和性能目标。
origin.date: 01/15/2021
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: yes
ms.testdate: 02/01/2021
ms.author: v-yeche
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 374f3c9c990381245dcbbb3d715dda672785d484
ms.sourcegitcommit: 1107b0d16ac8b1ad66365d504c925735eb079d93
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99064542"
---
<!--Verified successfully from rename article-->
# <a name="scalability-and-performance-targets-for-vm-disks"></a>VM 磁盘的可伸缩性和性能目标

[!INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

请参阅 [VM 大小](sizes.md)了解其他详细信息。

## <a name="managed-virtual-machine-disks"></a>托管虚拟机磁盘

用星号表示的大小当前处于预览阶段。 请参阅我们的[常见问题解答](faq-for-disks.md#new-disk-sizes-managed-and-unmanaged)，了解它们可用的区域。

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>非托管虚拟机磁盘
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="see-also"></a>另请参阅

[Azure 订阅和服务限制、配额和约束](../azure-resource-manager/management/azure-subscription-service-limits.md)

<!-- Update_Description: new article about disks scalability targets -->
<!--NEW.date: 02/01/2021-->