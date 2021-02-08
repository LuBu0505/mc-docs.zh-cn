---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: LOAD DATA - Azure Databricks
description: 了解如何在 <Databricks> 中使用 SQL 语言的 LOAD DATA 语法。
ms.openlocfilehash: b8dc329dd5b6b216fabb674cacbec17adccd958b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061228"
---
# <a name="load-data"></a>LOAD DATA

从用户指定的目录或文件将数据加载到 Hive SerDe 表中。 如果指定了某个目录，则会加载该目录中的所有文件。 如果指定了某个文件，则仅加载这一个文件。 此外，``LOAD DATA`` 语句采用可选的分区规范。 指定分区后，数据文件（输入源为目录时）或单个文件（输入源为文件时）都将加载到目标表的分区中。

## <a name="syntax"></a>语法

```sql
LOAD DATA [ LOCAL ] INPATH path [ OVERWRITE ] INTO TABLE table_identifier [ partition_spec ]
```

## <a name="parameters"></a>参数

* **路径**

  文件系统的路径。 可以是绝对路径，也可以是相对路径。

* **table_identifier**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

* **partition_spec**

  一个可选参数，用于指定分区键值对的逗号分隔列表。

  **语法：** ``PARTITION ( partition_col_name = partition_col_val [ , ... ] )``

* **LOCAL**

  如果指定此值，则会导致针对本地文件系统而不是默认文件系统（通常是分布式存储）解析 ``INPATH``。

* **OVERWRITE**

  默认情况下，新数据将追加到该表中。 如果使用了 ``OVERWRITE``，则该表将由新数据覆盖。

## <a name="examples"></a>示例

```sql
-- Example without partition specification.
-- Assuming the students table has already been created and populated.
SELECT * FROM students;
+---------+----------------------+----------+
|     name|               address|student_id|
+---------+----------------------+----------+
|Amy Smith|123 Park Ave, San Jose|    111111|
+---------+----------------------+----------+

CREATE TABLE test_load (name VARCHAR(64), address VARCHAR(64), student_id INT) USING HIVE;

-- Assuming the students table is in '/user/hive/warehouse/'
LOAD DATA LOCAL INPATH '/user/hive/warehouse/students' OVERWRITE INTO TABLE test_load;

SELECT * FROM test_load;
+---------+----------------------+----------+
|     name|               address|student_id|
+---------+----------------------+----------+
|Amy Smith|123 Park Ave, San Jose|    111111|
+---------+----------------------+----------+

-- Example with partition specification.
CREATE TABLE test_partition (c1 INT, c2 INT, c3 INT) PARTITIONED BY (c2, c3);

INSERT INTO test_partition PARTITION (c2 = 2, c3 = 3) VALUES (1);

INSERT INTO test_partition PARTITION (c2 = 5, c3 = 6) VALUES (4);

INSERT INTO test_partition PARTITION (c2 = 8, c3 = 9) VALUES (7);

SELECT * FROM test_partition;
+---+---+---+
| c1| c2| c3|
+---+---+---+
|  1|  2|  3|
|  4|  5|  6|
|  7|  8|  9|
+---+---+---+

CREATE TABLE test_load_partition (c1 INT, c2 INT, c3 INT) USING HIVE PARTITIONED BY (c2, c3);

-- Assuming the test_partition table is in '/user/hive/warehouse/'
LOAD DATA LOCAL INPATH '/user/hive/warehouse/test_partition/c2=2/c3=3'
    OVERWRITE INTO TABLE test_load_partition PARTITION (c2=2, c3=3);

SELECT * FROM test_load_partition;
+---+---+---+
| c1| c2| c3|
+---+---+---+
|  1|  2|  3|
+---+---+---+
```