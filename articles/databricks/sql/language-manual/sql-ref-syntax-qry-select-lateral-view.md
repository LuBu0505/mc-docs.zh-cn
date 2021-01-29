---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: LATERAL VIEW 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 LATERAL VIEW 语法。
ms.openlocfilehash: 42836057f1fa37f93a3cd8eb6ff372757bab23e8
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692155"
---
# <a name="lateral-view-clause"></a>LATERAL VIEW 子句

与生成器函数（例如 ``EXPLODE``）结合使用，将生成包含一个或多个行的虚拟表。 ``LATERAL VIEW`` 将行应用于每个原始输出行。

## <a name="syntax"></a>语法

```sql
LATERAL VIEW [ OUTER ] generator_function ( expression [ , ... ] ) [ table_alias ] AS column_alias [ , ... ]
```

## <a name="parameters"></a>参数

* **OUTER**

  如果指定了 ``OUTER``，则输入数组/映射为空或为 Null 时返回 Null。

* **generator_function**

  生成器函数（EXPLODE、INLINE 等）。

* table_alias

  ``generator_function`` 的别名，它是可选项。

* column_alias

  列出列别名 ``generator_function``，它可用于输出行。 如果 ``generator_function`` 有多个输出列，则可能有多个别名。

## <a name="examples"></a>示例

```sql
CREATE TABLE person (id INT, name STRING, age INT, class INT, address STRING);
INSERT INTO person VALUES
    (100, 'John', 30, 1, 'Street 1'),
    (200, 'Mary', NULL, 1, 'Street 2'),
    (300, 'Mike', 80, 3, 'Street 3'),
    (400, 'Dan', 50, 4, 'Street 4');

SELECT * FROM person
    LATERAL VIEW EXPLODE(ARRAY(30, 60)) tabelName AS c_age
    LATERAL VIEW EXPLODE(ARRAY(40, 80)) AS d_age;
+------+-------+-------+--------+-----------+--------+--------+
|  id  | name  |  age  | class  |  address  | c_age  | d_age  |
+------+-------+-------+--------+-----------+--------+--------+
| 100  | John  | 30    | 1      | Street 1  | 30     | 40     |
| 100  | John  | 30    | 1      | Street 1  | 30     | 80     |
| 100  | John  | 30    | 1      | Street 1  | 60     | 40     |
| 100  | John  | 30    | 1      | Street 1  | 60     | 80     |
| 200  | Mary  | NULL  | 1      | Street 2  | 30     | 40     |
| 200  | Mary  | NULL  | 1      | Street 2  | 30     | 80     |
| 200  | Mary  | NULL  | 1      | Street 2  | 60     | 40     |
| 200  | Mary  | NULL  | 1      | Street 2  | 60     | 80     |
| 300  | Mike  | 80    | 3      | Street 3  | 30     | 40     |
| 300  | Mike  | 80    | 3      | Street 3  | 30     | 80     |
| 300  | Mike  | 80    | 3      | Street 3  | 60     | 40     |
| 300  | Mike  | 80    | 3      | Street 3  | 60     | 80     |
| 400  | Dan   | 50    | 4      | Street 4  | 30     | 40     |
| 400  | Dan   | 50    | 4      | Street 4  | 30     | 80     |
| 400  | Dan   | 50    | 4      | Street 4  | 60     | 40     |
| 400  | Dan   | 50    | 4      | Street 4  | 60     | 80     |
+------+-------+-------+--------+-----------+--------+--------+

SELECT c_age, COUNT(1) FROM person
    LATERAL VIEW EXPLODE(ARRAY(30, 60)) AS c_age
    LATERAL VIEW EXPLODE(ARRAY(40, 80)) AS d_age
GROUP BY c_age;
+--------+-----------+
| c_age  | count(1)  |
+--------+-----------+
| 60     | 8         |
| 30     | 8         |
+--------+-----------+

SELECT * FROM person
    LATERAL VIEW EXPLODE(ARRAY()) tabelName AS c_age;
+-----+-------+------+--------+----------+--------+
| id  | name  | age  | class  | address  | c_age  |
+-----+-------+------+--------+----------+--------+
+-----+-------+------+--------+----------+--------+

SELECT * FROM person
    LATERAL VIEW OUTER EXPLODE(ARRAY()) tabelName AS c_age;
+------+-------+-------+--------+-----------+--------+
|  id  | name  |  age  | class  |  address  | c_age  |
+------+-------+-------+--------+-----------+--------+
| 100  | John  | 30    | 1      | Street 1  | NULL   |
| 200  | Mary  | NULL  | 1      | Street 2  | NULL   |
| 300  | Mike  | 80    | 3      | Street 3  | NULL   |
| 400  | Dan   | 50    | 4      | Street 4  | NULL   |
+------+-------+-------+--------+-----------+--------+
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
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)