---
title: Azure Monitor 资源组见解 |Microsoft Docs
description: 通过 Azure Monitor 了解资源组级别的分布式应用程序和服务的运行状况和性能
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
origin.date: 09/19/2018
ms.date: 03/24/2021
ms.reviewer: mbullwin
ms.openlocfilehash: d736ea96f07e569de5c7227cdff25009dd7291c8
ms.sourcegitcommit: 1a64114f25dd71acba843bd7f1cd00c4df737ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105603099"
---
# <a name="monitor-resource-groups-with-azure-monitor-preview"></a>使用 Azure Monitor（预览版）监视资源组

现代应用程序通常很复杂且分布广泛，许多独立部件协同工作来提供服务。 Azure Monitor 考虑到这种复杂性，为资源组提供监视见解。 因此，可以轻松分类和诊断各资源出现的任何问题，同时提供有关资源组和应用程序的运行状况和性能的整体上下文&mdash;&mdash;。

## <a name="access-insights-for-resource-groups"></a>资源组的访问见解

1. 在左侧的导航栏中选择“资源组”。
2. 选择其中一个要浏览的资源组。 （如果拥有大量资源组，则按订阅筛选有时会很有用。）
3. 若要访问资源组的见解，单击任意资源组的左侧菜单中的“见解”。

![资源组见解概述页的屏幕截图](./media/resource-group-insights/0001-overview.png)

## <a name="resources-with-active-alerts-and-health-issues"></a>具有活动警报和运行状况问题的资源

概述页显示已触发且仍处于活动状态的警报数以及每个资源的当前 Azure 资源运行状况。 这些信息有助于快速发现出现问题的任何资源。 警报可帮助检测代码中的问题以及配置基础结构的方式。 Azure 资源运行状况显示 Azure 平台本身的问题，这些问题并非特定于单个应用程序。

![Azure 资源运行状况窗格的屏幕截图](./media/resource-group-insights/0002-overview.png)

### <a name="azure-resource-health"></a>Azure 资源运行状况

若要显示 Azure 资源运行状况，选中表格上方的“显示 Azure 资源运行状况”。 默认情况下隐藏此列，以快速加载页面。

![添加了资源运行状况图的屏幕截图](./media/resource-group-insights/0003-overview.png)

默认情况下，按照应用层和资源类型对资源进行分组。 应用层是资源类型的简单分类，仅存在于资源组见解概述页的上下文中。 存在与应用程序代码、计算基础结构、网络、存储 + 数据库相关的资源类型。 管理工具具有自己的应用层，每个其他资源都归类为属于“其他”应用层。 此分组可以帮助快速查看应用程序的哪些子系统运行正常，哪些子系统运行不正常。

<!--Not available in MC: resource group insights page-->
## <a name="troubleshooting"></a>疑难解答

### <a name="enabling-access-to-alerts"></a>启用对警报的访问

若要在适用于资源组的 Azure Monitor 中查看警报，需要由此订阅中具有“所有者”或“参与者”角色的某人为订阅中的任何资源组打开适用于资源组的 Azure Monitor。 这会使得具有读取访问权限的任何人都能够在适用于资源组的 Azure Monitor 中查看针对该订阅中所有资源组的警报。 如果你拥有“所有者”或“参与者”角色，请在几分钟后刷新此页面。

适用于资源组的 Azure Monitor 依赖于 Azure Monitor 警报管理系统来检索警报状态。 默认情况下没有为每个资源组和订阅配置“警报管理”，它只能由具有“所有者”或“参与者”角色的某人来启用。 可以通过以下任一方式启用它：
* 为订阅中的任何资源组打开适用于资源组的 Azure Monitor。
* 或者，转到订阅，单击“资源提供程序”，然后单击“注册 Alerts.Management”。

## <a name="next-steps"></a>后续步骤

- [Azure Monitor 工作簿](../visualize/workbooks-overview.md)
- [Azure 资源运行状况](../../service-health/resource-health-overview.md)
- [Azure Monitor 警报](../alerts/alerts-overview.md)

