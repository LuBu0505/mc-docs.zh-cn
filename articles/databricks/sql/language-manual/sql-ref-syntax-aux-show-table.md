---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/05/2021
title: SHOW TABLE EXTENDED - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 SHOW TABLE EXTENDED 语法。
ms.openlocfilehash: a6b802ed0959a86fce883eb319b20dc448f4e4bc
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692145"
---
# <a name="show-table-extended"></a>SHOW TABLE EXTENDED

显示与给定正则表达式匹配的所有表的信息。
输出包括基本表信息和文件系统信息，如 ``Last Access``、``Created By``、``Type``、``Provider``、``Table Properties``、``Location``、``Serde Library``、``InputFormat``、``OutputFormat``、``Storage Properties``、``Partition Provider``、``Partition Columns`` 和 ``Schema``。

如果存在分区规范，则输出给定分区的特定于文件系统的信息，如 ``Partition Parameters`` 和 ``Partition Statistics``。 无法对分区规范使用表正则表达式。

## <a name="syntax"></a>语法

```
SHOW TABLE EXTENDED [ { IN | FROM } database_name ] LIKE regex_pattern
    [ partition_spec ]
```

## <a name="parameters"></a>参数

* **{ IN | FROM } database_name**

  指定数据库名。 如果未提供，则使用当前数据库。

* **regex_pattern**

  用于筛除不需要的表的正则表达式模式。

  * 除 ``*`` 和 ``|`` 字符外，该模式的工作方式类似于正则表达式。
  * 只有 ``*`` 则匹配 0 个或多个字符，``|`` 用于分隔多个不同的正则表达式，其中任何一个表达式都可以匹配。
  * 在处理前，在输入模式中删除前导空格和尾随空格。 模式匹配不区分大小写。
* **partition_spec**

  分区键值对的逗号分隔列表。 无法对分区规范使用表正则表达式。

  **语法：** ``PARTITION ( partition_col_name = partition_col_val [ , ... ] )``

## <a name="examples"></a>示例

```sql
-- Assumes `employee` table partitioned by column `grade`
CREATE TABLE employee(name STRING, grade INT) PARTITIONED BY (grade);
INSERT INTO employee PARTITION (grade = 1) VALUES ('sam');
INSERT INTO employee PARTITION (grade = 2) VALUES ('suj');

-- Show the details of the table

SHOW TABLE EXTENDED LIKE 'employee';
```

```console
+--------+---------+-----------+--------------------------------------------------------------+
|database|tableName|isTemporary|                         information                          |
+--------+---------+-----------+--------------------------------------------------------------+
|default |employee |false      |Database: default
                                Table: employee
                                Owner: root
                                Created Time: Fri Aug 30 15:10:21 IST 2019
                                Last Access: Thu Jan 01 05:30:00 IST 1970
                                Created By: Spark 3.0.0
                                Type: MANAGED
                                Provider: hive
                                Table Properties: [transient_lastDdlTime=1567158021]
                                Location: file:/opt/spark1/spark/spark-warehouse/employee
                                Serde Library: org.apache.hadoop.hive.serde2.lazy
                                .LazySimpleSerDe
                                InputFormat: org.apache.hadoop.mapred.TextInputFormat
                                OutputFormat: org.apache.hadoop.hive.ql.io
                                .HiveIgnoreKeyTextOutputFormat
                                Storage Properties: [serialization.format=1]
                                Partition Provider: Catalog
                                Partition Columns: [`grade`]
                                Schema: root
                                 |-- name: string (nullable = true)
                                 |-- grade: integer (nullable = true)
+--------+---------+-----------+--------------------------------------------------------------+
```

```sql
-- show multiple table details with pattern matching

SHOW TABLE EXTENDED  LIKE `employe*`;
```

```console
+--------+---------+-----------+--------------------------------------------------------------+
|database|tableName|isTemporary|                         information                          |
+--------+---------+-----------+--------------------------------------------------------------+
|default |employee |false      |Database: default
                                Table: employee
                                Owner: root
                                Created Time: Fri Aug 30 15:10:21 IST 2019
                                Last Access: Thu Jan 01 05:30:00 IST 1970
                                Created By: Spark 3.0.0
                                Type: MANAGED
                                Provider: hive
                                Table Properties: [transient_lastDdlTime=1567158021]
                                Location: file:/opt/spark1/spark/spark-warehouse/employee
                                Serde Library: org.apache.hadoop.hive.serde2.lazy
                                .LazySimpleSerDe
                                InputFormat: org.apache.hadoop.mapred.TextInputFormat
                                OutputFormat: org.apache.hadoop.hive.ql.io
                                .HiveIgnoreKeyTextOutputFormat
                                Storage Properties: [serialization.format=1]
                                Partition Provider: Catalog
                                Partition Columns: [`grade`]
                                Schema: root
                                 |-- name: string (nullable = true)
                                 |-- grade: integer (nullable = true)

|default |employee1|false      |Database: default
                                Table: employee1
                                Owner: root
                                Created Time: Fri Aug 30 15:22:33 IST 2019
                                Last Access: Thu Jan 01 05:30:00 IST 1970
                                Created By: Spark 3.0.0
                                Type: MANAGED
                                Provider: hive
                                Table Properties: [transient_lastDdlTime=1567158753]
                                Location: file:/opt/spark1/spark/spark-warehouse/employee1
                                Serde Library: org.apache.hadoop.hive.serde2.lazy
                                .LazySimpleSerDe
                                InputFormat: org.apache.hadoop.mapred.TextInputFormat
                                OutputFormat: org.apache.hadoop.hive.ql.io
                                .HiveIgnoreKeyTextOutputFormat
                                Storage Properties: [serialization.format=1]
                                Partition Provider: Catalog
                                Schema: root
                                 |-- name: string (nullable = true)
+--------+---------+----------+---------------------------------------------------------------+
```

```sql
-- show partition file system details

SHOW TABLE EXTENDED  IN default LIKE `employee` PARTITION (`grade=1`);
```

```console
+--------+---------+-----------+--------------------------------------------------------------+
|database|tableName|isTemporary|                         information                          |
+--------+---------+-----------+--------------------------------------------------------------+
|default |employee |false      |Partition Values: [grade=1]
                                Location: file:/opt/spark1/spark/spark-warehouse/employee
                                /grade=1
                                Serde Library: org.apache.hadoop.hive.serde2.lazy
                                .LazySimpleSerDe
                                InputFormat: org.apache.hadoop.mapred.TextInputFormat
                                OutputFormat: org.apache.hadoop.hive.ql.io
                                .HiveIgnoreKeyTextOutputFormat
                                Storage Properties: [serialization.format=1]
                                Partition Parameters: {rawDataSize=-1, numFiles=1,
                                transient_lastDdlTime=1567158221, totalSize=4,
                                COLUMN_STATS_ACCURATE=false, numRows=-1}
                                Created Time: Fri Aug 30 15:13:41 IST 2019
                                Last Access: Thu Jan 01 05:30:00 IST 1970
                                Partition Statistics: 4 bytes
+--------+---------+-----------+--------------------------------------------------------------+
```

```sql
-- show partition file system details with regex fail

SHOW TABLE EXTENDED  IN default LIKE `empl*` PARTITION (`grade=1`);
```

```console
Error: Error running query: org.apache.spark.sql.catalyst.analysis.NoSuchTableException:
 Table or view 'emplo*' not found in database 'default'; (state=,code=0)
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)