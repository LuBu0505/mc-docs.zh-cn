---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ALTER TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 ALTER TABLE 语法。
ms.openlocfilehash: 7823e2f771a2eda760c6f66a273ec2deec5d04b5
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692237"
---
# <a name="alter-table"></a>ALTER TABLE

更改表的架构或属性。

## <a name="table-identifier-parameter"></a>表标识符参数

所有语句中的表标识符参数都采用以下形式：

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

## <a name="add-columns"></a>ADD COLUMNS

向现有表添加列。

### <a name="syntax"></a>语法

```sql
ALTER TABLE table_identifier ADD COLUMNS ( col_spec [ , ... ] )
```

### <a name="parameters"></a>参数

* **COLUMNS ( col_spec )**

  要添加的列。

## <a name="add-and-drop-partition"></a>ADD AND DROP PARTITION

### <a name="add-partition"></a>ADD PARTITION

将分区添加到分区表。

#### <a name="syntax"></a>语法

```sql
ALTER TABLE table_identifier ADD [IF NOT EXISTS]
    ( partition_spec [ partition_spec ... ] )
```

#### <a name="parameters"></a>参数

* **partition_spec**

  要添加的分区。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

### <a name="drop-partition"></a>DROP PARTITION

删除表的分区。

#### <a name="syntax"></a>语法

```sql
ALTER TABLE table_identifier DROP [ IF EXISTS ] partition_spec [PURGE]
```

#### <a name="parameters"></a>参数

* **partition_spec**

  要删除的分区。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

## <a name="rename-to"></a>RENAME TO

更改数据库中现有表的名称。

### <a name="syntax"></a>语法

```sql
ALTER TABLE table_name RENAME TO table_name

ALTER TABLE table_identifier partition_spec RENAME TO partition_spec
```

### <a name="parameters"></a>参数

* **table_name**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

* **partition_spec**

  要重命名的分区。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

## <a name="set-and-unset"></a>SET AND UNSET

### <a name="set-table-properties"></a>SET TABLE PROPERTIES

设置和取消设置表属性。 如果已设置属性，将用新属性覆盖旧值。

#### <a name="syntax"></a>语法

```sql
-- Set Table Properties
ALTER TABLE table_identifier SET TBLPROPERTIES ( key1 = val1, key2 = val2, ... )

-- Unset Table Properties
ALTER TABLE table_identifier UNSET TBLPROPERTIES [ IF EXISTS ] ( key1, key2, ... )
```

### <a name="set-serde"></a>SET SERDE

设置 Hive 表中的 ``SERDE`` 或 ``SERDE`` 属性。 如果已设置属性，将用新属性覆盖旧值。

#### <a name="syntax"></a>语法

```sql
-- Set SERDE Properties
ALTER TABLE table_identifier [ partition_spec ]
    SET SERDEPROPERTIES ( key1 = val1, key2 = val2, ... )

ALTER TABLE table_identifier [ partition_spec ] SET SERDE serde_class_name
    [ WITH SERDEPROPERTIES ( key1 = val1, key2 = val2, ... ) ]
```

### <a name="set-location-and-set-file-format"></a>SET LOCATION And SET FILE FORMAT

设置现有表的文件位置和文件格式。

#### <a name="syntax"></a>语法

```sql
-- Changing File Format
ALTER TABLE table_identifier [ partition_spec ] SET FILEFORMAT file_format

-- Changing File Location
ALTER TABLE table_identifier [ partition_spec ] SET LOCATION 'new_location'
```

### <a name="parameters"></a>参数

* **partition_spec**

  必须在其上设置属性的分区。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

* **SERDEPROPERTIES ( key1 = val1, key2 = val2, … )**

  要设置的 SERDE 属性。

## <a name="delta-table-options"></a>Delta 表选项

除标准 ``ALTER TABLE`` 选项外，Delta 表还支持本部分中介绍的选项。

### <a name="in-this-section"></a>本节内容：

