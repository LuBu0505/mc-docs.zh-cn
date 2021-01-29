---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: GROUP BY 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 GROUP BY 语法。
ms.openlocfilehash: 4fbe457a907b32c4cde5cd797125a43741bd6b86
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692180"
---
# <a name="group-by-clause"></a>GROUP BY 子句

基于一组指定的分组表达式对行进行分组，并基于一个或多个指定的聚合函数对行组计算聚合。 SQL Analytics 还支持高级聚合，以使用 ``GROUPING SETS``、``CUBE`` 和 ``ROLLUP`` 子句对同一输入记录集进行多个聚合。
将 ``FILTER`` 子句附加到聚合函数时，仅将匹配的行传递给该函数。

## <a name="syntax"></a>语法

```
GROUP BY group_expression [ , group_expression [ , ... ] ]
    [ { WITH ROLLUP | WITH CUBE | GROUPING SETS (grouping_set [ , ...]) } ]

GROUP BY GROUPING SETS (grouping_set [ , ...])
```

而聚合函数被定义为

```sql
aggregate_name ( [ DISTINCT ] expression [ , ... ] ) [ FILTER ( WHERE boolean_expression ) ]
```

## <a name="parameters"></a>参数

* **GROUPING SETS**

  针对分组集中指定的表达式的每个子集的行进行分组。 例如，``GROUP BY GROUPING SETS (warehouse, product)`` 在语义上等效于 ``GROUP BY warehouse`` 和 ``GROUP BY product`` 的结果的并集。 此子句是 ``UNION ALL`` 的简写形式，其中 ``UNION ALL`` 运算符的每个段执行 ``GROUPING SETS`` 子句中指定的列子集的聚合。

* **grouping_set**

  分组集由括号中的零个或多个逗号分隔的表达式指定。

  **语法：** ``( [ expression [ , ... ] ] )``

* **grouping_expression**

  将行分组在一起的条件。 基于分组表达式的结果值执行行的分组。 分组表达式可以是列别名、列位置或表达式。

* **ROLLUP**

  在一个语句中指定多个级别的聚合。 此子句用于基于多个分组集计算聚合。 ``ROLLUP`` 是 ``GROUPING SETS`` 的速记。 例如，``GROUP BY warehouse, product WITH ROLLUP`` 等效于 ``GROUP BY GROUPING SETS
  ((warehouse, product), (warehouse), ())``。
  ``ROLLUP`` 规范的 N 个元素将得到 N + 1 个 ``GROUPING SETS``。

* **CUBE**

  ``CUBE`` 子句用于根据 ``GROUP BY`` 子句中指定的分组列的组合执行聚合。 ``CUBE`` 是 ``GROUPING SETS`` 的速记。 例如，``GROUP BY warehouse, product WITH CUBE`` 等效于 ``GROUP BY GROUPING SETS
  ((warehouse, product), (warehouse), (product), ())``。
  ``CUBE`` 规范的 N 个元素将得到 2^N 个 ``GROUPING SETS``。

* aggregate_name

  聚合函数名称（MIN、MAX、COUNT、SUM、AVG 等）。

* **DISTINCT**

  在将输入行传递给聚合函数之前，将其中的重复项删除。

* **FILTER**

  筛选 ``WHERE`` 子句中 ``boolean_expression`` 计算结果为 true 的输入行，并将其传递给聚合函数；将放弃其他行。

## <a name="examples"></a>示例

