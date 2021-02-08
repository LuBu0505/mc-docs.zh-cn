---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: EXPLAIN - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 EXPLAIN 语法。
ms.openlocfilehash: 7bfda1ba495b2f918ae270fc769ff7ab61e3a5ac
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060997"
---
# <a name="explain"></a>EXPLAIN

为输入语句提供逻辑或物理计划。
默认情况下，此子句仅提供有关物理计划的信息。

## <a name="syntax"></a>语法

```
EXPLAIN [ EXTENDED | CODEGEN | COST | FORMATTED ] statement
```

## <a name="parameters"></a>参数

* **EXTENDED**

  生成解析逻辑计划、分析逻辑计划、优化逻辑计划和物理计划。
  解析逻辑计划是从查询中提取的未解析的计划。
  分析型逻辑计划转换可将 unresolvedAttribute 和 unresolvedRelation 转换为完全类型化的对象。 优化型逻辑计划通过一组优化规则进行转换，从而生成物理计划。

* **CODEGEN**

  为语句（如果有）和物理计划生成代码。

* **COST**

  如果计划节点统计信息可用，则将生成逻辑计划和统计信息。

* **FORMATTED**

  生成两个部分：物理计划概述和节点详细信息。

* **语句**

  要说明的 SQL 语句。

## <a name="examples"></a>示例

```sql
-- Default Output
EXPLAIN select k, sum(v) from values (1, 2), (1, 3) t(k, v) group by k;
```

```
+----------------------------------------------------+
|                                                plan|
+----------------------------------------------------+
| == Physical Plan ==
 *(2) HashAggregate(keys=[k#33], functions=[sum(cast(v#34 as bigint))])
 +- Exchange hashpartitioning(k#33, 200), true, [id=#59]
    +- *(1) HashAggregate(keys=[k#33], functions=[partial_sum(cast(v#34 as bigint))])
       +- *(1) LocalTableScan [k#33, v#34]
|
+----------------------------------------------------

-- Using Extended
```

```sql
EXPLAIN EXTENDED select k, sum(v) from values (1, 2), (1, 3) t(k, v) group by k;
```

```
+----------------------------------------------------+
|                                                plan|
+----------------------------------------------------+
| == Parsed Logical Plan ==
 'Aggregate ['k], ['k, unresolvedalias('sum('v), None)]
 +- 'SubqueryAlias `t`
    +- 'UnresolvedInlineTable [k, v], [List(1, 2), List(1, 3)]

 == Analyzed Logical Plan ==
 k: int, sum(v): bigint
 Aggregate [k#47], [k#47, sum(cast(v#48 as bigint)) AS sum(v)#50L]
 +- SubqueryAlias `t`
    +- LocalRelation [k#47, v#48]

 == Optimized Logical Plan ==
 Aggregate [k#47], [k#47, sum(cast(v#48 as bigint)) AS sum(v)#50L]
 +- LocalRelation [k#47, v#48]

 == Physical Plan ==
 *(2) HashAggregate(keys=[k#47], functions=[sum(cast(v#48 as bigint))], output=[k#47, sum(v)#50L])
+- Exchange hashpartitioning(k#47, 200), true, [id=#79]
   +- *(1) HashAggregate(keys=[k#47], functions=[partial_sum(cast(v#48 as bigint))], output=[k#47, sum#52L])
    +- *(1) LocalTableScan [k#47, v#48]
|
+----------------------------------------------------+

-- Using Formatted
```

```sql
EXPLAIN FORMATTED select k, sum(v) from values (1, 2), (1, 3) t(k, v) group by k;
```

```
+----------------------------------------------------+
|                                                plan|
+----------------------------------------------------+
| == Physical Plan ==
 * HashAggregate (4)
 +- Exchange (3)
    +- * HashAggregate (2)
       +- * LocalTableScan (1)

 (1) LocalTableScan [codegen id : 1]
 Output: [k#19, v#20]

 (2) HashAggregate [codegen id : 1]
 Input: [k#19, v#20]

 (3) Exchange
 Input: [k#19, sum#24L]

 (4) HashAggregate [codegen id : 2]
 Input: [k#19, sum#24L]
|
+----------------------------------------------------+
```