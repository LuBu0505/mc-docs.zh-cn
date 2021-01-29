---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 队列 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 上的队列。
ms.openlocfilehash: 25a9597b4810f01d62d2a9b9531c45290ecefda4
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692271"
---
# <a name="cohorts"></a>队列

队列分析会检查预定义组（称为队列）在一系列阶段中的进展结果。 队列图表的显著特征是比较两个不同时序中变量的变化。 例如，常见的队列定义是按注册时间段划分的用户和按天划分的使用模式。 其他示例包括：

* 每月硬盘驱动器故障统计信息（按月）
* 每周供应商交付业绩（按周）
* 每月班级平均成绩（按月）

尽管有多种方法可定义队列分析的各个阶段，但 SQL Analytics 支持按每日、每周或每月进行队列可视化。 此外，SQL Analytics 队列图表会将队列在给定时间段内的度量值与该组的初始总体大小进行比较。

## <a name="data-format"></a>数据格式

SQL Analytics 要求输入示例包含以下字段：

* **队列日期**：唯一标识队列的日期。 假设你要按注册日期来可视化显示每月的用户活动，则 2018 年 1 月注册的所有用户的队列日期为 2018 年 1 月 1 日。 2 月注册的所有用户的队列日期为 2018 年 2 月 1 日。
* **时间段**：从队列日期到本示例为止经过的时间段计数。 如果你要按注册月份对用户进行分组，则时间段将是自这些用户注册以来的月份计数。 在上例中，对 1 月注册的用户在 7 月的活动进行度量将得到时间段值 7，因为在 1 月与 7 月之间经过了 7 个时间段。
* **满足目标的计数**：此队列在给定时间段内的表现的实际度量值。 在上例中，如果 1 月注册的 30 位用户在 7 月均有活动，则满足目标的计数将为 30。
* **总队列大小**：SQL Analytics 将用于计算队列在给定时间段内目标满意度百分比的分母。 继续上面的示例，如果有 72 位用户在 1 月注册，则总队列大小为 72。 呈现可视化效果时，SQL Analytics 会将该值显示为 ``41.67%`` (``32 ÷ 72``)。

## <a name="cohort-date-notes"></a>队列日期注释

即使你按月或按周定义队列，SQL Analytics 也要求“队列日期”列中的值是完整的日期值。 如果按月分组，则应将 ``2018-01-18`` 缩短为 ``2018-01-01`` 或 1 月中的任何其他完整日期，而不是 ``2018-01``。

在呈现之前，队列可视化工具会将所有日期和时间值转换为 GMT。 为避免呈现出现问题，应按照当地 UTC 时差调整从数据库返回的日期时间。