```sql
CREATE TABLE dealer (id INT, city STRING, car_model STRING, quantity INT);
INSERT INTO dealer VALUES
    (100, 'Fremont', 'Honda Civic', 10),
    (100, 'Fremont', 'Honda Accord', 15),
    (100, 'Fremont', 'Honda CRV', 7),
    (200, 'Dublin', 'Honda Civic', 20),
    (200, 'Dublin', 'Honda Accord', 10),
    (200, 'Dublin', 'Honda CRV', 3),
    (300, 'San Jose', 'Honda Civic', 5),
    (300, 'San Jose', 'Honda Accord', 8);

-- Sum of quantity per dealership. Group by `id`.
SELECT id, sum(quantity) FROM dealer GROUP BY id ORDER BY id;
+---+-------------+
| id|sum(quantity)|
+---+-------------+
|100|           32|
|200|           33|
|300|           13|
+---+-------------+

-- Use column position in GROUP by clause.
SELECT id, sum(quantity) FROM dealer GROUP BY 1 ORDER BY 1;
+---+-------------+
| id|sum(quantity)|
+---+-------------+
|100|           32|
|200|           33|
|300|           13|
+---+-------------+

-- Multiple aggregations.
-- 1. Sum of quantity per dealership.
-- 2. Max quantity per dealership.
SELECT id, sum(quantity) AS sum, max(quantity) AS max FROM dealer GROUP BY id ORDER BY id;
+---+---+---+
| id|sum|max|
+---+---+---+
|100| 32| 15|
|200| 33| 20|
|300| 13|  8|
+---+---+---+

-- Count the number of distinct dealer cities per car_model.
SELECT car_model, count(DISTINCT city) AS count FROM dealer GROUP BY car_model;
+------------+-----+
|   car_model|count|
+------------+-----+
| Honda Civic|    3|
|   Honda CRV|    2|
|Honda Accord|    3|
+------------+-----+

-- Sum of only 'Honda Civic' and 'Honda CRV' quantities per dealership.
SELECT id, sum(quantity) FILTER (
            WHERE car_model IN ('Honda Civic', 'Honda CRV')
        ) AS `sum(quantity)` FROM dealer
    GROUP BY id ORDER BY id;
+---+-------------+
| id|sum(quantity)|
+---+-------------+
|100|           17|
|200|           23|
|300|            5|
+---+-------------+

-- Aggregations using multiple sets of grouping columns in a single statement.
-- Following performs aggregations based on four sets of grouping columns.
-- 1. city, car_model
-- 2. city
-- 3. car_model
-- 4. Empty grouping set. Returns quantities for all city and car models.
SELECT city, car_model, sum(quantity) AS sum FROM dealer
    GROUP BY GROUPING SETS ((city, car_model), (city), (car_model), ())
    ORDER BY city;
+---------+------------+---+
|     city|   car_model|sum|
+---------+------------+---+
|     null|        null| 78|
|     null| HondaAccord| 33|
|     null|    HondaCRV| 10|
|     null|  HondaCivic| 35|
|   Dublin|        null| 33|
|   Dublin| HondaAccord| 10|
|   Dublin|    HondaCRV|  3|
|   Dublin|  HondaCivic| 20|
|  Fremont|        null| 32|
|  Fremont| HondaAccord| 15|
|  Fremont|    HondaCRV|  7|
|  Fremont|  HondaCivic| 10|
| San Jose|        null| 13|
| San Jose| HondaAccord|  8|
| San Jose|  HondaCivic|  5|
+---------+------------+---+

-- Alternate syntax for `GROUPING SETS` in which both `GROUP BY` and `GROUPING SETS`
-- specifications are present.
SELECT city, car_model, sum(quantity) AS sum FROM dealer
    GROUP BY city, car_model GROUPING SETS ((city, car_model), (city), (car_model), ())
    ORDER BY city, car_model;
+---------+------------+---+
|     city|   car_model|sum|
+---------+------------+---+
|     null|        null| 78|
|     null| HondaAccord| 33|
|     null|    HondaCRV| 10|
|     null|  HondaCivic| 35|
|   Dublin|        null| 33|
|   Dublin| HondaAccord| 10|
|   Dublin|    HondaCRV|  3|
|   Dublin|  HondaCivic| 20|
|  Fremont|        null| 32|
|  Fremont| HondaAccord| 15|
|  Fremont|    HondaCRV|  7|
|  Fremont|  HondaCivic| 10|
| San Jose|        null| 13|
| San Jose| HondaAccord|  8|
| San Jose|  HondaCivic|  5|
+---------+------------+---+

-- Group by processing with `ROLLUP` clause.
-- Equivalent GROUP BY GROUPING SETS ((city, car_model), (city), ())
SELECT city, car_model, sum(quantity) AS sum FROM dealer
    GROUP BY city, car_model WITH ROLLUP
    ORDER BY city, car_model;
+---------+------------+---+
|     city|   car_model|sum|
+---------+------------+---+
|     null|        null| 78|
|   Dublin|        null| 33|
|   Dublin| HondaAccord| 10|
|   Dublin|    HondaCRV|  3|
|   Dublin|  HondaCivic| 20|
|  Fremont|        null| 32|
|  Fremont| HondaAccord| 15|
|  Fremont|    HondaCRV|  7|
|  Fremont|  HondaCivic| 10|
| San Jose|        null| 13|
| San Jose| HondaAccord|  8|
| San Jose|  HondaCivic|  5|
+---------+------------+---+

-- Group by processing with `CUBE` clause.
-- Equivalent GROUP BY GROUPING SETS ((city, car_model), (city), (car_model), ())
SELECT city, car_model, sum(quantity) AS sum FROM dealer
    GROUP BY city, car_model WITH CUBE
    ORDER BY city, car_model;
+---------+------------+---+
|     city|   car_model|sum|
+---------+------------+---+
|     null|        null| 78|
|     null| HondaAccord| 33|
|     null|    HondaCRV| 10|
|     null|  HondaCivic| 35|
|   Dublin|        null| 33|
|   Dublin| HondaAccord| 10|
|   Dublin|    HondaCRV|  3|
|   Dublin|  HondaCivic| 20|
|  Fremont|        null| 32|
|  Fremont| HondaAccord| 15|
|  Fremont|    HondaCRV|  7|
|  Fremont|  HondaCivic| 10|
| San Jose|        null| 13|
| San Jose| HondaAccord|  8|
| San Jose|  HondaCivic|  5|
+---------+------------+---+

--Prepare data for ignore nulls example
CREATE TABLE person (id INT, name STRING, age INT);
INSERT INTO person VALUES
    (100, 'Mary', NULL),
    (200, 'John', 30),
    (300, 'Mike', 80),
    (400, 'Dan', 50);

--Select the first row in cloumn age
SELECT FIRST(age) FROM person;
+--------------------+
| first(age, false)  |
+--------------------+
| NULL               |
+--------------------+

--Get the first row in cloumn `age` ignore nulls,last row in column `id` and sum of cloumn `id`.
SELECT FIRST(age IGNORE NULLS), LAST(id), SUM(id) FROM person;
+-------------------+------------------+----------+
| first(age, true)  | last(id, false)  | sum(id)  |
+-------------------+------------------+----------+
| 30                | 400              | 1000     |
+-------------------+------------------+----------+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)