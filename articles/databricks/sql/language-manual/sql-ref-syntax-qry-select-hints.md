---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 提示 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的提示语法。
ms.openlocfilehash: 4918b897d12c8ea8fd0f5335d3fa1da4f5dc7c29
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692181"
---
# <a name="hints"></a>提示

建议生成执行计划的特定方法。

## <a name="syntax"></a>语法

```sql
/*+ hint [ , ... ] */
```

## <a name="partitioning-hints"></a>分区提示

 通过分区提示，可建议 SQL Analytics 应遵循的分区策略。
支持 ``COALESCE``、``REPARTITION`` 和 ``REPARTITION_BY_RANGE`` 提示，它们分别等效于数据集 API ``coalesce``、``repartition`` 和 ``repartitionByRange``。 通过这些提示，你可优化性能并控制输出文件的数量。 如果指定了多个分区提示，则会在逻辑计划中插入多个节点，但优化器会选取最左侧的提示。

### <a name="partitioning-hint-types"></a>分区提示类型

* **COALESCE**

  将分区数减少到指定的分区数。 它采用分区数作为参数。

* **REPARTITION**

  使用指定的分区表达式将分区数调整为指定数量。 它采用分区数和/或列名称作为参数。

* **REPARTITION_BY_RANGE**

  使用指定的分区表达式将分区数调整为指定数量。 它采用列名称和分区数作为参数，其中分区数是可选项。

### <a name="examples"></a>示例

```sql
SELECT /*+ COALESCE(3) */ * FROM t;

SELECT /*+ REPARTITION(3) */ * FROM t;

SELECT /*+ REPARTITION(c) */ * FROM t;

SELECT /*+ REPARTITION(3, c) */ * FROM t;

SELECT /*+ REPARTITION_BY_RANGE(c) */ * FROM t;

SELECT /*+ REPARTITION_BY_RANGE(3, c) */ * FROM t;

-- multiple partitioning hints
EXPLAIN EXTENDED SELECT /*+ REPARTITION(100), COALESCE(500), REPARTITION_BY_RANGE(3, c) */ * FROM t;
== Parsed Logical Plan ==
'UnresolvedHint REPARTITION, [100]
+- 'UnresolvedHint COALESCE, [500]
   +- 'UnresolvedHint REPARTITION_BY_RANGE, [3, 'c]
      +- 'Project [*]
         +- 'UnresolvedRelation [t]

== Analyzed Logical Plan ==
name: string, c: int
Repartition 100, true
+- Repartition 500, false
   +- RepartitionByExpression [c#30 ASC NULLS FIRST], 3
      +- Project [name#29, c#30]
         +- SubqueryAlias spark_catalog.default.t
            +- Relation[name#29,c#30] parquet

== Optimized Logical Plan ==
Repartition 100, true
+- Relation[name#29,c#30] parquet

== Physical Plan ==
Exchange RoundRobinPartitioning(100), false, [id=#121]
+- *(1) ColumnarToRow
   +- FileScan parquet default.t[name#29,c#30] Batched: true, DataFilters: [], Format: Parquet,
      Location: CatalogFileIndex[file:/spark/spark-warehouse/t], PartitionFilters: [],
      PushedFilters: [], ReadSchema: struct<name:string>
```

## <a name="join-hints"></a>联接提示

通过联接提示，可建议 SQL Analytics 应使用的联接策略。 如果在联接的两端指定了不同的联接策略提示，SQL Analytics 将按以下顺序设置提示优先级：``BROADCAST`` 高于 ``MERGE`` 高于 ``SHUFFLE_HASH`` 高于 ``SHUFFLE_REPLICATE_NL``。 如果两端都指定有 ``BROADCAST`` 提示或 ``SHUFFLE_HASH`` 提示，SQL Analytics 会根据联接类型和关系大小选取生成端。 由于给定的策略可能不支持部分联接类型，因此不保证 SQL Analytics 使用提示建议的联接策略。

### <a name="join-hint-types"></a>联接提示类型

* **BROADCAST**

  使用广播联接。 无论 ``autoBroadcastJoinThreshold`` 如何，都将广播带有提示的联接端。 如果联接的两端都具有广播提示，则广播较小的一端（根据统计信息确定）。 ``BROADCAST`` 的别名为 ``BROADCASTJOIN`` 和 ``MAPJOIN``。

* **MERGE**

  使用随机排序合并联接。 ``MERGE`` 的别名为 ``SHUFFLE_MERGE`` 和 ``MERGEJOIN``。

* **SHUFFLE_HASH**

  使用随机哈希联接。 如果两端都有随机哈希提示，SQL Analytics 会选择较小的一端作为生成端（根据统计信息确定）。

* **SHUFFLE_REPLICATE_NL**

  使用随机复制嵌套循环联接。

### <a name="examples"></a>示例

```sql
-- Join Hints for broadcast join
SELECT /*+ BROADCAST(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
SELECT /*+ BROADCASTJOIN (t1) */ * FROM t1 left JOIN t2 ON t1.key = t2.key;
SELECT /*+ MAPJOIN(t2) */ * FROM t1 right JOIN t2 ON t1.key = t2.key;

-- Join Hints for shuffle sort merge join
SELECT /*+ SHUFFLE_MERGE(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
SELECT /*+ MERGEJOIN(t2) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
SELECT /*+ MERGE(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;

-- Join Hints for shuffle hash join
SELECT /*+ SHUFFLE_HASH(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;

-- Join Hints for shuffle-and-replicate nested loop join
SELECT /*+ SHUFFLE_REPLICATE_NL(t1) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;

-- When different join strategy hints are specified on both sides of a join, SQL Analytics
-- prioritizes the BROADCAST hint over the MERGE hint over the SHUFFLE_HASH hint
-- over the SHUFFLE_REPLICATE_NL hint.
-- SQL Analytics will issue Warning in the following example
-- org.apache.spark.sql.catalyst.analysis.HintErrorLogger: Hint (strategy=merge)
-- is overridden by another hint and will not take effect.
SELECT /*+ BROADCAST(t1), MERGE(t1, t2) */ * FROM t1 INNER JOIN t2 ON t1.key = t2.key;
```

## <a name="skew-hints"></a>倾斜提示

（Azure Databricks 上的 Delta Lake）有关 ``SKEW`` 提示的详细信息，请参阅[倾斜联接优化](../../delta/join-performance/skew-join.md#skew-join)。

## <a name="related-statements"></a>相关语句

* [JOIN](sql-ref-syntax-qry-select-join.md)
* [SELECT](sql-ref-syntax-qry-select.md)