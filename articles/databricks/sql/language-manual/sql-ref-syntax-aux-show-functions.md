---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW FUNCTIONS - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 SHOW FUNCTIONS 语法。
ms.openlocfilehash: 7dcc5be46b0fa4d436a1afa31a01bb474964bd8d
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692384"
---
# <a name="show-functions"></a>SHOW FUNCTIONS

应用可选 regex 模式后返回函数列表。
鉴于 SQL Analytics 支持的函数数量相当大，此语句与[描述函数](sql-ref-syntax-aux-describe-function.md)结合可用于快速查找函数并了解其用法。 ``LIKE`` 子句是可选的，仅支持与其他系统兼容。

## <a name="syntax"></a>语法

```
SHOW [ function_kind ] FUNCTIONS [ [ LIKE ] { function_name | regex_pattern } ]
```

## <a name="parameters"></a>参数

* **function_kind**

  要搜索的函数的名称空间。 有效的名称空间为：

  * **USER** - 在用户定义的函数中查找函数。
  * **SYSTEM** - 在系统定义的函数中查找函数。
  * **ALL** - 在用户和系统定义的函数之间查找函数。
* function_name

  系统中现有函数的名称。 可选择使用数据库名称来限定函数名称。 如果 ``function_name`` 使用数据库进行限定，则从用户指定的数据库解析该函数，否则从当前数据库解析该函数。

  **语法：** ``[database_name.] function_name``

* **regex_pattern**

  用于筛选语句结果的正则表达式模式。

  * 除 ``*`` 和 ``|`` 字符外，该模式的工作方式类似于正则表达式。
  * 只有 ``*`` 则匹配 0 个或多个字符，``|`` 用于分隔多个不同的正则表达式，其中任何一个表达式都可以匹配。
  * 在处理前，在输入模式中删除前导空格和尾随空格。 模式匹配不区分大小写。

## <a name="examples"></a>示例

```sql
-- List a system function `trim` by searching both user defined and system
-- defined functions.
SHOW FUNCTIONS trim;
+--------+
|function|
+--------+
|    trim|
+--------+

-- List a system function `concat` by searching system defined functions.
SHOW SYSTEM FUNCTIONS concat;
+--------+
|function|
+--------+
|  concat|
+--------+

-- List a qualified function `max` from database `salesdb`.
SHOW SYSTEM FUNCTIONS salesdb.max;
+--------+
|function|
+--------+
|     max|
+--------+

-- List all functions starting with `t`
SHOW FUNCTIONS LIKE 't*';
+-----------------+
|         function|
+-----------------+
|              tan|
|             tanh|
|        timestamp|
|          tinyint|
|           to_csv|
|          to_date|
|          to_json|
|     to_timestamp|
|to_unix_timestamp|
| to_utc_timestamp|
|        transform|
|   transform_keys|
| transform_values|
|        translate|
|             trim|
|            trunc|
|           typeof|
+-----------------+

-- List all functions starting with `yea` or `windo`
SHOW FUNCTIONS LIKE 'yea*|windo*';
+--------+
|function|
+--------+
|  window|
|    year|
+--------+

-- Use normal regex pattern to list function names that has 4 characters
-- with `t` as the starting character.
SHOW FUNCTIONS LIKE 't[a-z][a-z][a-z]';
+--------+
|function|
+--------+
|    tanh|
|    trim|
+--------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE FUNCTION](sql-ref-syntax-aux-describe-function.md)