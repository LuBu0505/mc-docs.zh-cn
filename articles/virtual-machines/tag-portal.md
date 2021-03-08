---
title: 如何使用 Azure 门户标记 VM
description: 了解如何使用 Azure 门户标记虚拟机。
ms.topic: how-to
ms.workload: infrastructure-services
ms.service: virtual-machines
origin.date: 11/11/2020
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 11/30/2020
ms.author: v-yeche
ms.openlocfilehash: e444104505fb867885b20a665b368aeff6a113fb
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054374"
---
<!--Verified successfully-->
# <a name="tagging-a-vm-using-the-portal"></a>使用门户标记 VM

本文介绍如何使用门户将标记添加到 VM。 标记是用户定义的键/值对，可直接放置在资源或资源组中。 针对每个资源和资源组，Azure 当前支持最多 50 个标记。 标记可以在创建时放置在资源中或添加到现有资源中。

1. 在门户中导航到你的 VM。
1. 在“概要”中，选择“单击此处以添加标记”。

    :::image type="content" source="media/tag/azure-portal-tag.png" alt-text="VM 页的“概要”部分的屏幕截图。":::

1. 为“名称”和“值”添加值，然后选择“保存”。

    :::image type="content" source="media/tag/key-value.png" alt-text="用于将键值对添加为标记的页的屏幕截图。":::

### <a name="next-steps"></a>后续步骤

- 若要详细了解如何标记 Azure 资源，请参阅 [Azure 资源管理器概述](../azure-resource-manager/management/overview.md)和 [使用标记来组织 Azure 资源](../azure-resource-manager/management/tag-resources.md)。

<!--NOT AVAILABLE ON [Understanding your Azure Bill](../cost-management-billing/understand/review-individual-bill.md)-->
<!--Update_Description: update meta properties, wording update, update link-->