* [ADD COLUMNS](#add-columns)
* [CHANGE COLUMN](#change-column)
* [CHANGE COLUMN（Hive 语法）](#change-column-hive-syntax)
* [REPLACE COLUMNS](#replace-columns)

有关添加、更改和替换列示例的详细说明，请参阅[显式更新架构](../../delta/delta-batch.md#explicit-schema-update)。

### <a name="add-columns"></a>ADD COLUMNS

将列（包括嵌套列）添加到现有表中。 如果表中已存在具有相同名称的列或同一嵌套结构，会引发异常。

#### <a name="syntax"></a>语法

```
ALTER TABLE table_identifier ADD COLUMNS (col_name data_type [COMMENT col_comment] [FIRST|AFTER colA_name], ...)

ALTER TABLE table_identifier ADD COLUMNS (col_name.nested_col_name data_type [COMMENT col_comment] [FIRST|AFTER colA_name], ...)
```

### <a name="change-column"></a>CHANGE COLUMN

更改现有表的列定义。 可更改列的数据类型、注释或者为 Null 性，并对列重新排序。

#### <a name="syntax"></a>语法

```sql
ALTER TABLE table_identifier (ALTER|CHANGE) [COLUMN] alterColumnAction

ALTER TABLE table_identifier (ALTER|CHANGE) [COLUMN] alterColumnAction

alterColumnAction:
    : TYPE dataType
    : [COMMENT col_comment]
    : [FIRST|AFTER colA_name]
    : (SET | DROP) NOT NULL
```

### <a name="change-column-hive-syntax"></a>CHANGE COLUMN（Hive 语法）

更改现有表的列定义。 可以更改列的注释并对列重新排序。

#### <a name="syntax"></a>语法

```sql
ALTER TABLE table_identifier CHANGE [COLUMN] col_name col_name data_type [COMMENT col_comment] [FIRST|AFTER colA_name]

ALTER TABLE table_identifier CHANGE [COLUMN] col_name.nested_col_name col_name data_type [COMMENT col_comment] [FIRST|AFTER colA_name]
```

> [!NOTE]
>
> 不得使用 ``CHANGE COLUMN``：
>
> * 要更改复杂数据类型（如结构）的内容。 改用 ``ADD COLUMNS`` 来将新列添加到嵌套字段，或使用 ``ALTER COLUMN`` 更改嵌套列的属性。
> * 若要放宽列的为 Null 性。 请改用 ``ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL``。

### <a name="replace-columns"></a>REPLACE COLUMNS

替换现有表的列定义。 它支持更改列的注释、添加列和对列重新排序。 如果指定的列定义与现有定义不兼容，则引发异常。

#### <a name="syntax"></a>语法

```sql
ALTER TABLE table_name REPLACE COLUMNS (col_name1 col_type1 [COMMENT col_comment1], ...)
```

## <a name="examples"></a>示例

```sql
-- RENAME table
DESC student;
+-----------------------+---------+-------+
|               col_name|data_type|comment|
+-----------------------+---------+-------+
|                   name|   string|   NULL|
|                 rollno|      int|   NULL|
|                    age|      int|   NULL|
|# Partition Information|         |       |
|             # col_name|data_type|comment|
|                    age|      int|   NULL|
+-----------------------+---------+-------+

ALTER TABLE Student RENAME TO StudentInfo;

-- After Renaming the table
DESC StudentInfo;
+-----------------------+---------+-------+
|               col_name|data_type|comment|
+-----------------------+---------+-------+
|                   name|   string|   NULL|
|                 rollno|      int|   NULL|
|                    age|      int|   NULL|
|# Partition Information|         |       |
|             # col_name|data_type|comment|
|                    age|      int|   NULL|
+-----------------------+---------+-------+

-- RENAME partition

SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=10|
|   age=11|
|   age=12|
+---------+

ALTER TABLE default.StudentInfo PARTITION (age='10') RENAME TO PARTITION (age='15');

-- After renaming Partition
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
+---------+

-- Add new columns to a table
DESC StudentInfo;
+-----------------------+---------+-------+
|               col_name|data_type|comment|
+-----------------------+---------+-------+
|                   name|   string|   NULL|
|                 rollno|      int|   NULL|
|                    age|      int|   NULL|
|# Partition Information|         |       |
|             # col_name|data_type|comment|
|                    age|      int|   NULL|
+-----------------------+---------+-------+

ALTER TABLE StudentInfo ADD columns (LastName string, DOB timestamp);

-- After Adding New columns to the table
DESC StudentInfo;
+-----------------------+---------+-------+
|               col_name|data_type|comment|
+-----------------------+---------+-------+
|                   name|   string|   NULL|
|                 rollno|      int|   NULL|
|               LastName|   string|   NULL|
|                    DOB|timestamp|   NULL|
|                    age|      int|   NULL|
|# Partition Information|         |       |
|             # col_name|data_type|comment|
|                    age|      int|   NULL|
+-----------------------+---------+-------+

-- Add a new partition to a table
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
+---------+

ALTER TABLE StudentInfo ADD IF NOT EXISTS PARTITION (age=18);

-- After adding a new partition to the table
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
|   age=18|
+---------+

-- Drop a partition from the table
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
|   age=18|
+---------+

ALTER TABLE StudentInfo DROP IF EXISTS PARTITION (age=18);

-- After dropping the partition of the table
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
+---------+

-- Adding multiple partitions to the table
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
+---------+

ALTER TABLE StudentInfo ADD IF NOT EXISTS PARTITION (age=18) PARTITION (age=20);

-- After adding multiple partitions to the table
SHOW PARTITIONS StudentInfo;
+---------+
|partition|
+---------+
|   age=11|
|   age=12|
|   age=15|
|   age=18|
|   age=20|
+---------+

-- ALTER OR CHANGE COLUMNS
DESC StudentInfo;
+-----------------------+---------+-------+
|               col_name|data_type|comment|
+-----------------------+---------+-------+
|                   name|   string|   NULL|
|                 rollno|      int|   NULL|
|               LastName|   string|   NULL|
|                    DOB|timestamp|   NULL|
|                    age|      int|   NULL|
|# Partition Information|         |       |
|             # col_name|data_type|comment|
|                    age|      int|   NULL|
+-----------------------+---------+-------+

ALTER TABLE StudentInfo ALTER COLUMN name COMMENT "new comment";

--After ALTER or CHANGE COLUMNS
DESC StudentInfo;
+-----------------------+---------+-----------+
|               col_name|data_type|    comment|
+-----------------------+---------+-----------+
|                   name|   string|new comment|
|                 rollno|      int|       NULL|
|               LastName|   string|       NULL|
|                    DOB|timestamp|       NULL|
|                    age|      int|       NULL|
|# Partition Information|         |           |
|             # col_name|data_type|    comment|
|                    age|      int|       NULL|
+-----------------------+---------+-----------+

-- Change the fileformat
ALTER TABLE loc_orc SET fileformat orc;

ALTER TABLE p1 partition (month=2, day=2) SET fileformat parquet;

-- Change the file Location
ALTER TABLE dbx.tab1 PARTITION (a='1', b='2') SET LOCATION '/path/to/part/ways'

-- SET SERDE/ SERDE Properties
ALTER TABLE test_tab SET SERDE 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe';

ALTER TABLE dbx.tab1 SET SERDE 'org.apache.hadoop' WITH SERDEPROPERTIES ('k' = 'v', 'kay' = 'vee')

-- SET TABLE PROPERTIES
ALTER TABLE dbx.tab1 SET TBLPROPERTIES ('winner' = 'loser');

-- SET TABLE COMMENT Using SET PROPERTIES
ALTER TABLE dbx.tab1 SET TBLPROPERTIES ('comment' = 'A table comment.');

-- Alter TABLE COMMENT Using SET PROPERTIES
ALTER TABLE dbx.tab1 SET TBLPROPERTIES ('comment' = 'This is a new comment.');

-- DROP TABLE PROPERTIES
ALTER TABLE dbx.tab1 UNSET TBLPROPERTIES ('winner');
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [DROP TABLE](sql-ref-syntax-ddl-drop-table.md)
* [ALTER VIEW](sql-ref-syntax-ddl-alter-view.md)