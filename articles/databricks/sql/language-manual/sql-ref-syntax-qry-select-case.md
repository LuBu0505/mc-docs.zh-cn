---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CASE 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 CASE 语法。
ms.openlocfilehash: 9e8a4d012929e58285f28724939248640e42fd97
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692340"
---
# <a name="case-clause"></a>CASE 子句

使用规则根据指定的条件返回特定结果，这类似于其他编程语言中的 if 和 else 语句。

## <a name="syntax"></a>语法

```
CASE [ expression ] { WHEN boolean_expression THEN then_expression } [ ... ]
    [ ELSE else_expression ]
END
```

## <a name="parameters"></a>参数

* **boolean_expression**

  计算得出 ``boolean`` 结果类型的任何表达式。 使用逻辑运算符（``AND``、``OR``）可以将两个或多个表达式组合在一起。

* **then_expression**

  then 表达式基于 ``boolean_expression`` 条件；``then_expression`` 和 ``else_expression`` 应为同一类型或强制为通用类型。

* **else_expression**

  默认表达式；``then_expression`` 和 ``else_expression`` 应为同一类型或强制为通用类型。

## <a name="examples"></a>示例

```sql
CREATE TABLE person (id INT, name STRING, age INT);
INSERT INTO person VALUES
    (100, 'John', 30),
    (200, 'Mary', NULL),
    (300, 'Mike', 80),
    (400, 'Dan', 50);

SELECT id, CASE WHEN id > 200 THEN 'bigger' ELSE 'small' END FROM person;
+------+--------------------------------------------------+
|  id  | CASE WHEN (id > 200) THEN bigger ELSE small END  |
+------+--------------------------------------------------+
| 100  | small                                            |
| 200  | small                                            |
| 300  | bigger                                           |
| 400  | bigger                                           |
+------+--------------------------------------------------+

SELECT id, CASE id WHEN 100 then 'bigger' WHEN  id > 300 THEN '300' ELSE 'small' END FROM person;
+------+-----------------------------------------------------------------------------------------------+
|  id  | CASE WHEN (id = 100) THEN bigger WHEN (id = CAST((id > 300) AS INT)) THEN 300 ELSE small END  |
+------+-----------------------------------------------------------------------------------------------+
| 100  | bigger                                                                                        |
| 200  | small                                                                                         |
| 300  | small                                                                                         |
| 400  | small                                                                                         |
+------+-----------------------------------------------------------------------------------------------+

SELECT * FROM person
    WHERE
        CASE 1 = 1
            WHEN 100 THEN 'big'
            WHEN 200 THEN 'bigger'
            WHEN 300 THEN 'biggest'
            ELSE 'small'
        END = 'small';
+------+-------+-------+
|  id  | name  |  age  |
+------+-------+-------+
| 100  | John  | 30    |
| 200  | Mary  | NULL  |
| 300  | Mike  | 80    |
| 400  | Dan   | 50    |
+------+-------+-------+
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
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)