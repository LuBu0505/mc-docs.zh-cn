---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 表值函数 (TVF) - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的表值函数。
ms.openlocfilehash: f8a77b723e3eef523757fc7ca0fb86b78876f3ab
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692299"
---
# <a name="table-valued-function-tvf"></a>表值函数 (TVF)

返回关系或行集的函数。 有两种类型的 TVF：

* 已在 ``FROM`` 子句中指定，例如 ``range``。
* 已在 ``SELECT`` 和 ``LATERAL VIEW`` 子句中指定，例如 ``explode``。

## <a name="syntax"></a>语法

```sql
function_name ( expression [ , ... ] ) [ table_alias ]
```

## <a name="parameters"></a>参数

* **expression**

  生成值的一个或多个值、运算符和 SQL 函数的组合。

* table_alias

  具有可选列名列表的临时名称。

  **语法：** ``[ AS ] table_name [ ( column_name [ , ... ] ) ]``

## <a name="supported-table-valued-functions"></a>支持的表值函数

### <a name="tvfs-that-can-be-specified-in-from-clauses"></a>可在 ``FROM`` 子句中指定的 TVF

| 功能                                | 参数类型      | 说明                                                                                                                                                                      |
|-----------------------------------------|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| range (end)                             | Long                  | 创建具有名为 id 的单个 LongType 列的表，其中包含从 0 到结束（不包含）范围内的行，步进值为 1。                                                  |
| range (start, end)                      | Long, Long            | 创建具有名为 id 的单个 LongType 列的表，其中包含从开始到结束（不包含）范围内的行，步进值为 1。                                              |
| range (start, end, step)                | Long, Long, Long      | 创建具有名为 id 的单个 LongType 列的表，其中包含从开始到结束（不包含）范围内的行，带步进值。                                                |
| range (start, end, step, numPartitions) | Long, Long, Long, Int | 创建具有名为 id 的单个 LongType 列的表，其中包含从开始到结束（不包含）范围内的行，带步进值并指定了分区编号 numPartitions。 |

### <a name="tvfs-that-can-be-specified-in-select-and-lateral-view-clauses"></a>可在 ``SELECT`` 和 ``LATERAL VIEW`` 子句中指定的 TVF

| 功能                                | 参数类型 | 说明                                                                                                                                                                                                                                                                                   |
|-----------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| explode (expr)                          | 数组/映射        | 将数组 expr 的元素分为多个行，或将映射 expr 的元素分为多个行和列。 除非另有说明，否则对数组或键的元素使用默认列名称 col，并对映射元素使用值。                                     |
| explode_outer (expr)                   | 数组/映射        | 将数组 expr 的元素分为多个行，或将映射 expr 的元素分为多个行和列。 除非另有说明，否则对数组或键的元素使用默认列名称 col，并对映射元素使用值。                                     |
| inline (expr)                           | Expression       | 将结构数组分解为一个表。 默认情况下使用列名称 col1、col2 等，除非另有说明。                                                                                                                                                                          |
| inline_outer (expr)                    | Expression       | 将结构数组分解为一个表。 默认情况下使用列名称 col1、col2 等，除非另有说明。                                                                                                                                                                          |
| posexplode (expr)                       | 数组/映射        | 将数组 expr 的元素分为多个包含位置的行，或将映射 expr 的元素分为多个包含位置的行和列。 除非另有说明，否则使用列名称 pos 作为位置，将 col 用于数组或键的元素，并将值用于映射元素。 |
| posexplode_outer (expr)                 | 数组/映射        | 将数组 expr 的元素分为多个包含位置的行，或将映射 expr 的元素分为多个包含位置的行和列。 除非另有说明，否则使用列名称 pos 作为位置，将 col 用于数组或键的元素，并将值用于映射元素。 |
| stack (n, expr1, …, exprk)              | Seq[Expression]  | 将 expr1, …, exprk 拆分为 n 行。 默认情况下使用列名称 col0、col1 等，除非另有说明。                                                                                                                                                                              |
| json_tuple (jsonStr, p1, p2, …, pn)    | Seq[Expression]  | 像函数 get_json_object 一样返回一个元组，但采用多个名称。 所有输入参数和输出列类型均为字符串。                                                                                                                                                  |
| parse_url (url, partToExtract[, key] ) | Seq[Expression]  | 从 URL 中提取部件。                                                                                                                                                                                                                                                                   |

## <a name="examples"></a>示例

```sql
-- range call with end
SELECT * FROM range(6 + cos(3));
+---+
| id|
+---+
|  0|
|  1|
|  2|
|  3|
|  4|
+---+

-- range call with start and end
SELECT * FROM range(5, 10);
+---+
| id|
+---+
|  5|
|  6|
|  7|
|  8|
|  9|
+---+

-- range call with numPartitions
SELECT * FROM range(0, 10, 2, 200);
+---+
| id|
+---+
|  0|
|  2|
|  4|
|  6|
|  8|
+---+

-- range call with a table alias
SELECT * FROM range(5, 8) AS test;
+---+
| id|
+---+
|  5|
|  6|
|  7|
+---+

SELECT explode(array(10, 20));
+---+
|col|
+---+
| 10|
| 20|
+---+

SELECT inline(array(struct(1, 'a'), struct(2, 'b')));
+----+----+
|col1|col2|
+----+----+
|   1|   a|
|   2|   b|
+----+----+

SELECT posexplode(array(10,20));
+---+---+
|pos|col|
+---+---+
|  0| 10|
|  1| 20|
+---+---+

SELECT stack(2, 1, 2, 3);
+----+----+
|col0|col1|
+----+----+
|   1|   2|
|   3|null|
+----+----+

SELECT json_tuple('{"a":1, "b":2}', 'a', 'b');
+---+---+
| c0| c1|
+---+---+
|  1|  2|
+---+---+

SELECT parse_url('http://spark.apache.org/path?query=1', 'HOST');
+-----------------------------------------------------+
|parse_url(http://spark.apache.org/path?query=1, HOST)|
+-----------------------------------------------------+
|                                     spark.apache.org|
+-----------------------------------------------------+

-- Use explode in a LATERAL VIEW clause
CREATE TABLE test (c1 INT);
INSERT INTO test VALUES (1);
INSERT INTO test VALUES (2);
SELECT * FROM test LATERAL VIEW explode (ARRAY(3,4)) AS c2;
+--+--+
|c1|c2|
+--+--+
| 1| 3|
| 1| 4|
| 2| 3|
| 2| 4|
+--+--+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)