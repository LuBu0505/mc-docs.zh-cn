---
title: 在 Azure Monitor 中更新经典警报和监视
description: 关于停用经典监视服务和功能（以前在 Azure 门户中的“警报(经典)”下显示）的说明。
author: Johnnytechn
services: azure-monitor
ms.topic: conceptual
ms.date: 02/20/2021
ms.author: v-johya
ms.subservice: alerts
ms.openlocfilehash: d1491bdd0e42038e9718c312ce77f775383e9f5d
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197251"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Azure Monitor 中的统一警报和监视替换经典警报和监视

Azure Monitor 现已成为统一的全栈监视服务，支持对多个资源使用“一个指标”和“一个警报”；如需更多信息，请参阅[关于新 Azure Monitor 的博客文章](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/)。新构建的 Azure 监视和警报平台更快速、更智能并且可以扩展 - 与不断增长的云计算齐头并进，并与 Microsoft 智能云理念保持一致。

随着新的 Azure 监视和警报平台部署到位，Azure Monitor 中的经典警报已对公有云用户停用，但仍可有限制地用于那些尚不支持新警报的资源。

 ![Azure 门户中的经典警报](./media/monitoring-classic-retirement/monitor-alert-screen2.png) 

我们鼓励你开始在新平台中重新创建警报。

> [!IMPORTANT]
> 基于活动日志创建的经典警报规则不会被弃用或迁移。 可以从新的 Azure Monitor -“警报”按现样访问和使用基于活动日志创建的所有经典警报规则。 有关详细信息，请参阅[使用 Azure Monitor 创建、查看和管理活动日志警报](./alerts-activity-log.md)。 类似地，可以从新的“服务运行状况”部分按现样访问和使用基于服务运行状况的警报。 有关详细信息，请参阅[基于服务运行状况通知的警报](../../service-health/alerts-activity-log-service-notifications-portal.md)。

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Application Insights 中的统一指标和警报

<!--Not available in MC: ITSM, Voice Calls -->
Azure Monitor 的新指标平台现将支持来自 Application Insights 的监视。 此项移动意味着 Application Insights 将挂接到操作组，允许除上一封电子邮件和 webhook 调用之外的更多选项。 警报现在可以触发 Azure Functions、逻辑应用和短信。 凭借准实时的监视和警报，新平台使 Application Insights 用户能够利用支持在其他 Azure 资源中监视的相同技术和 Microsoft 产品的核心监视。

新的适用于 Application Insights 的统一监视和警报将包含：

- **Application Insights 平台指标** - 提供 Application Insights 产品中常用的预建指标。 有关详细信息，请参阅这篇有关如何使用[新 Azure Monitor 上的 Application Insights 平台指标](../app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics)的文章。
- **Application Insights 自定义指标** - 使你能够定义和发出自己的监视和警报指标。 有关详细信息，请参阅这篇有关如何使用[新 Azure Monitor 上 Application Insights 的自定义指标](../app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation)的文章。
- **Application Insights 故障异常（智能检测的一部分）** - 如果 Web 应用的失败 HTTP 请求速率或依赖项调用速率出现异常上升，Application Insights 会准实时地自动通知你。 有关详细信息，请参阅此文：[智能检测 - 故障异常](../app/proactive-failure-diagnostics.md)。

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>其他 Azure 资源的统一指标和警报

<!--Not available in MC: ITSM, Voice Calls -->
从 2018 年 3 月开始，已提供 Azure 资源的新一代警报和多维度监视。 现在，新指标平台和警报更快速，具有准实时的功能。 更重要的是，新指标平台警报能够提供更多粒度，因为新平台包括维度选项，使你能够切片和筛选到特定值组合、条件或操作。 与新 Azure Monitor 中的所有警报一样，新指标警报通过使用 ActionGroup 增加了可扩展性 - 使通知能够扩展到电子邮件或 Webhook 之外，涵盖短信、Azure 函数、自动化 Runbook 等。 有关详细信息，请参阅[使用 Azure Monitor 创建、查看和管理指标警报](./alerts-metric.md)。
Azure 资源的新指标按以下形式提供：

