---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: WHERE 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 WHERE 语法。
ms.openlocfilehash: 3b97cfe181e403ad9f785312804d1f4132de701d
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692139"
---
# <a name="where-clause"></a>WHERE 子句

根据指定的条件限制查询或子查询的 ``FROM`` 子句的结果。

## <a name="syntax"></a>语法

```sql
WHERE boolean_expression
```

## <a name="parameters"></a>参数

* **boolean_expression**

  计算得出 ``Boolean`` 结果类型的任何表达式。 可使用逻辑运算符（``AND``、``OR``）将两个或多个表达式组合在一起。

## <a name="examples"></a>示例

```sql
CREATE TABLE person (id INT, name STRING, age INT);
INSERT INTO person VALUES
    (100, 'John', 30),
    (200, 'Mary', NULL),
    (300, 'Mike', 80),
    (400, 'Dan',  50);

-- Comparison operator in `WHERE` clause.
SELECT * FROM person WHERE id > 200 ORDER BY id;
+---+----+---+
| id|name|age|
+---+----+---+
|300|Mike| 80|
|400| Dan| 50|
+---+----+---+

-- Comparison and logical operators in `WHERE` clause.
SELECT * FROM person WHERE id = 200 OR id = 300 ORDER BY id;
+---+----+----+
| id|name| age|
+---+----+----+
|200|Mary|null|
|300|Mike|  80|
+---+----+----+

-- IS NULL expression in `WHERE` clause.
SELECT * FROM person WHERE id > 300 OR age IS NULL ORDER BY id;
+---+----+----+
| id|name| age|
+---+----+----+
|200|Mary|null|
|400| Dan|  50|
+---+----+----+

-- Function expression in `WHERE` clause.
SELECT * FROM person WHERE length(name) > 3 ORDER BY id;
+---+----+----+
| id|name| age|
+---+----+----+
|100|John|  30|
|200|Mary|null|
|300|Mike|  80|
+---+----+----+

-- `BETWEEN` expression in `WHERE` clause.
SELECT * FROM person WHERE id BETWEEN 200 AND 300 ORDER BY id;
+---+----+----+
| id|name| age|
+---+----+----+
|200|Mary|null|
|300|Mike|  80|
+---+----+----+

-- Scalar Subquery in `WHERE` clause.
SELECT * FROM person WHERE age > (SELECT avg(age) FROM person);
+---+----+---+
| id|name|age|
+---+----+---+
|300|Mike| 80|
+---+----+---+

-- Correlated Subquery in `WHERE` clause.
SELECT * FROM person AS parent
    WHERE EXISTS (
        SELECT 1 FROM person AS child
        WHERE parent.id = child.id AND child.age IS NULL
    );
+---+----+----+
|id |name|age |
+---+----+----+
|200|Mary|null|
+---+----+----+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)