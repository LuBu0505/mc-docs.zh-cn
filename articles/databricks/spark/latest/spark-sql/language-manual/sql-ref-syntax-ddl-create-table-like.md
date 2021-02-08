---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE TABLE LIKE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 CREATE TABLE LIKE 语法。
ms.openlocfilehash: 4bfbb3547e9d3a1e3aa70e3e4a67528f99ebe304
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061134"
---
# <a name="create-table-like"></a>CREATE TABLE LIKE

定义一个使用现有表或视图的定义和元数据的表。

Delta Lake 不支持 ``CREATE TABLE LIKE``。 请改用 ``CREATE TABLE AS``。 请参阅 [AS](sql-ref-syntax-ddl-create-table-datasource.md#as)。

## <a name="syntax"></a>语法

```sql
CREATE TABLE [ IF NOT EXISTS ] table_identifier LIKE source_table_identifier
    USING data_source
    [ ROW FORMAT row_format ]
    [ STORED AS file_format ]
    [ TBLPROPERTIES ( key1=val1, key2=val2, ... ) ]
    [ LOCATION path ]

row_format:
    : SERDE serde_class [ WITH SERDEPROPERTIES (k1=v1, k2=v2, ... ) ]
    | DELIMITED [ FIELDS TERMINATED BY fields_terminated_char [ ESCAPED BY escaped_char ] ]
        [ COLLECTION ITEMS TERMINATED BY collection_items_terminated_char ]
        [ MAP KEYS TERMINATED BY map_key_terminated_char ]
        [ LINES TERMINATED BY row_terminated_char ]
        [ NULL DEFINED AS null_char ]
```

## <a name="parameters"></a>参数

* **table_identifier**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

* **USING data_source**

  将要用于表的文件格式。 ``data_source`` 必须是 ``TEXT``、``CSV``、``JSON``、``JDBC``、``PARQUET``、``ORC``、``HIVE``、或 ``LIBSVM`` 中的一个，或 ``org.apache.spark.sql.sources.DataSourceRegister`` 的自定义实现的完全限定的类名。

  支持使用 ``HIVE`` 创建 Hive SerDe 表。 你可以使用 ``OPTIONS`` 子句指定 Hive 特定的 ``file_format`` 和 ``row_format``，这是不区分大小写的字符串映射。 选项键为 ``FILEFORMAT``、``INPUTFORMAT``、``OUTPUTFORMAT``、``SERDE``、``FIELDDELIM``、``ESCAPEDELIM``、``MAPKEYDELIM`` 和 ``LINEDELIM``。

* **ROW FORMAT row_format**

  若要指定自定义 SerDe，请将其设置为 ``SERDE`` 并指定自定义 SerDe 和可选 SerDe 属性的完全限定类名。 若要使用本机 SerDe，请将其设置为 ``DELIMITED`` 并指定分隔符、转义字符和空字符等。

* **SERDEPROPERTIES**

  用于标记 SerDe 定义的键值对列表。

* **FIELDS TERMINATED BY**

  定义列分隔符。

* **ESCAPED BY**

  定义转义机制。

* **COLLECTION ITEMS TERMINATED BY**

  定义收集项分隔符。

* **MAP KEYS TERMINATED BY**

  定义映射键分隔符。

* **LINES TERMINATED BY**

  定义行分隔符。

* **NULL DEFINED AS**

  定义 ``NULL`` 的特定值。

* **STORED AS**

  此表的文件格式。 可用格式包括 ``TEXTFILE``、``SEQUENCEFILE``、``RCFILE``、``ORC``、``PARQUET`` 和 ``AVRO``。 或者，你可以通过 ``INPUTFORMAT`` 和 ``OUTPUTFORMAT`` 指定你自己的输入和输出格式。 只能将格式 ``TEXTFILE``、``SEQUENCEFILE`` 和 ``RCFILE`` 用于 ``ROW FORMAT SERDE``，并且只能将 ``TEXTFILE`` 用于 ``ROW FORMAT DELIMITED``。

* **TBLPROPERTIES**

  用于标记表定义的键值对列表。

* **LOCATION**

  用于存储表数据的目录路径，可以是分布式存储上的一个路径。

## <a name="examples"></a>示例

```sql
-- Create table using an existing table
CREATE TABLE Student_Dupli like Student;

-- Create table like using a data source
CREATE TABLE Student_Dupli like Student USING CSV;

-- Table is created as external table at the location specified
CREATE TABLE Student_Dupli like Student location  '/root1/home';

-- Create table like using a rowformat
CREATE TABLE Student_Dupli like Student
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    STORED AS TEXTFILE
    TBLPROPERTIES ('owner'='xxxx');
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE USING](sql-ref-syntax-ddl-create-table-datasource.md)
* [采用 Hive 格式的 CREATE TABLE](sql-ref-syntax-ddl-create-table-hiveformat.md)