- **Azure Monitor 标准版平台指标** - 提供来自各种 Azure 服务和产品的常用预填充指标。 有关详细信息，请参阅这篇有关 [Azure Monitor 上支持的指标](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported)和 [Azure Monitor 上支持的指标警报](./alerts-metric-overview.md#supported-resource-types-for-metric-alerts)文章。

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>经典监视和警报平台的停用

如前所述，旧的经典监视和警报已对公有云用户停用（包括关闭相关的 API、Azure 门户界面以及其中的服务），但仍可有限制地用于那些尚不支持新警报的资源。 具体而言，将弃用以下功能：

- 当前可通过 Azure 门户的[警报(经典)部分](./alerts-classic.overview.md)使用 Azure 资源的旧（经典）指标和警报；可作为 [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) 资源访问
- 当前可通过 Azure 门户的[警报(经典)部分](./alerts-classic.overview.md)使用 Application Insights 的旧（经典）平台和自定义指标以及相关警报；可作为 [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) 资源访问
- 旧（经典）故障异常警报当前在 Azure 门户中作为 [Application Insights 内的智能检测](../app/proactive-diagnostics.md)提供；其中配置的警报显示在 Azure 门户的[警报(经典)部分](./alerts-classic.overview.md)

这意味着：

- 经典监视和警报服务将停用，并且不再可用于创建新的警报规则。
- 继续存在于“警报(经典)”中的任何警报规则都会继续执行并触发通知。
- 经典监视和警报中可迁移的警报规则将由 Microsoft 在几周内分阶段自动移到新的 Azure 监视平台中的等效项。 该过程无缝进行且没有任何停机时间，并且客户在监视覆盖范围内将没有任何损失。
- 迁移到新的警报平台的警报规则将提供与之前相同的监视覆盖范围，但将触发具有新的有效负载的通知。 与经典警报规则相关联的任何电子邮件地址、Webhook 终结点或逻辑应用链接在迁移时都将被转移，但行为可能不正确，因为警报有效负载在新平台中将有所不同。
- 一些[因无法自动迁移而需要用户手动操作的经典警报规则](../alerts/alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts)将继续运行。

> [!IMPORTANT]
> Azure Monitor 已经分阶段推出了[自愿性迁移工具](../alerts/alerts-using-migration-tool.md)，可以很快将其经典警报规则迁移到新平台。 请针对仍然存在且可迁移的所有经典警报规则强制运行该工具。 在迁移经典警报规则后，客户将需要确保对使用经典警报规则有效负载的自动化进行修改以处理来自 [Application Insights 中的统一指标和警报](#unified-metrics-and-alerts-in-application-insights)或[其他 Azure 资源的统一指标和警报](#unified-metrics-and-alerts-for-other-azure-resources)的新有效负载。 有关详细信息，请参阅[准备经典警报规则迁移](../alerts/alerts-prepare-migration.md)

本文将不断更新有关新 Azure 监视和警报功能的链接和详细信息，以及工具的可用性，以帮助用户采用新的 Azure Monitor 平台。

## <a name="pricing-for-migrated-alert-rules"></a>已迁移的警报规则的定价

我们正在推出一个迁移工具，它可以帮助你将 Azure Monitor [经典警报](./alerts-classic.overview.md)迁移到新的警报体验。 已迁移的警报规则和相应的已迁移操作组（电子邮件、Webhook 或 LogicApp）仍将免费。 迁移警报规则以后，你可以继续免费使用以前的经典警报功能，包括编辑阈值、聚合类型和聚合粒度的功能。 但是，如果你编辑已迁移的警报规则，以便使用任何新的警报平台功能、通知或操作类型，则需支付相应的费用。 若要详细了解警报规则和通知的定价，请参阅 [Azure Monitor 定价](https://www.azure.cn/pricing/details/monitor/)。

下面是警报规则收费案例的示例：

- 新 Azure Monitor 平台上除免费单位数之外创建的任何新警报（非迁移）规则
- Azure Monitor 内除免费单位数之外引入和保留的任何数据
- Application Insights 执行的任何多测试 Web 测试
- Azure Monitor 内除免费单位数之外存储的任何自定义指标
- 经过编辑后可以使用更新的指标警报功能（例如频率、多个资源/维度、[动态阈值](../alerts/alerts-dynamic-thresholds.md)、变化的资源/信号等）的任何已迁移警报规则。
- 任何经过编辑后可以使用更新的通知或操作类型（例如短信）的已迁移操作组。

## <a name="next-steps"></a>后续步骤

* 了解[新的统一 Azure Monitor](../overview.md)。
* 了解新的 [Azure 警报](../platform/alerts-overview.md)。


