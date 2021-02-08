---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: HAVING 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 HAVING 语法。
ms.openlocfilehash: aa8e4d256d12950429528c5903ff0f7aba7a24ee
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061226"
---
# <a name="having-clause"></a>HAVING 子句

根据指定条件筛选由 ``GROUP BY`` 生成的结果。 通常与 [GROUP BY](sql-ref-syntax-qry-select-groupby.md) 子句结合使用。

## <a name="syntax"></a>语法

```sql
HAVING boolean_expression
```

## <a name="parameters"></a>参数

* **boolean_expression**

  计算得出 ``boolean`` 结果类型的任何表达式。 使用逻辑运算符（``AND``、``OR``）可以将两个或多个表达式组合在一起。

  **注意**

  ``HAVING`` 子句中指定的表达式只能引用以下内容：

  1. 常量
  2. GROUP BY 中显示的表达式
  3. 聚合函数

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

-- `HAVING` clause referring to column in `GROUP BY`.
SELECT city, sum(quantity) AS sum FROM dealer GROUP BY city HAVING city = 'Fremont';
+-------+---+
|   city|sum|
+-------+---+
|Fremont| 32|
+-------+---+

-- `HAVING` clause referring to aggregate function.
SELECT city, sum(quantity) AS sum FROM dealer GROUP BY city HAVING sum(quantity) > 15;
+-------+---+
|   city|sum|
+-------+---+
| Dublin| 33|
|Fremont| 32|
+-------+---+

-- `HAVING` clause referring to aggregate function by its alias.
SELECT city, sum(quantity) AS sum FROM dealer GROUP BY city HAVING sum > 15;
+-------+---+
|   city|sum|
+-------+---+
| Dublin| 33|
|Fremont| 32|
+-------+---+

-- `HAVING` clause referring to a different aggregate function than what is present in
-- `SELECT` list.
SELECT city, sum(quantity) AS sum FROM dealer GROUP BY city HAVING max(quantity) > 15;
+------+---+
|  city|sum|
+------+---+
|Dublin| 33|
+------+---+

-- `HAVING` clause referring to constant expression.
SELECT city, sum(quantity) AS sum FROM dealer GROUP BY city HAVING 1 > 0 ORDER BY city;
+--------+---+
|    city|sum|
+--------+---+
|  Dublin| 33|
| Fremont| 32|
|San Jose| 13|
+--------+---+

-- `HAVING` clause without a `GROUP BY` clause.
SELECT sum(quantity) AS sum FROM dealer HAVING sum(quantity) > 10;
+---+
|sum|
+---+
| 78|
+---+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)