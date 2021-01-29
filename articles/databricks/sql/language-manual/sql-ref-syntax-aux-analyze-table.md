---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ANALYZE TABLE - Azure Databricks
description: 了解如何使用“ANALYZE TABLE”... Azure Databricks 中 SQL Analytics SQL 语言的 STATISTICS 语法。
ms.openlocfilehash: 2767c4525075162a6f7f4b25f3e39ba6b1b9ff7a
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692369"
---
# <a name="analyze-table"></a>ANALYZE TABLE

收集有关表的统计信息，查询优化器可以使用该表查找更好的查询执行计划。

## <a name="syntax"></a>语法

```sql
ANALYZE TABLE table_identifier [ partition_spec ]
    COMPUTE STATISTICS [ NOSCAN | FOR COLUMNS col [ , ... ] | FOR ALL COLUMNS ]
```

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **partition_spec**

  一个可选参数，用于指定分区键值对的逗号分隔列表。 如果指定，则返回分区统计信息。

  **语法：** ``PARTITION ( partition_col_name [ = partition_col_val ] [ , ... ] )``

* **[ NOSCAN | FOR COLUMNS col [ , … ] | FOR ALL COLUMNS ]**

  如果未指定分析选项，``ANALYZE TABLE`` 将收集表的行数和大小（以字节为单位）。

  * **NOSCAN**

    仅收集表的大小（以字节为单位）（不需要扫描整个表）。

  * **FOR COLUMNS col [ , … ] | FOR ALL COLUMNS**

    收集每个指定列或每列的列统计信息，以及表统计信息。

## <a name="examples"></a>示例

```sql
CREATE TABLE students (name STRING, student_id INT) PARTITIONED BY (student_id);
INSERT INTO students PARTITION (student_id = 111111) VALUES ('Mark');
INSERT INTO students PARTITION (student_id = 222222) VALUES ('John');

ANALYZE TABLE students COMPUTE STATISTICS NOSCAN;

DESC EXTENDED students;
+--------------------+--------------------+-------+
|            col_name|           data_type|comment|
+--------------------+--------------------+-------+
|                name|              string|   null|
|          student_id|                 int|   null|
|                 ...|                 ...|    ...|
|          Statistics|           864 bytes|       |
|                 ...|                 ...|    ...|
|  Partition Provider|             Catalog|       |
+--------------------+--------------------+-------+

ANALYZE TABLE students COMPUTE STATISTICS;

DESC EXTENDED students;
+--------------------+--------------------+-------+
|            col_name|           data_type|comment|
+--------------------+--------------------+-------+
|                name|              string|   null|
|          student_id|                 int|   null|
|                 ...|                 ...|    ...|
|          Statistics|   864 bytes, 2 rows|       |
|                 ...|                 ...|    ...|
|  Partition Provider|             Catalog|       |
+--------------------+--------------------+-------+

ANALYZE TABLE students PARTITION (student_id = 111111) COMPUTE STATISTICS;

DESC EXTENDED students PARTITION (student_id = 111111);
+--------------------+--------------------+-------+
|            col_name|           data_type|comment|
+--------------------+--------------------+-------+
|                name|              string|   null|
|          student_id|                 int|   null|
|                 ...|                 ...|    ...|
|Partition Statistics|   432 bytes, 1 rows|       |
|                 ...|                 ...|    ...|
|        OutputFormat|org.apache.hadoop...|       |
+--------------------+--------------------+-------+

ANALYZE TABLE students COMPUTE STATISTICS FOR COLUMNS name;

DESC EXTENDED students name;
+--------------+----------+
|     info_name|info_value|
+--------------+----------+
|      col_name|      name|
|     data_type|    string|
|       comment|      NULL|
|           min|      NULL|
|           max|      NULL|
|     num_nulls|         0|
|distinct_count|         2|
|   avg_col_len|         4|
|   max_col_len|         4|
|     histogram|      NULL|
+--------------+----------+
```