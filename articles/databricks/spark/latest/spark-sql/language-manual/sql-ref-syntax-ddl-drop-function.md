---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DROP FUNCTION - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 DROP FUNCTION 语法。
ms.openlocfilehash: 75cc3ad93bbb31bb433d213f7b961f91ca163110
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061064"
---
# <a name="drop-function"></a>.DROP FUNCTION

删除临时或用户定义的函数 (UDF)。 如果该函数不存在，则会引发异常。

## <a name="syntax"></a>语法

```sql
DROP [ TEMPORARY ] FUNCTION [ IF EXISTS ] function_name
```

## <a name="parameters"></a>参数

* function_name

  现有函数的名称。 可选择使用数据库名称来限定函数名称。

  **语法：** ``[database_name.] function_name``

* **TEMPORARY**

  应用于删除 ``TEMPORARY`` 函数。

* IF EXISTS

  如果已指定，则仅在函数存在时才会引发异常。

## <a name="examples"></a>示例

```sql
-- Create a permanent function `test_avg`
CREATE FUNCTION test_avg AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDAFAverage';

-- List user functions
SHOW USER FUNCTIONS;
+----------------+
|        function|
+----------------+
|default.test_avg|
+----------------+

-- Create Temporary function `test_avg`
CREATE TEMPORARY FUNCTION test_avg AS
    'org.apache.hadoop.hive.ql.udf.generic.GenericUDAFAverage';

-- List user functions
SHOW USER FUNCTIONS;
+----------------+
|        function|
+----------------+
|default.test_avg|
|        test_avg|
+----------------+

-- Drop Permanent function
DROP FUNCTION test_avg;

-- Try to drop Permanent function which is not present
DROP FUNCTION test_avg;
Error: Error running query:
org.apache.spark.sql.catalyst.analysis.NoSuchPermanentFunctionException:
Function 'default.test_avg' not found in database 'default'; (state=,code=0)

-- List the functions after dropping, it should list only temporary function
SHOW USER FUNCTIONS;
+--------+
|function|
+--------+
|test_avg|
+--------+

-- Drop Temporary function
DROP TEMPORARY FUNCTION IF EXISTS test_avg;
```

## <a name="related-statements"></a>相关语句

* [CREATE FUNCTION](sql-ref-syntax-ddl-create-function.md)
* [DESCRIBE FUNCTION](sql-ref-syntax-aux-describe-function.md)
* [SHOW FUNCTIONS](sql-ref-syntax-aux-show-functions.md)