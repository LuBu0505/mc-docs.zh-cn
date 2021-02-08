---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE TABLE 使用 Hive 格式 - Azure Databricks
description: 了解如何在 Azure Databricks 中将 CREATE TABLE 与 SQL 语言的 Hive 格式语法一起使用。
ms.openlocfilehash: 0077b956bfee8dfb74b55854d0e5e6c25cee935b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061135"
---
# <a name="create-table-with-hive-format"></a>CREATE TABLE，采用 Hive 格式

使用 Hive 格式定义表。

## <a name="syntax"></a>语法

```sql
CREATE [ EXTERNAL ] TABLE [ IF NOT EXISTS ] table_identifier
    [ ( col_name1[:] col_type1 [ COMMENT col_comment1 ], ... ) ]
    [ COMMENT table_comment ]
    [ PARTITIONED BY ( col_name2[:] col_type2 [ COMMENT col_comment2 ], ... )
        | ( col_name1, col_name2, ... ) ]
    [ ROW FORMAT row_format ]
    [ STORED AS file_format ]
    [ LOCATION path ]
    [ TBLPROPERTIES ( key1=val1, key2=val2, ... ) ]
    [ AS select_statement ]

row_format:
    : SERDE serde_class [ WITH SERDEPROPERTIES (k1=v1, k2=v2, ... ) ]
    | DELIMITED [ FIELDS TERMINATED BY fields_terminated_char [ ESCAPED BY escaped_char ] ]
        [ COLLECTION ITEMS TERMINATED BY collection_items_terminated_char ]
        [ MAP KEYS TERMINATED BY map_key_terminated_char ]
        [ LINES TERMINATED BY row_terminated_char ]
        [ NULL DEFINED AS null_char ]
```

列定义子句和 ``AS SELECT``子句之间的子句可按任意顺序出现。 例如，可在 ``TBLPROPERTIES`` 之后写入 ``COMMENT table_comment``。

## <a name="parameters"></a>参数

* **table_identifier**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

* EXTERNAL

  使用 ``LOCATION`` 中提供的路径定义表。

* **PARTITIONED BY**

  请按指定的列对表进行分区。

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

* **LOCATION**

  用于存储表数据的目录路径，可以是分布式存储上的一个路径。

* **COMMENT**

  用于描述表的字符串字面量。

* **TBLPROPERTIES**

  用于标记表定义的键值对列表。

* **AS select_statement**

  使用 select 语句中的数据填充表。

## <a name="examples"></a>示例

```sql
--Use hive format
CREATE TABLE student (id INT, name STRING, age INT) STORED AS ORC;

--Use data from another table
CREATE TABLE student_copy STORED AS ORC
    AS SELECT * FROM student;

--Specify table comment and properties
CREATE TABLE student (id INT, name STRING, age INT)
    COMMENT 'this is a comment'
    STORED AS ORC
    TBLPROPERTIES ('foo'='bar');

--Specify table comment and properties with different clauses order
CREATE TABLE student (id INT, name STRING, age INT)
    STORED AS ORC
    TBLPROPERTIES ('foo'='bar')
    COMMENT 'this is a comment';

--Create partitioned table
CREATE TABLE student (id INT, name STRING)
    PARTITIONED BY (age INT)
    STORED AS ORC;

--Create partitioned table with different clauses order
CREATE TABLE student (id INT, name STRING)
    STORED AS ORC
    PARTITIONED BY (age INT);

--Use Row Format and file format
CREATE TABLE student (id INT, name STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    STORED AS TEXTFILE;

--Use complex datatype
CREATE EXTERNAL TABLE family(
        name STRING,
        friends ARRAY<STRING>,
        children MAP<STRING, INT>,
        address STRUCT<street: STRING, city: STRING>
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' ESCAPED BY '\\'
    COLLECTION ITEMS TERMINATED BY '_'
    MAP KEYS TERMINATED BY ':'
    LINES TERMINATED BY '\n'
    NULL DEFINED AS 'foonull'
    STORED AS TEXTFILE
    LOCATION '/tmp/family/';

--Use predefined custom SerDe
CREATE TABLE avroExample
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
    STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
        OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
    TBLPROPERTIES ('avro.schema.literal'='{ "namespace": "org.apache.hive",
        "name": "first_schema",
        "type": "record",
        "fields": [
                { "name":"string1", "type":"string" },
                { "name":"string2", "type":"string" }
            ] }');

--Use personalized custom SerDe(we may need to `ADD JAR xxx.jar` first to ensure we can find the serde_class,
--or you may run into `CLASSNOTFOUND` exception)
ADD JAR /tmp/hive_serde_example.jar;

CREATE EXTERNAL TABLE family (id INT, name STRING)
    ROW FORMAT SERDE 'com.ly.spark.serde.SerDeExample'
    STORED AS INPUTFORMAT 'com.ly.spark.example.serde.io.SerDeExampleInputFormat'
        OUTPUTFORMAT 'com.ly.spark.example.serde.io.SerDeExampleOutputFormat'
    LOCATION '/tmp/family/';
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE USING](sql-ref-syntax-ddl-create-table-datasource.md)
* [CREATE TABLE LIKE](sql-ref-syntax-ddl-create-table-like.md)