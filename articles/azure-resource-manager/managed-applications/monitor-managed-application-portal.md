---
title: 使用 Azure 门户监视托管应用
description: 显示如何使用 Azure 门户监视托管应用程序的可用性和警报。
ms.topic: conceptual
origin.date: 10/04/2018
author: rockboyfor
ms.date: 03/01/2021
ms.author: v-yeche
ms.openlocfilehash: 59f34465a5bb92125af60078941bc8209d8c893c
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102053014"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>监视部署的托管应用程序实例

将托管应用程序部署到 Azure 订阅后，可能需要检查应用程序的状态。 本文介绍 Azure 门户中用于检查状态的选项。 在托管应用程序中，可以监视资源的可用性。 还可以设置和查看警报。

## <a name="view-resource-health"></a>查看资源运行状况

1. 选择托管应用程序实例。

    :::image type="content" source="./media/monitor-managed-application-portal/select-managed-application.png" alt-text="创建托管应用程序":::

1. 选择“资源运行状况”。

    :::image type="content" source="./media/monitor-managed-application-portal/select-resource-health.png" alt-text="选择“资源运行状况”":::

1. 在托管应用程序中查看资源的可用性。

    :::image type="content" source="./media/monitor-managed-application-portal/view-health.png" alt-text="查看资源运行状况":::

## <a name="view-alerts"></a>查看警报

1. 选择“**警报**”。

    :::image type="content" source="./media/monitor-managed-application-portal/select-alerts.png" alt-text="选择“警报”":::

1. 如果已配置警报规则，则会看到有关已引发警报的信息。

    :::image type="content" source="./media/monitor-managed-application-portal/view-alerts.png" alt-text="查看警报":::

1. 若要添加警报规则，请选择“+ 新建警报规则”。

    :::image type="content" source="./media/monitor-managed-application-portal/create-new-alert.png" alt-text="创建警报":::

可以针对托管应用程序实例或托管应用程序中的资源创建警报。 有关创建警报的信息，请参阅 [Azure 中的警报概述](../../azure-monitor/platform/alerts-overview.md)。

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序示例，请参阅 [Azure 托管应用程序的示例项目](sample-projects.md)。
* 若要部署托管应用程序，请参阅[通过 Azure 门户部署服务目录应用](deploy-service-catalog-quickstart.md)。

<!--Update_Description: update meta properties, wording update, update link-->