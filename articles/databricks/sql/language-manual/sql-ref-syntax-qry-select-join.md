---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: JOIN - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 JOIN 语法。
ms.openlocfilehash: 9f89d86aaaff8d9c743ddfc5dab61f38d0b1f7d0
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692221"
---
# <a name="join"></a>JOIN

根据联接条件合并两个关系中的行。

## <a name="syntax"></a>语法

```
relation { [ join_type ] JOIN relation [ join_criteria ] | NATURAL join_type JOIN relation }
```

## <a name="parameters"></a>参数

* **relation**

  要联接的关系。

* **join_type**

  联接类型。

  **语法：**

  ``[ INNER ] | CROSS | LEFT [ OUTER ] | [ LEFT ] SEMI | RIGHT [ OUTER ] | FULL [ OUTER ] | [ LEFT ] ANTI``

* **join_criteria**

  指定如何将一个关系中的行与另一个关系中的行进行合并。

  **语法：** ``ON boolean_expression | USING ( column_name [ , ... ] )``

  ``boolean_expression``

  返回类型为布尔类型的表达式。

## <a name="join-types"></a>联接类型

### <a name="inner-join"></a>**Inner Join**

返回在两个关系中具有匹配值的行。 默认联接。

**语法：**

``relation [ INNER ] JOIN relation [ join_criteria ]``

### <a name="left-join"></a>**Left Join**

返回左侧关系中的所有值和右侧关系中的匹配值；如果没有匹配值，则追加 ``NULL``。 它也称为左外部联接。

**语法：**

``relation LEFT [ OUTER ] JOIN relation [ join_criteria ]``

### <a name="right-join"></a>**Right Join**

返回右侧关系中的所有值和左侧关系中的匹配值；如果没有匹配值，则追加 ``NULL``。 它也称为右外部联接。

**语法：**

``relation RIGHT [ OUTER ] JOIN relation [ join_criteria ]``

### <a name="full-join"></a>**Full Join**

返回两个关系中的所有值，在没有匹配值的那一侧追加 ``NULL`` 值。 它也称为完全外部联接。

**语法：**

``relation FULL [ OUTER ] JOIN relation [ join_criteria ]``

### <a name="cross-join"></a>**Cross Join**

返回两个关系的笛卡尔乘积。

**语法：**

``relation CROSS JOIN relation [ join_criteria ]``

### <a name="semi-join"></a>**Semi Join**

返回与右侧关系有匹配值的左侧关系的值。 它也称为左半联接。

**语法：**

``relation [ LEFT ] SEMI JOIN relation [ join_criteria ]``

### <a name="anti-join"></a>**Anti Join**

返回与右侧关系没有匹配值的左侧关系的值。 它也称为左反联接。

**语法：**

``relation [ LEFT ] ANTI JOIN relation [ join_criteria ]``

## <a name="examples"></a>示例

```sql
-- Use employee and department tables to demonstrate different type of joins.
SELECT * FROM employee;
+---+-----+------+
| id| name|deptno|
+---+-----+------+
|105|Chloe|     5|
|103| Paul|     3|
|101| John|     1|
|102| Lisa|     2|
|104| Evan|     4|
|106|  Amy|     6|
+---+-----+------+

SELECT * FROM department;
+------+-----------+
|deptno|   deptname|
+------+-----------+
|     3|Engineering|
|     2|      Sales|
|     1|  Marketing|
+------+-----------+

-- Use employee and department tables to demonstrate inner join.
SELECT id, name, employee.deptno, deptname
    FROM employee INNER JOIN department ON employee.deptno = department.deptno;
+---+-----+------+-----------|
| id| name|deptno|   deptname|
+---+-----+------+-----------|
|103| Paul|     3|Engineering|
|101| John|     1|  Marketing|
|102| Lisa|     2|      Sales|
+---+-----+------+-----------|

-- Use employee and department tables to demonstrate left join.
SELECT id, name, employee.deptno, deptname
    FROM employee LEFT JOIN department ON employee.deptno = department.deptno;
+---+-----+------+-----------|
| id| name|deptno|   deptname|
+---+-----+------+-----------|
|105|Chloe|     5|       NULL|
|103| Paul|     3|Engineering|
|101| John|     1|  Marketing|
|102| Lisa|     2|      Sales|
|104| Evan|     4|       NULL|
|106|  Amy|     6|       NULL|
+---+-----+------+-----------|

-- Use employee and department tables to demonstrate right join.
SELECT id, name, employee.deptno, deptname
    FROM employee RIGHT JOIN department ON employee.deptno = department.deptno;
+---+-----+------+-----------|
| id| name|deptno|   deptname|
+---+-----+------+-----------|
|103| Paul|     3|Engineering|
|101| John|     1|  Marketing|
|102| Lisa|     2|      Sales|
+---+-----+------+-----------|

-- Use employee and department tables to demonstrate full join.
SELECT id, name, employee.deptno, deptname
    FROM employee FULL JOIN department ON employee.deptno = department.deptno;
+---+-----+------+-----------|
| id| name|deptno|   deptname|
+---+-----+------+-----------|
|101| John|     1|  Marketing|
|106|  Amy|     6|       NULL|
|103| Paul|     3|Engineering|
|105|Chloe|     5|       NULL|
|104| Evan|     4|       NULL|
|102| Lisa|     2|      Sales|
+---+-----+------+-----------|

-- Use employee and department tables to demonstrate cross join.
SELECT id, name, employee.deptno, deptname FROM employee CROSS JOIN department;
+---+-----+------+-----------|
| id| name|deptno|   deptname|
+---+-----+------+-----------|
|105|Chloe|     5|Engineering|
|105|Chloe|     5|  Marketing|
|105|Chloe|     5|      Sales|
|103| Paul|     3|Engineering|
|103| Paul|     3|  Marketing|
|103| Paul|     3|      Sales|
|101| John|     1|Engineering|
|101| John|     1|  Marketing|
|101| John|     1|      Sales|
|102| Lisa|     2|Engineering|
|102| Lisa|     2|  Marketing|
|102| Lisa|     2|      Sales|
|104| Evan|     4|Engineering|
|104| Evan|     4|  Marketing|
|104| Evan|     4|      Sales|
|106|  Amy|     4|Engineering|
|106|  Amy|     4|  Marketing|
|106|  Amy|     4|      Sales|
+---+-----+------+-----------|

-- Use employee and department tables to demonstrate semi join.
SELECT * FROM employee SEMI JOIN department ON employee.deptno = department.deptno;
+---+-----+------+
| id| name|deptno|
+---+-----+------+
|103| Paul|     3|
|101| John|     1|
|102| Lisa|     2|
+---+-----+------+

-- Use employee and department tables to demonstrate anti join.
SELECT * FROM employee ANTI JOIN department ON employee.deptno = department.deptno;
+---+-----+------+
| id| name|deptno|
+---+-----+------+
|105|Chloe|     5|
|104| Evan|     4|
|106|  Amy|     6|
+---+-----+------+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [提示](sql-ref-syntax-qry-select-hints.md)