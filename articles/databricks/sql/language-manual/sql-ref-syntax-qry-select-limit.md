---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: LIMIT 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 LIMIT 语法。
ms.openlocfilehash: 3d602768c7bffd57b170b186889ed29ab8674f81
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692255"
---
# <a name="limit-clause"></a>LIMIT 子句

限制 [SELECT](sql-ref-syntax-qry-select.md) 语句返回的行数。 通常，此子句会与 [ORDER BY](sql-ref-syntax-qry-select-orderby.md) 结合使用，以确保结果是确定的。
### <a name="syntax"></a>语法

```
LIMIT { ALL | integer_expression }
```

## <a name="parameters"></a>参数

* **ALL**

  如果指定了它，查询将返回所有行。 换句话说，如果指定了此选项，则没有限制。

* **integer_expression**

  一个返回整数的可折叠表达式。

## <a name="examples"></a>示例

```sql
CREATE TABLE person (name STRING, age INT);
INSERT INTO person VALUES
    ('Zen Hui', 25),
    ('Anil B', 18),
    ('Shone S', 16),
    ('Mike A', 25),
    ('John A', 18),
    ('Jack N', 16);

-- Select the first two rows.
SELECT name, age FROM person ORDER BY name LIMIT 2;
+------+---+
|  name|age|
+------+---+
|Anil B| 18|
|Jack N| 16|
+------+---+

-- Specifying ALL option on LIMIT returns all the rows.
SELECT name, age FROM person ORDER BY name LIMIT ALL;
+-------+---+
|   name|age|
+-------+---+
| Anil B| 18|
| Jack N| 16|
| John A| 18|
| Mike A| 25|
|Shone S| 16|
|Zen Hui| 25|
+-------+---+

-- A function expression as an input to LIMIT.
SELECT name, age FROM person ORDER BY name LIMIT length('SPARK');
+-------+---+
|   name|age|
+-------+---+
| Anil B| 18|
| Jack N| 16|
| John A| 18|
| Mike A| 25|
|Shone S| 16|
+-------+---+

-- A non-foldable expression as an input to LIMIT is not allowed.
SELECT name, age FROM person ORDER BY name LIMIT length(name);
org.apache.spark.sql.AnalysisException: The limit expression must evaluate to a constant value ...
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)