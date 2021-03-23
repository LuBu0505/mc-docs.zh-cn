---
title: include 文件
description: include 文件
services: virtual-machines
ms.service: virtual-machines
ms.topic: include
origin.date: 04/02/2020
author: rockboyfor
ms.date: 03/22/2021
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 0caee800eca46bb8802a5d900934861bb54ce9d1
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766126"
---
<!--Verified successfully by PG team-->
1. 登录到 [Azure 门户](https://portal.azure.cn/)并导航到要拍摄快照的磁盘。
1. 在磁盘上，选择“创建快照”

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="屏幕截图。磁盘的边栏选项卡，其中突出显示了“+创建快照”****，因为这是必选的。":::

1. 选择要使用的资源组并输入名称。
1. 选择“增量”，然后选择“查看 + 创建” 

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="屏幕截图。创建快照边栏选项卡，填写名称并选择增量，然后创建快照。":::

1. 选择“创建”

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="屏幕截图。验证快照的验证页，确认所做的选择，然后创建快照。":::

<!-- Update_Description: update meta properties, wording update, update link -->