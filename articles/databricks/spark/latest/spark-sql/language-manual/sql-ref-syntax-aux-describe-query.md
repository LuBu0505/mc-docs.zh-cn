---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DESCRIBE QUERY - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 DESCRIBE QUERY 语法。
ms.openlocfilehash: b415c886f940e3b25334e7530cf07a67e6642046
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060712"
---
# <a name="describe-query"></a>DESCRIBE QUERY

返回查询输出的元数据。

## <a name="syntax"></a>语法

```
{ DESC | DESCRIBE } [ QUERY ] input_statement
```

## <a name="parameters"></a>参数

* **QUERY** 此子句是可选的，并可以省略。
* **input_statement**

  生成语句的结果集，可以是以下内容之一：

  * ``SELECT`` 语句
  * ``CTE(Common table expression)`` 语句
  * ``INLINE TABLE`` 语句
  * ``TABLE`` 语句
  * ``FROM`` 语句

  有关查询参数的详细语法，请参阅 [select-statement](sql-ref-syntax-qry-select.md)。

## <a name="examples"></a>示例

```sql
-- Create table `person`
CREATE TABLE person (name STRING , age INT COMMENT 'Age column', address STRING);

-- Returns column metadata information for a simple select query
DESCRIBE QUERY SELECT age, sum(age) FROM person GROUP BY age;
+--------+---------+----------+
|col_name|data_type|   comment|
+--------+---------+----------+
|     age|      int|Age column|
|sum(age)|   bigint|      null|
+--------+---------+----------+

-- Returns column metadata information for common table expression (`CTE`).
DESCRIBE QUERY WITH all_names_cte
    AS (SELECT name from person) SELECT * FROM all_names_cte;
+--------+---------+-------+
|col_name|data_type|comment|
+--------+---------+-------+
|    name|   string|   null|
+--------+---------+-------+

-- Returns column metadata information for an inline table.
DESC QUERY VALUES(100, 'John', 10000.20D) AS employee(id, name, salary);
+--------+---------+-------+
|col_name|data_type|comment|
+--------+---------+-------+
|      id|      int|   null|
|    name|   string|   null|
|  salary|   double|   null|
+--------+---------+-------+

-- Returns column metadata information for `TABLE` statement.
DESC QUERY TABLE person;
+--------+---------+----------+
|col_name|data_type|   comment|
+--------+---------+----------+
|    name|   string|      null|
|     age|      int| Agecolumn|
| address|   string|      null|
+--------+---------+----------+

-- Returns column metadata information for a `FROM` statement.
-- `QUERY` clause is optional and can be omitted.
DESCRIBE FROM person SELECT age;
+--------+---------+----------+
|col_name|data_type|   comment|
+--------+---------+----------+
|     age|      int| Agecolumn|
+--------+---------+----------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)
* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)
* [DESCRIBE FUNCTION](sql-ref-syntax-aux-describe-function.md)