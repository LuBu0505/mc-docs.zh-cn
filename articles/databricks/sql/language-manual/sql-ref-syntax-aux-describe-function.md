---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DESCRIBE FUNCTION - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 DESCRIBE FUNCTION 语法。
ms.openlocfilehash: 1e68090c08fb9d8436fb908432848864bf3196a7
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692263"
---
# <a name="describe-function"></a>DESCRIBE FUNCTION

返回现有函数的基本元数据信息。 元数据信息包括函数名称、实现类和使用详细信息。  如果指定了可选的 ``EXTENDED`` 选项，则将返回基本元数据信息和详细使用信息。

## <a name="syntax"></a>语法

```
{ DESC | DESCRIBE } FUNCTION [ EXTENDED ] function_name
```

## <a name="parameters"></a>参数

* function_name

  系统中现有函数的名称。 可选择使用数据库名称来限定函数名称。 如果 ``function_name`` 使用数据库进行限定，则从用户指定的数据库解析该函数，否则从当前数据库解析该函数。

  **语法：** ``[database_name.] function_name``

## <a name="examples"></a>示例

```sql
-- Describe a builtin scalar function.
-- Returns function name, implementing class and usage
DESC FUNCTION abs;
+-------------------------------------------------------------------+
|function_desc                                                      |
+-------------------------------------------------------------------+
|Function: abs                                                      |
|Class: org.apache.spark.sql.catalyst.expressions.Abs               |
|Usage: abs(expr) - Returns the absolute value of the numeric value.|
+-------------------------------------------------------------------+

-- Describe a builtin scalar function.
-- Returns function name, implementing class and usage and examples.
DESC FUNCTION EXTENDED abs;
+-------------------------------------------------------------------+
|function_desc                                                      |
+-------------------------------------------------------------------+
|Function: abs                                                      |
|Class: org.apache.spark.sql.catalyst.expressions.Abs               |
|Usage: abs(expr) - Returns the absolute value of the numeric value.|
|Extended Usage:                                                    |
|    Examples:                                                      |
|      > SELECT abs(-1);                                            |
|       1                                                           |
|                                                                   |
+-------------------------------------------------------------------+

-- Describe a builtin aggregate function
DESC FUNCTION max;
+--------------------------------------------------------------+
|function_desc                                                 |
+--------------------------------------------------------------+
|Function: max                                                 |
|Class: org.apache.spark.sql.catalyst.expressions.aggregate.Max|
|Usage: max(expr) - Returns the maximum value of `expr`.       |
+--------------------------------------------------------------+

-- Describe a builtin user defined aggregate function
-- Returns function name, implementing class and usage and examples.
DESC FUNCTION EXTENDED explode
+---------------------------------------------------------------+
|function_desc                                                  |
+---------------------------------------------------------------+
|Function: explode                                              |
|Class: org.apache.spark.sql.catalyst.expressions.Explode       |
|Usage: explode(expr) - Separates the elements of array `expr`  |
| into multiple rows, or the elements of map `expr` into        |
| multiple rows and columns. Unless specified otherwise, uses   |
| the default column name `col` for elements of the array or    |
| `key` and `value` for the elements of the map.                |
|Extended Usage:                                                |
|    Examples:                                                  |
|      > SELECT explode(array(10, 20));                         |
|       10                                                      |
|       20                                                      |
+---------------------------------------------------------------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)
* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)
* [DESCRIBE QUERY](sql-ref-syntax-aux-describe-query.md)