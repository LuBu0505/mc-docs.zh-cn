---
title: 在 Azure Monitor 中监视使用情况和预估成本
description: Azure Monitor 中的使用情况和预估成本页的使用过程概述
author: Johnnytechn
services: azure-monitor
ms.topic: conceptual
origin.date: 10/28/2019
ms.date: 02/20/2021
ms.author: v-johya
ms.reviewer: Dale.Koetke
ms.subservice: ''
ms.openlocfilehash: f700e1f6aa9c66f82d2ba453070566ac72b048fb
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102205365"
---
# <a name="monitoring-usage-and-estimated-costs-in-azure-monitor"></a>在 Azure Monitor 中监视使用情况和预估成本

> [!NOTE]
> 本文介绍如何查看多个 Azure 监视功能的使用情况和估算成本。 Azure Monitor 特定组件的相关文章包括：
> - [使用 Azure Monitor 日志管理使用情况和成本](logs/manage-cost-storage.md)介绍了如何通过更改数据保留期来控制成本，以及如何分析数据使用情况并对其发出警报。
> - [管理 Application Insights 的使用情况和成本](app/pricing.md)介绍了如何在 Application Insights 中分析数据使用情况。

## <a name="azure-monitor-pricing-model"></a>Azure Monitor 定价模型

基本 Azure Monitor 计费模型是云友好的基于消耗量的定价模型（“即用即付”）。 只需为所用的部分付费。 我们已提供适用于[警报、指标和通知](https://www.azure.cn/pricing/details/monitor/)、[Log Analytics](https://www.azure.cn/pricing/details/monitor/) 及 [Application Insights](https://www.azure.cn/pricing/details/monitor/) 的定价详细信息。 

除了适用于日志数据的即用即付模型以外，Log Analytics 还提供容量预留，与即用即付价格相比，该模型最多可将成本节省 25%。 产能预留定价使你可以购买起价为 100 GB/天的保留。 超过预留级别的任何用量将按即用即付费率计费。 [详细了解](https://www.azure.cn/pricing/details/monitor/)容量预留定价。

某些客户有权访问[传统 Log Analytics 定价层](logs/manage-cost-storage.md#legacy-pricing-tiers)和[传统 Enterprise Application Insights 定价层](app/pricing.md#legacy-enterprise-per-node-pricing-tier)。 

## <a name="understanding-your-azure-monitor-costs"></a>了解 Azure Monitor 成本

<!-- removed by FB -->
### <a name="estimating-the-costs-to-manage-your-environment"></a>估算环境的管理成本

如果你尚未使用 Azure Monitor 日志，可以使用 [Azure Monitor 定价计算器](https://www.azure.cn/pricing/calculator/?service=monitor)来估算 Azure Monitor 的使用成本。 首先在搜索框中输入“Azure Monitor”，然后单击生成的 Azure Monitor 磁贴。 在页面中向下滚动到“Azure Monitor”，然后从“类型”下拉列表中选择一个选项。
<!-- correct of https://www.azure.cn/pricing/calculator/ -->

- 指标查询和警报  
- Log Analytics
- Application Insights

在上述每种情况下，定价计算器都可帮助你根据预期用量估算可能的成本。

例如，对于 Log Analytics，可以输入 VM 数目，以及要从每个 VM 收集的数据量 (GB)。 通常每月会从一个典型的 Azure VM 引入 1 GB 到 3 GB 数据。 如果已经评估了 Azure Monitor 日志，则可以使用自己环境中的数据统计信息。 请参阅下文来了解如何确定[受监视 VM 数](logs/manage-cost-storage.md#understanding-nodes-sending-data)和[工作区引入的数据量](logs/manage-cost-storage.md#understanding-ingested-data-volume)。

类似于 Application Insights，如果启用“基于应用程序活动估算数据量”功能，则可以提供有关应用程序的输入（如果收集客户端遥测数据，请提供每月请求数和页面视图数），然后，计算器会告知类似应用程序收集的中间值和第 90 百分位的数据量。 这些应用程序超过了 Application Insights 的配置范围（例如，有些应用程序采用默认采样，而有些应用程序根本不使用采样，等等），因此，你仍可以通过采样控制，将引入的数据量降到远远低于中位水平。 但这只是了解其他类似客户看到的情况的一个起点。 [详细了解](app/pricing.md#estimating-the-costs-to-manage-your-application)如何估算 Application Insights 的成本。

<!-- removed by FB -->

也可以在“Monitor”中心的“使用情况和估算成本”页中查看 Azure Monitor 的使用情况。  此页会显示[警报、指标和通知](https://www.azure.cn/pricing/details/monitor/)、[Azure Log Analytics](https://www.azure.cn/pricing/details/monitor/) 及 [Azure Application Insights](https://www.azure.cn/pricing/details/monitor/) 等核心监视功能的使用情况。 对于使用 2018 年 4 月之前提供的定价计划的客户，这还包括通过见解和分析套餐购买的 Log Analytics 使用情况功能。

在此页上，用户可以查看过去 31 天的资源使用情况（按订阅聚合）。 `Drill-ins` 显示 31 天的使用趋势。 需要聚合大量的数据才能进行此估算，因此请耐心等待页面加载。

以下示例显示正在监视使用情况，以及最终的预估成本：

![使用情况和预估成本门户屏幕截图](./media/usage-estimated-costs/001.png)

选择“每月使用情况”列中的链接会打开一个图表，其中显示过去 31 天的使用趋势： 

![“每个节点包含”条形图屏幕截图](./media/usage-estimated-costs/002.png)

<!-- removed by FB about Operations Management Suite subscription entitlements -->
