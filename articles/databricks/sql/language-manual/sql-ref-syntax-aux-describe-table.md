---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DESCRIBE TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 DESCRIBE TABLE 语法。
ms.openlocfilehash: 197adebae4c884b0351bcc87d1b05002bbdb50f9
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692308"
---
# <a name="describe-table"></a>DESCRIBE TABLE

返回表的基本元数据信息。 元数据信息包括列名、列类型和列注释。 可根据需要指定分区规范或列名称，以分别返回与分区或列有关的元数据。

## <a name="syntax"></a>语法

```
{ DESC | DESCRIBE } TABLE [EXTENDED] [ format ] table_identifier [ partition_spec ] [ col_name ]
```

**``EXTENDED``**

显示有关指定列的详细信息，包括命令 ``ANALYZE TABLE table_name COMPUTE STATISTICS FOR COLUMNS column_name [column_name, ...]`` 收集的列统计信息。

## <a name="parameters"></a>参数

* **format**

  描述输出的可选格式。 如果指定了 ``EXTENDED``，则返回其他元数据信息（例如父数据库、所有者和访问时间）。

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **partition_spec**

  一个可选参数，用于指定分区键值对的逗号分隔列表。 指定后，将返回其他分区元数据。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

* **col_name**

  一个可选参数，用于指定需说明的列名。
  提供的列名可选择性进行限定。 参数 ``partition_spec`` 和 ``col_name`` 互斥，不能同时指定。 目前不允许指定嵌套列。

  **语法：** ``[database_name.] [ table_name. ] column_name``

## <a name="examples"></a>示例

```sql
-- Creates a table `customer`. Assumes current database is `salesdb`.
CREATE TABLE customer(
        cust_id INT,
        state VARCHAR(20),
        name STRING COMMENT 'Short name'
    )
    USING parquet
    PARTITIONED BY (state);

INSERT INTO customer PARTITION (state = 'AR') VALUES (100, 'Mike');

-- Returns basic metadata information for unqualified table `customer`
DESCRIBE TABLE customer;
+-----------------------+---------+----------+
|               col_name|data_type|   comment|
+-----------------------+---------+----------+
|                cust_id|      int|      null|
|                   name|   string|Short name|
|                  state|   string|      null|
|# Partition Information|         |          |
|             # col_name|data_type|   comment|
|                  state|   string|      null|
+-----------------------+---------+----------+

-- Returns basic metadata information for qualified table `customer`
DESCRIBE TABLE salesdb.customer;
+-----------------------+---------+----------+
|               col_name|data_type|   comment|
+-----------------------+---------+----------+
|                cust_id|      int|      null|
|                   name|   string|Short name|
|                  state|   string|      null|
|# Partition Information|         |          |
|             # col_name|data_type|   comment|
|                  state|   string|      null|
+-----------------------+---------+----------+

-- Returns additional metadata such as parent database, owner, access time etc.
```

```sql
DESCRIBE TABLE EXTENDED customer;
+----------------------------+------------------------------+----------+
|                    col_name|                     data_type|   comment|
+----------------------------+------------------------------+----------+
|                     cust_id|                           int|      null|
|                        name|                        string|Short name|
|                       state|                        string|      null|
|     # Partition Information|                              |          |
|                  # col_name|                     data_type|   comment|
|                       state|                        string|      null|
|                            |                              |          |
|# Detailed Table Information|                              |          |
|                    Database|                       default|          |
|                       Table|                      customer|          |
|                       Owner|                 <TABLE OWNER>|          |
|                Created Time|  Tue Apr 07 22:56:34 JST 2020|          |
|                 Last Access|                       UNKNOWN|          |
|                  Created By|               <SPARK VERSION>|          |
|                        Type|                       MANAGED|          |
|                    Provider|                       parquet|          |
|                    Location|file:/tmp/salesdb.db/custom...|          |
|               Serde Library|org.apache.hadoop.hive.ql.i...|          |
|                 InputFormat|org.apache.hadoop.hive.ql.i...|          |
|                OutputFormat|org.apache.hadoop.hive.ql.i...|          |
|          Partition Provider|                       Catalog|          |
+----------------------------+------------------------------+----------+

-- Returns partition metadata such as partitioning column name, column type and comment.
```sql
DESCRIBE TABLE EXTENDED customer PARTITION (state = 'AR');
```

```
+------------------------------+------------------------------+----------+
|                      col_name|                     data_type|   comment|
+------------------------------+------------------------------+----------+
|                       cust_id|                           int|      null|
|                          name|                        string|Short name|
|                         state|                        string|      null|
|       # Partition Information|                              |          |
|                    # col_name|                     data_type|   comment|
|                         state|                        string|      null|
|                              |                              |          |
|# Detailed Partition Inform...|                              |          |
|                      Database|                       default|          |
|                         Table|                      customer|          |
|              Partition Values|                    [state=AR]|          |
|                      Location|file:/tmp/salesdb.db/custom...|          |
|                 Serde Library|org.apache.hadoop.hive.ql.i...|          |
|                   InputFormat|org.apache.hadoop.hive.ql.i...|          |
|                  OutputFormat|org.apache.hadoop.hive.ql.i...|          |
|            Storage Properties|[serialization.format=1, pa...|          |
|          Partition Parameters|{transient_lastDdlTime=1586...|          |
|                  Created Time|  Tue Apr 07 23:05:43 JST 2020|          |
|                   Last Access|                       UNKNOWN|          |
|          Partition Statistics|                     659 bytes|          |
|                              |                              |          |
|         # Storage Information|                              |          |
|                      Location|file:/tmp/salesdb.db/custom...|          |
|                 Serde Library|org.apache.hadoop.hive.ql.i...|          |
|                   InputFormat|org.apache.hadoop.hive.ql.i...|          |
|                  OutputFormat|org.apache.hadoop.hive.ql.i...|          |
+------------------------------+------------------------------+----------+

-- Returns the metadata for `name` column.
-- Optional `TABLE` clause is omitted and column is fully qualified.
```

```sql
DESCRIBE customer salesdb.customer.name;
```

```
+---------+----------+
|info_name|info_value|
+---------+----------+
| col_name|      name|
|data_type|    string|
|  comment|Short name|
+---------+----------+
```

## <a name="describe-detail-delta-lake-on-azure-databricks"></a><a id="describe-detail"> </a><a id="describe-detail-delta-lake-on-azure-databricks"> </a>DESCRIBE DETAIL（Azure Databricks 上的 Delta Lake）

```sql
DESCRIBE DETAIL [db_name.]table_name

DESCRIBE DETAIL delta.`<path-to-table>`
```

返回架构、分区、表大小等方面的信息。 例如，你可以查看表的当前[读取器和编写器的版本](../../delta/versioning.md#table-version)。

## <a name="related-statements"></a>相关语句

* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)
* [DESCRIBE QUERY](sql-ref-syntax-aux-describe-query.md)
* [DESCRIBE FUNCTION](sql-ref-syntax-aux-describe-function.md)