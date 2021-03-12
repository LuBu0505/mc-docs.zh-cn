---
title: 托管磁盘突发
description: 了解 Azure 磁盘和 Azure 虚拟机的磁盘突发。
origin.date: 01/27/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: 11/16/2020
ms.author: v-yeche
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 94674ad252f0679a718af9e5d0bc9f1fa41d96e6
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055223"
---
<!--Verified successfully from renamed articles-->
<!--CONFIRM THE DEVELOPMENT REGIONS BEFORE RELEASEMENT-->
# <a name="managed-disk-bursting"></a>托管磁盘突发
[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>虚拟机级突发
在以下支持 VM 系列的所有区域中启用了 VM 级别突发：

<!--NOT AVAILABLE ON lsv2-series.md-->

- [Dsv3 系列](dv3-dsv3-series.md)
- [Esv3 系列](ev3-esv3-series.md)

默认情况下会为支持突发的虚拟机启用突发。

## <a name="disk-level-bursting"></a>磁盘级别突发
对于中国云的所有区域中大小为 P20 和更小的磁盘，在我们的[高级 SSD](disks-types.md#premium-ssd) 上也提供了突发功能。 在支持磁盘突发的磁盘大小的所有新的和现有的部署上，默认已启用磁盘突发。 

[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting-2.md)]

## <a name="next-steps"></a>后续步骤

若要了解如何深入了解突发资源，请参阅[磁盘突发指标](disks-metrics.md)

<!--Update_Description: update meta properties, wording update, update link-->