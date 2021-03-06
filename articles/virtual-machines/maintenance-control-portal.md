---
title: 使用 Azure 门户对 Azure 虚拟机进行维护控制
description: 了解如何使用维护控制和 Azure 门户来控制对 Azure VM 应用维护的时间。
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
origin.date: 04/22/2020
author: rockboyfor
ms.date: 01/04/2021
ms.testscope: no
ms.testdate: 01/04/2021
ms.author: v-yeche
ms.openlocfilehash: 8b5147c1f54cd146ec42053aba339a7c8fbadb21
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98229947"
---
<!--Confirmed successfully from PG team-->
# <a name="control-updates-with-maintenance-control-and-the-azure-portal"></a>使用维护控制和 Azure 门户来控制更新

维护控制允许你决定何时向独立 VM 和 Azure 专用主机应用更新。 本主题介绍了用于维护控制的 Azure 门户选项。 有关使用维护控制的好处、其限制和其他管理选项的详细信息，请参阅[使用维护控制管理平台更新](maintenance-control.md)。

## <a name="create-a-maintenance-configuration"></a>创建维护配置

1. 登录到 Azure 门户。

1. 搜索“维护配置”。

    :::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-search.png" alt-text="屏幕截图显示了如何打开“维护配置”":::

1. 单击“添加”。

    :::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-add.png" alt-text="屏幕截图显示了如何添加维护配置":::

1. 选择订阅和资源组，提供配置名称，然后选择区域。 单击“下一步” 。

    :::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-basics.png" alt-text="屏幕截图显示了维护配置基本信息":::

1. 添加标记和值。 单击“下一步” 。

    :::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-tags.png" alt-text="屏幕截图显示了如何向维护配置添加标记":::

1. 查看摘要。 单击“创建”。

    :::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-create.png" alt-text="屏幕截图显示了如何创建维护配置":::

1. 部署完成后，单击“转到资源”。

    :::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-deployment-complete.png" alt-text="屏幕截图显示了维护配置部署完成":::

## <a name="assign-the-configuration"></a>分配此配置

在维护配置的详细信息页面上，单击“分配”，然后单击“分配资源”。 

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-add-assignment.png" alt-text="屏幕截图显示了如何分配资源":::

选择要为其分配维护配置的资源，然后单击“确定”。 “类型”列显示资源是独立 VM 还是 Azure 专用主机。 VM 需要处于运行状态才能分配配置。 如果尝试将配置分配给已停止的 VM，则会出现错误。 

<!---Shantanu to add details about the error case--->

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-select-resource.png" alt-text="屏幕截图显示了如何选择资源":::

## <a name="check-configuration"></a>检查配置

你可以验证是否已正确应用了配置，或查看当前使用“维护配置”指定的任何维护配置。 “类型”列显示配置分配给独立 VM 还是 Azure 专用主机。 

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-host-type.png" alt-text="屏幕截图显示了如何检查维护配置":::

你还可以在特定虚拟机的“属性”页上检查其配置。 单击“维护”可查看分配给该虚拟机的配置。

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-check-config.png" alt-text="屏幕截图显示了如何检查主机的维护":::

## <a name="check-for-pending-updates"></a>检查是否有挂起的更新

还可以通过两种方式检查维护配置是否有挂起的更新。 在“维护配置”的配置详细信息中，单击“分配”并检查“维护状态”。

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-pending.png" alt-text="屏幕截图显示了如何检查挂起的更新":::

你还可以使用“虚拟机”或专用主机的属性来检查特定主机。 

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-pending-vm.png" alt-text="屏幕截图显示了突出显示的维护状态。":::

## <a name="apply-updates"></a>应用更新

你可以使用“虚拟机”按需应用挂起的更新。 在 VM 详细信息中单击“维护”，然后单击“立即应用维护”。

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-apply-updates-now.png" alt-text="屏幕截图显示了如何应用挂起的更新":::

## <a name="check-the-status-of-applying-updates"></a>检查应用更新的状态 

可以通过“维护配置”或“虚拟机”检查某个配置的更新进度。 在 VM 详细信息中，单击“维护”。 在下面的示例中，“维护状态”显示某个更新处于“挂起”状态。

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-status.png" alt-text="屏幕截图显示了如何检查挂起的更新的状态":::

## <a name="delete-a-maintenance-configuration"></a>删除维护配置

若要删除某个配置，请打开配置详细信息，然后单击“删除”。

:::image type="content" source="media/virtual-machines-maintenance-control-portal/maintenance-configurations-delete.png" alt-text="屏幕截图显示了如何删配置。":::

## <a name="next-steps"></a>后续步骤

若要了解详细信息，请参阅[维护和更新](maintenance-and-updates.md)。

<!-- Update_Description: new article about maintenance control portal -->
<!--NEW.date: 01/04/2021-->