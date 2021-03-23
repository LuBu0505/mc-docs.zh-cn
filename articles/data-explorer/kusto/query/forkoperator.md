---
title: fork 运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 fork 运算符。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
ms.date: 03/18/2021
zone_pivot_group_filename: data-explorer/zone-pivot-groups.json
zone_pivot_groups: kql-flavors
ms.openlocfilehash: f4647c33fc9832e5f07cda6d267b9180c3254afa
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104765909"
---
# <a name="fork-operator"></a>fork 运算符
::: zone pivot="azuredataexplorer"

并行运行多个 consumer 运算符。
 
## <a name="syntax"></a>语法

*T* `|` `fork` [*name*`=`]`(`*subquery*`)` [*name*`=`]`(`*subquery*`)` ...

## <a name="arguments"></a>参数

* subquery 是查询运算符的下游管道
* name 是子查询结果表的临时名称

## <a name="returns"></a>返回

多个结果表，每个子查询一个。

**支持的运算符**

[`as`](asoperator.md), [`count`](countoperator.md), [`extend`](extendoperator.md), [`parse`](parseoperator.md), [`where`](whereoperator.md), [`take`](takeoperator.md), [`project`](projectoperator.md), [`project-away`](projectawayoperator.md), [`project-keep`](project-keep-operator.md), [`project-rename`](projectrenameoperator.md), [`project-reorder`](projectreorderoperator.md), [`summarize`](summarizeoperator.md), [`top`](topoperator.md), [`top-nested`](topnestedoperator.md), [`sort`](sortoperator.md), [`mv-expand`](mvexpandoperator.md), [`reduce`](reduceoperator.md)

**备注**

* [`materialize`](materializefunction.md) 函数可用作在分支上使用 [`join`](joinoperator.md) 或 [`union`](unionoperator.md) 的替代方法。
输入流将通过 materialize 进行缓存，然后缓存的表达式可以在 join/union 分支中使用。

* 通过 `name` 参数或 [`as`](asoperator.md) 运算符给定的名称将用作 [`Kusto.Explorer`](../tools/kusto-explorer.md) 工具中结果选项卡的名称。

* 请避免将 `fork` 与单个子查询一起使用。

* 优先使用[批处理](batches.md)与表格表达式语句的 [`materialize`](materializefunction.md)，而不是 `fork` 运算符。

## <a name="examples"></a>示例

```kusto
KustoLogs
| where Timestamp > ago(1h)
| fork
    ( where Level == "Error" | project EventText | limit 100 )
    ( project Timestamp, EventText | top 1000 by Timestamp desc)
    ( summarize min(Timestamp), max(Timestamp) by ActivityID )
 
// In the following examples the result tables will be named: Errors, EventsTexts and TimeRangePerActivityID
KustoLogs
| where Timestamp > ago(1h)
| fork
    ( where Level == "Error" | project EventText | limit 100 | as Errors )
    ( project Timestamp, EventText | top 1000 by Timestamp desc | as EventsTexts )
    ( summarize min(Timestamp), max(Timestamp) by ActivityID | as TimeRangePerActivityID )
    
 KustoLogs
| where Timestamp > ago(1h)
| fork
    Errors = ( where Level == "Error" | project EventText | limit 100 )
    EventsTexts = ( project Timestamp, EventText | top 1000 by Timestamp desc )
    TimeRangePerActivityID = ( summarize min(Timestamp), max(Timestamp) by ActivityID )
```

::: zone-end

::: zone pivot="azuremonitor"

Azure Monitor 不支持此功能

::: zone-end
