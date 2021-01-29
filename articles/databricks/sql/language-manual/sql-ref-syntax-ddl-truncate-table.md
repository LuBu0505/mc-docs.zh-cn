---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: TRUNCATE TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 TRUNCATE TABLE 语法。
ms.openlocfilehash: 524eb5a3bcd9bbf6a87aecd7be6eec2916143b73
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692346"
---
# <a name="truncate-table"></a>TRUNCATE TABLE

从表或分区中删除所有行。 表不能是视图，也不能是外部表或临时表。 若要同时截断多个分区，请在 ``partition_spec`` 中指定分区。 如果未指定 ``partition_spec``，则删除表中的所有分区。

## <a name="syntax"></a>语法

```sql
TRUNCATE TABLE table_identifier [ partition_spec ]
```

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **partition_spec**

  分区键值对的可选逗号分隔列表。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

## <a name="examples"></a>示例

```sql
-- Create table Student with partition
CREATE TABLE Student (name STRING, rollno INT) PARTITIONED BY (age INT);

SELECT * FROM Student;
+----+------+---+
|name|rollno|age|
+----+------+---+
| ABC|     1| 10|
| DEF|     2| 10|
| XYZ|     3| 12|
+----+------+---+

-- Remove all rows from the table in the specified partition
TRUNCATE TABLE Student partition(age=10);

-- After truncate execution, records belonging to partition age=10 are removed
SELECT * FROM Student;
+----+------+---+
|name|rollno|age|
+----+------+---+
| XYZ|     3| 12|
+----+------+---+

-- Remove all rows from the table from all partitions
TRUNCATE TABLE Student;

SELECT * FROM Student;
+----+------+---+
|name|rollno|age|
+----+------+---+
+----+------+---+
```

## <a name="related-statements"></a>相关语句

* [DROP TABLE](sql-ref-syntax-ddl-drop-table.md)
* [ALTER TABLE](sql-ref-syntax-ddl-alter-table.md)