---
title: 虚拟机和磁盘性能
description: 详细了解如何组合使用虚拟机及其附加的磁盘以提高性能。
origin.date: 10/12/2020
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 02/08/2021
ms.author: v-yeche
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: b090f65b9ceef27421d2b9ca21182510209b5b9f
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054389"
---
<!--Verified successfully from rename article-->
# <a name="virtual-machine-and-disk-performance"></a>虚拟机和磁盘性能
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>虚拟机非缓存限制与缓存限制
同时启用了高级存储和高级存储缓存的虚拟机有两种不同的存储带宽限制。 让我们以 Standard_D8s_v3 虚拟机为例。 下面是有关 [Dsv3 系列](dv3-dsv3-series.md)和 Standard_D8s_v3 的文档：

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

<!--Update_Description: update meta properties, wording update, update link-->