---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW PARTITIONS - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 SHOW PARTITIONS 语法。
ms.openlocfilehash: 60458b6a2ed2fc8baad15f8b8f2431098e0301d7
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692292"
---
# <a name="show-partitions"></a>SHOW PARTITIONS

列出表的分区。

## <a name="syntax"></a>语法

```sql
SHOW PARTITIONS table_identifier [ partition_spec ]
```

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **partition_spec**

  一个可选参数，用于指定分区键值对的逗号分隔列表。 指定后，将返回与分区规范匹配的分区。

  **语法：** ``PARTITION ( partition_col_name = partition_col_val [ , ... ] )``

## <a name="examples"></a>示例

```sql
-- create a partitioned table and insert a few rows.
USE salesdb;
CREATE TABLE customer(id INT, name STRING) PARTITIONED BY (state STRING, city STRING);
INSERT INTO customer PARTITION (state = 'CA', city = 'Fremont') VALUES (100, 'John');
INSERT INTO customer PARTITION (state = 'CA', city = 'San Jose') VALUES (200, 'Marry');
INSERT INTO customer PARTITION (state = 'AZ', city = 'Peoria') VALUES (300, 'Daniel');

-- Lists all partitions for table `customer`
SHOW PARTITIONS customer;
+----------------------+
|             partition|
+----------------------+
|  state=AZ/city=Peoria|
| state=CA/city=Fremont|
|state=CA/city=San Jose|
+----------------------+

-- Lists all partitions for the qualified table `customer`
SHOW PARTITIONS salesdb.customer;
+----------------------+
|             partition|
+----------------------+
|  state=AZ/city=Peoria|
| state=CA/city=Fremont|
|state=CA/city=San Jose|
+----------------------+

-- Specify a full partition spec to list specific partition
SHOW PARTITIONS customer PARTITION (state = 'CA', city = 'Fremont');
+---------------------+
|            partition|
+---------------------+
|state=CA/city=Fremont|
+---------------------+

-- Specify a partial partition spec to list the specific partitions
SHOW PARTITIONS customer PARTITION (state = 'CA');
+----------------------+
|             partition|
+----------------------+
| state=CA/city=Fremont|
|state=CA/city=San Jose|
+----------------------+

-- Specify a partial spec to list specific partition
SHOW PARTITIONS customer PARTITION (city =  'San Jose');
+----------------------+
|             partition|
+----------------------+
|state=CA/city=San Jose|
+----------------------+
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [INSERT INTO](sql-ref-syntax-dml-insert-into.md)
* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)
* [SHOW TABLE EXTENDED](sql-ref-syntax-aux-show-table.md)