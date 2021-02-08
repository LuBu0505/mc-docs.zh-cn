---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE FUNCTION - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 CREATE FUNCTION 语法。
ms.openlocfilehash: 25d840c3e9baa86fb3d34f42d5c1b70a046ad480
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060931"
---
# <a name="create-function"></a>CREATE FUNCTION

创建临时或永久函数。 临时函数的范围为会话级别，永久函数在持久性目录中创建，并且可供所有会话使用。
在 ``USING`` 子句中指定的资源在首次执行时可供所有执行程序使用。 除了 SQL 接口，Spark 还支持使用 Scala、Python 和 Java API 创建自定义用户定义的标量函数和聚合函数。 有关详细信息，请参阅[用户定义的标量函数 (UDF)](sql-ref-functions-udf-scalar.md) 和[用户定义的聚合函数 (UDAF)](sql-ref-functions-udf-aggregate.md)。

## <a name="syntax"></a>语法

```sql
CREATE [ OR REPLACE ] [ TEMPORARY ] FUNCTION [ IF NOT EXISTS ]
    function_name AS class_name [ resource_locations ]
```

## <a name="parameters"></a>参数

* **OR REPLACE**

  如果指定，将重新加载该函数的资源。 这主要用于获取对函数实现所做的任何更改。 此参数与 ``IF NOT EXISTS`` 互斥，不能一起指定。

* **TEMPORARY**

  指示要创建的函数范围。 指定 ``TEMPORARY`` 时，创建的函数有效并在当前会话中可见。 在目录中不会为这些类型的函数生成永久条目。

* **IF NOT EXISTS**

  如果指定，则仅在不存在时才创建该函数。 如果系统中已存在指定的函数，则函数的创建将成功（不会引发错误）。 此参数与 ``OR REPLACE`` 互斥，不能一起指定。

* function_name

  要创建的函数的名称。 可选择使用数据库名称来限定函数名称。

  **语法：** ``[database_name.] function_name``

* **class_name**

  提供要创建的函数的实现的类的名称。
  实现类应按如下方式扩展其中一个基类：

  * 应在 ``org.apache.hadoop.hive.ql.exec`` 包中扩展 ``UDF`` 或 ``UDAF``。
  * 应在 ``org.apache.hadoop.hive.ql.udf.generic`` 包中扩展 ``AbstractGenericUDAFResolver``、``GenericUDF`` 或 ``GenericUDTF``。
  * 应在 ``org.apache.spark.sql.expressions`` 包中扩展 ``UserDefinedAggregateFunction``。
* **resource_locations**

  包含函数实现及其依赖项的资源列表。

  **语法：** ``USING { { (JAR | FILE ) resource_uri } , ... }``

## <a name="examples"></a>示例

```sql
-- 1. Create a simple UDF `SimpleUdf` that increments the supplied integral value by 10.
--    import org.apache.hadoop.hive.ql.exec.UDF;
--    public class SimpleUdf extends UDF {
--      public int evaluate(int value) {
--        return value + 10;
--      }
--    }
-- 2. Compile and place it in a JAR file called `SimpleUdf.jar` in /tmp.

-- Create a table called `test` and insert two rows.
CREATE TABLE test(c1 INT);
INSERT INTO test VALUES (1), (2);

-- Create a permanent function called `simple_udf`.
CREATE FUNCTION simple_udf AS 'SimpleUdf'
    USING JAR '/tmp/SimpleUdf.jar';

-- Verify that the function is in the registry.
SHOW USER FUNCTIONS;
+------------------+
|          function|
+------------------+
|default.simple_udf|
+------------------+

-- Invoke the function. Every selected value should be incremented by 10.
SELECT simple_udf(c1) AS function_return_value FROM t1;
+---------------------+
|function_return_value|
+---------------------+
|                   11|
|                   12|
+---------------------+

-- Created a temporary function.
CREATE TEMPORARY FUNCTION simple_temp_udf AS 'SimpleUdf'
    USING JAR '/tmp/SimpleUdf.jar';

-- Verify that the newly created temporary function is in the registry.
-- The temporary function does not have a qualified
-- database associated with it.
SHOW USER FUNCTIONS;
+------------------+
|          function|
+------------------+
|default.simple_udf|
|   simple_temp_udf|
+------------------+

-- 1. Modify `SimpleUdf`'s implementation to add supplied integral value by 20.
--    import org.apache.hadoop.hive.ql.exec.UDF;

--    public class SimpleUdfR extends UDF {
--      public int evaluate(int value) {
--        return value + 20;
--      }
--    }
-- 2. Compile and place it in a jar file called `SimpleUdfR.jar` in /tmp.

-- Replace the implementation of `simple_udf`
CREATE OR REPLACE FUNCTION simple_udf AS 'SimpleUdfR'
    USING JAR '/tmp/SimpleUdfR.jar';

-- Invoke the function. Every selected value should be incremented by 20.
SELECT simple_udf(c1) AS function_return_value FROM t1;
+---------------------+
|function_return_value|
+---------------------+
|                   21|
|                   22|
+---------------------+
```

## <a name="related-statements"></a>相关语句

* [SHOW FUNCTIONS](sql-ref-syntax-aux-show-functions.md)
* [DESCRIBE FUNCTION](sql-ref-syntax-aux-describe-function.md)
* [.DROP FUNCTION](sql-ref-syntax-ddl-drop-function.md)