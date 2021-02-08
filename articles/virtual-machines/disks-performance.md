---
title: 虚拟机和磁盘性能
description: 详细了解如何组合使用虚拟机及其附加的磁盘以提高性能。
origin.date: 10/12/2020
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: yes
ms.testdate: 02/01/2021
ms.author: v-yeche
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: d3cee2c167fb10f4a726703751053d6e8ceb373c
ms.sourcegitcommit: 1107b0d16ac8b1ad66365d504c925735eb079d93
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99064544"
---
<!--Verified successfully from rename article-->
# <a name="virtual-machine-and-disk-performance"></a>虚拟机和磁盘性能
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>虚拟机非缓存限制与缓存限制
同时启用了高级存储和高级存储缓存的虚拟机有两种不同的存储带宽限制。 让我们以 Standard_D8s_v3 虚拟机为例。 下面是有关 [Dsv3 系列](dv3-dsv3-series.md)和 Standard_D8s_v3 的文档：

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

让我们对可创建 IO 活动的此虚拟机和磁盘组合运行基准测试。 若要了解如何在 Azure 上对存储 IO 进行基准测试，请参阅[在 Azure 磁盘存储上对应用程序进行基准测试](disks-benchmarks.md)。 通过基准测试工具，可以看到 VM 和磁盘组合可以实现 22,800 个 IOPS：

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-3.md)]

<!-- Update_Description: new article about disks performance -->
<!--NEW.date: 02/01/2021-->