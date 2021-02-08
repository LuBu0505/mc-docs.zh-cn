---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: PIVOT 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 PIVOT 语法。
ms.openlocfilehash: 9cbbd00446ca0f9bcd050cabc50f5f5954eacf30
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061222"
---
# <a name="pivot-clause"></a>PIVOT 子句

用于数据透视。 可以基于特定列值获取聚合值，这些值将转换为在 ``SELECT`` 子句中使用的多个列。 在表名或子查询之后指定 ``PIVOT`` 子句。

## <a name="syntax"></a>语法

```
PIVOT ( { aggregate_expression [ AS aggregate_expression_alias ] } [ , ... ]
    FOR column_list IN ( expression_list ) )
```

## <a name="parameters"></a>参数

* **aggregate_expression**

  聚合表达式（SUM(a)、COUNT(DISTINCT b) 等）。

* **aggregate_expression_alias**

  聚合表达式的别名。

* column_list

  在 ``FROM`` 子句中包含要替换为新列的列。 我们可以使用括号括住列，例如 ``(c1, c2)``。

* **expression_list**

  指定新列，这些新列用于匹配 ``column_list`` 中的值（作为聚合条件）。 我们也可以为其添加别名。

## <a name="examples"></a>示例

```sql
CREATE TABLE person (id INT, name STRING, age INT, class INT, address STRING);
INSERT INTO person VALUES
    (100, 'John', 30, 1, 'Street 1'),
    (200, 'Mary', NULL, 1, 'Street 2'),
    (300, 'Mike', 80, 3, 'Street 3'),
    (400, 'Dan', 50, 4, 'Street 4');

SELECT * FROM person
    PIVOT (
        SUM(age) AS a, AVG(class) AS c
        FOR name IN ('John' AS john, 'Mike' AS mike)
    );
+------+-----------+---------+---------+---------+---------+
|  id  |  address  | john_a  | john_c  | mike_a  | mike_c  |
+------+-----------+---------+---------+---------+---------+
| 200  | Street 2  | NULL    | NULL    | NULL    | NULL    |
| 100  | Street 1  | 30      | 1.0     | NULL    | NULL    |
| 300  | Street 3  | NULL    | NULL    | 80      | 3.0     |
| 400  | Street 4  | NULL    | NULL    | NULL    | NULL    |
+------+-----------+---------+---------+---------+---------+

SELECT * FROM person
    PIVOT (
        SUM(age) AS a, AVG(class) AS c
        FOR (name, age) IN (('John', 30) AS c1, ('Mike', 40) AS c2)
    );
+------+-----------+-------+-------+-------+-------+
|  id  |  address  | c1_a  | c1_c  | c2_a  | c2_c  |
+------+-----------+-------+-------+-------+-------+
| 200  | Street 2  | NULL  | NULL  | NULL  | NULL  |
| 100  | Street 1  | 30    | 1.0   | NULL  | NULL  |
| 300  | Street 3  | NULL  | NULL  | NULL  | NULL  |
| 400  | Street 4  | NULL  | NULL  | NULL  | NULL  |
+------+-----------+-------+-------+-------+-------+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)