---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SELECT - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SELECT 语法。
ms.openlocfilehash: 7adaf23a36b6792ccf385061a30b347f14fb8cf5
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060889"
---
# <a name="select"></a>SELECT

从一个或多个表中检索结果集。

## <a name="syntax"></a>语法

```
[ WITH with_query [ , ... ] ]
select_statement [ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select_statement, ... ]
    [ ORDER BY { expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [ , ... ] } ]
    [ SORT BY { expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [ , ... ] } ]
    [ CLUSTER BY { expression [ , ... ] } ]
    [ DISTRIBUTE BY { expression [, ... ] } ]
    [ WINDOW { named_window [ , WINDOW named_window, ... ] } ]
    [ LIMIT { ALL | expression } ]
```

而 ``select_statement`` 被定义为

```
SELECT [ hints , ... ] [ ALL | DISTINCT ] { named_expression [ , ... ] }
    FROM { from_item [ , ... ] }
    [ PIVOT clause ]
    [ LATERAL VIEW clause ] [ ... ]
    [ WHERE boolean_expression ]
    [ GROUP BY expression [ , ... ] ]
    [ HAVING boolean_expression ]
```

## <a name="parameters"></a>参数

* **with_query**

  主查询块前面的一个或多个[通用表表达式](sql-ref-syntax-qry-select-cte.md)。稍后可在 ``FROM`` 子句中引用这些表表达式。 若要在 ``FROM`` 子句中找出重复的子查询块，这很有用，它还能提高查询的可读性。

* **[提示](sql-ref-syntax-qry-select-hints.md)**

  提示可帮助 Spark 优化器作出更好的计划决策。 Spark 支持使用会影响联接策略的选择和数据的重新分区的提示。

* **ALL**

  选择关系中的所有匹配行。 默认情况下启用。

* **DISTINCT**

  删除结果中的重复项后，从关系中选择所有匹配的行。

* **named_expression**

  具有指定名称的表达式。 表示列表达式。

  **语法：** ``expression [AS] [alias]``

* **from_item**

  查询的输入源。 下列类型作之一：

  * 表标识符
    * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
      * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。 可在表关系后使用 ``TIMESTAMP AS OF``、``VERSION AS OF`` 或 ``@`` 语法指定“按时间顺序查看”版本。 有关详细信息，请参阅[查询表的旧快照（按时间顺序查看）](../../../../delta/delta-batch.md#deltatimetravel)。
    * [JOIN](sql-ref-syntax-qry-select-join.md)
    * [表值函数 (TVF)](sql-ref-syntax-qry-select-tvf.md)
    * [内联表](sql-ref-syntax-qry-select-inline-table.md)
    * 子查询
* **[PIVOT](sql-ref-syntax-qry-select-pivot.md)**

  用于数据透视；可根据特定的列值获取聚合值。

* **[LATERAL VIEW](sql-ref-syntax-qry-select-lateral-view.md)**

  与生成器函数（例如 ``EXPLODE``）结合使用，将生成包含一个或多个行的虚拟表。 ``LATERAL VIEW`` 将行应用于每个原始输出行。

* **WHERE**

  根据提供的谓词筛选 ``FROM`` 子句的结果。

* **[GROUP BY](sql-ref-syntax-qry-select-groupby.md)**

  用于对行进行分组的表达式。 它与聚合函数（``MIN``、``MAX``、``COUNT``、``SUM``、``AVG``）结合使用，基于分组表达式和每个组中的聚合值对行进行分组。 将 ``FILTER`` 子句附加到聚合函数时，只会将匹配的行传递给该函数。

* **[HAVING](sql-ref-syntax-qry-select-having.md)**

  用于筛选由 ``GROUP BY`` 生成的行的谓词。 ``HAVING`` 子句用于在执行分组后对行进行筛选。 如果指定的 ``HAVING`` 不带 ``GROUP BY``，则表示不带分组表达式的 ``GROUP BY``（全局聚合）。

* **[ORDER BY](sql-ref-syntax-qry-select-orderby.md)**

  查询的完整结果集的行顺序。 跨分区对输出行进行排序。 此参数与 ``SORT BY``、``CLUSTER BY`` 和 ``DISTRIBUTE BY`` 互斥，不能一起指定。

* **[SORT BY](sql-ref-syntax-qry-select-sortby.md)**

  用于对每个分区中的行进行排序的排序依据。 此参数与 ``ORDER BY`` 和 ``CLUSTER BY`` 互斥，不能一起指定。

* **[CLUSTER BY](sql-ref-syntax-qry-select-clusterby.md)**

  一组表达式，用于对行进行重新分区和排序。 使用此子句与同时使用 ``DISTRIBUTE BY`` 和 ``SORT BY`` 的效果相同。

* **[DISTRIBUTE BY](sql-ref-syntax-qry-select-distributeby.md)**

  一组表达式，作为对结果行进行重新分区的依据。 此参数与 ``ORDER BY`` 和 ``CLUSTER BY`` 互斥，不能一起指定。

* **[LIMIT](sql-ref-syntax-qry-select-limit.md)**

  语句或子查询可返回的最大行数。 此子句通常与 ``ORDER BY`` 结合使用来生成确定的结果。

* **boolean_expression**

  计算得出 ``Boolean`` 结果类型的任何表达式。 可使用逻辑运算符（``AND``、``OR``）将两个或多个表达式组合在一起。

* **expression**

  计算得出值的一个或多个值、运算符和 SQL 函数的组合。

* **named_window**

  一个或多个源窗口规范的别名。 可在查询的窗口定义中引用源窗口规范。

## <a name="related-articles"></a>相关文章

* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [公用表表达式 (CTE)](sql-ref-syntax-qry-select-cte.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [提示](sql-ref-syntax-qry-select-hints.md)
* [内联表](sql-ref-syntax-qry-select-inline-table.md)
* [JOIN](sql-ref-syntax-qry-select-join.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)
* [LIKE 谓词](sql-ref-syntax-qry-select-like.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [采样查询](sql-ref-syntax-qry-select-sampling.md)
* [set 运算符](sql-ref-syntax-qry-select-setops.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [表值函数 (TVF)](sql-ref-syntax-qry-select-tvf.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [开窗函数](sql-ref-syntax-qry-select-window.md)