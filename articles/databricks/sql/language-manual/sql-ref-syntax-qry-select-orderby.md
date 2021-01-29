---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ORDER BY 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 ORDER BY 语法。
ms.openlocfilehash: 2503d12880eca1c211e02b9f495a1b5c67255c34
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692253"
---
# <a name="order-by-clause"></a>ORDER BY 子句

返回按用户指定顺序排序的结果行。 与 [SORT BY](sql-ref-syntax-qry-select-sortby.md) 子句不同，此子句可保证输出的总序。

## <a name="syntax"></a>语法

```
ORDER BY { expression [ sort_direction | nulls_sort_oder ] [ , ... ] }
```

## <a name="parameters"></a>参数

* **ORDER BY**

  用逗号分隔的表达式列表以及可选参数 ``sort_direction`` 和 ``nulls_sort_order``（用于对行进行排序）。

* **sort_direction**

  可选择指定是按升序还是降序对行进行排序。 排序方向的有效值为 ``ASC``（升序）和 ``DESC``（降序）。 如果未显式指定排序方向，则默认按升序对行排序。

  **语法：** ``[ ASC | DESC ]``

* **nulls_sort_order**

  可选择指定是在非 NULL 值之前还是之后返回 NULL 值。 如果未指定 ``null_sort_order``，则在排序顺序为 ``ASC`` 时，Null 排在前面；排序顺序为 ``DESC`` 时，Null 排在后面。

  1. 如果指定了 ``NULLS FIRST``，则首先返回 NULL 值，而不考虑排序顺序。
  2. 如果指定了 ``NULLS LAST``，则最后返回 NULL 值，而不考虑排序顺序。

  **语法：** ``[ NULLS { FIRST | LAST } ]``

## <a name="examples"></a>示例

```sql
CREATE TABLE person (id INT, name STRING, age INT);
INSERT INTO person VALUES
    (100, 'John', 30),
    (200, 'Mary', NULL),
    (300, 'Mike', 80),
    (400, 'Jerry', NULL),
    (500, 'Dan',  50);

-- Sort rows by age. By default rows are sorted in ascending manner with NULL FIRST.
SELECT name, age FROM person ORDER BY age;
+-----+----+
| name| age|
+-----+----+
|Jerry|null|
| Mary|null|
| John|  30|
|  Dan|  50|
| Mike|  80|
+-----+----+

-- Sort rows in ascending manner keeping null values to be last.
SELECT name, age FROM person ORDER BY age NULLS LAST;
+-----+----+
| name| age|
+-----+----+
| John|  30|
|  Dan|  50|
| Mike|  80|
| Mary|null|
|Jerry|null|
+-----+----+

-- Sort rows by age in descending manner, which defaults to NULL LAST.
SELECT name, age FROM person ORDER BY age DESC;
+-----+----+
| name| age|
+-----+----+
| Mike|  80|
|  Dan|  50|
| John|  30|
|Jerry|null|
| Mary|null|
+-----+----+

-- Sort rows in ascending manner keeping null values to be first.
SELECT name, age FROM person ORDER BY age DESC NULLS FIRST;
+-----+----+
| name| age|
+-----+----+
|Jerry|null|
| Mary|null|
| Mike|  80|
|  Dan|  50|
| John|  30|
+-----+----+

-- Sort rows based on more than one column with each column having different
-- sort direction.
SELECT * FROM person ORDER BY name ASC, age DESC;
+---+-----+----+
| id| name| age|
+---+-----+----+
|500|  Dan|  50|
|400|Jerry|null|
|100| John|  30|
|200| Mary|null|
|300| Mike|  80|
+---+-----+----+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)