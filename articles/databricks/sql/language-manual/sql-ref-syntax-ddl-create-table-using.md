---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE TABLE USING - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 CREATE TABLE USING 语法。
ms.openlocfilehash: 6534069d7cdfb98b253f9edcaaca0562d53374fe
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692233"
---
# <a name="create-table-using"></a>CREATE TABLE USING

使用数据源来定义表。

## <a name="syntax"></a>语法

```sql
CREATE TABLE [ OR REPLACE ] [ IF NOT EXISTS ] table_identifier
    [ ( col_name1 col_type1 [ COMMENT col_comment1 ], ... ) ]
    [ USING data_source ]
    [ OPTIONS ( key1=val1, key2=val2, ... ) ]
    [ PARTITIONED BY ( col_name1, col_name2, ... ) ]
    [ LOCATION path ]
    [ COMMENT table_comment ]
    [ TBLPROPERTIES ( key1=val1, key2=val2, ... ) ]
    [ AS select_statement ]
```

``USING`` 子句和 ``AS SELECT`` 子句之间的子句可按任意顺序出现。 例如，可在 ``TBLPROPERTIES`` 之后写入 ``COMMENT table_comment``。

## <a name="parameters"></a>参数

* **table_identifier**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[ database_name. ] table_name``

* **OR REPLACE**

  如果已存在同名的表，则会使用新的配置替换该表。

  > [!NOTE]
  >
  > Databricks 强烈建议使用 ``OR REPLACE``，而不是删除再重新创建表。

* **IF NOT EXISTS**

  如果已存在同名表，则不会执行任何操作。 ``IF NOT EXISTS`` 无法与 ``OR REPLACE`` 共存，这意味着不允许使用 ``CREATE TABLE OR REPLACE IF NOT EXISTS``。

* **USING data_source**

  将要用于表的文件格式。 ``data_source`` 必须是 ``TEXT``、``CSV``、``JSON``、``JDBC``、``PARQUET``、``ORC`` 或 ``DELTA`` 之一。 如果省略 ``USING``，则默认值为 ``DELTA``。

  > [!IMPORTANT]
  > * 当省略 ``USING`` 或 ``data_source`` 为 ``DELTA`` 时，将应用 [Delta 表选项](#create-table-delta)中的其他选项。
  > * 当 ``data_source`` 为 ``TEXT``、``CSV``、``JSON``、``JDBC``、``PARQUET`` 或 ``ORC`` 时，必须指定 ``LOCATION``，并且不允许使用 ``AS select_statement``。

* **PARTITIONED BY**

  请按指定的列对表进行分区。

* **LOCATION**

  用于存储表数据的目录路径，可以是分布式存储上的一个路径。

* **OPTIONS**

  用于优化表行为的表选项。 当数据源为 ``DELTA`` 时，不支持此子句。

* **COMMENT**

  用于描述表的字符串字面量。

* **TBLPROPERTIES**

  用于标记表定义的键值对列表。

* **AS select_statement**

  使用来自 ``select_statement`` 的数据填充表。

## <a name="delta-table-options"></a><a id="as"> </a><a id="create-table-delta"> </a><a id="delta-table-options"> </a>Delta 表选项

除标准 ``CREATE TABLE`` 选项外，Delta 表还支持本部分中介绍的选项。

```
CREATE ... table_identifier
  [ (col_name1 col_type1 [ NOT NULL ] [ COMMENT col_comment1], ...) ]
  [ USING DELTA ]
  [ LOCATION <path-to-delta-files> ]
```

### <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **NOT NULL**

  指示列值不能为 ``NULL``。 默认设置为允许 ``NULL`` 值。 如果指定了该设置，则对 Delta 表的任何更改都将检查这些 ``NOT NULL`` 约束。

  有关详细信息，请参阅 [NOT NULL 约束](../../delta/delta-constraints.md#not-null-constraint)。

* **LOCATION <path-to-delta-files>**

  如果指定的 ``LOCATION`` 已包含存储在 Delta Lake 中的数据，Delta Lake 会执行以下操作：

  * 如果仅指定了表名称和位置，例如：

    ```sql
    CREATE TABLE events
      LOCATION '/mnt/delta/events'
    ```

    Hive 元存储中的表会自动继承现有数据的架构、分区和表属性。 此功能可用于将数据“导入”到元存储中。

  * 如果你指定了任何配置（架构、分区或表属性），则 Delta Lake 会验证指定的内容是否与现有数据的配置完全匹配。

  > [!WARNING]
  >
  > 如果指定的配置与数据的配置并非完全匹配，则 Delta Lake 会引发一个描述差异的异常。

## <a name="data-source-interaction"></a>数据源交互

数据源表的作用类似于指向基础数据源的指针。 例如，可在 Azure Databricks 中创建指向文件 ``bar`` 的表 ``foo``。 读取和写入表 ``foo`` 时，实际上是读取和写入文件 ``bar``。 如果未指定 ``LOCATION``，Azure Databricks 会创建默认的表位置。

对于 ``CREATE TABLE AS SELECT``，Azure Databricks 会用输入查询的数据覆盖基础数据源，确保创建的表包含与输入查询完全相同的数据。

## <a name="examples"></a>示例

```sql
--Creates a Delta table
CREATE TABLE student (id INT, name STRING, age INT);
--Use data from another table
CREATE TABLE student_copy AS SELECT * FROM student;
--Creates a CSV table from an external directory
CREATE TABLE student USING CSV LOCATION '/mnt/csv_files';
--Specify table comment and properties
CREATE TABLE student (id INT, name STRING, age INT)
    COMMENT 'this is a comment'
    TBLPROPERTIES ('foo'='bar');
--Specify table comment and properties with different clauses order
CREATE TABLE student (id INT, name STRING, age INT)
    TBLPROPERTIES ('foo'='bar')
    COMMENT 'this is a comment';
--Create partitioned table
CREATE TABLE student (id INT, name STRING, age INT)
    PARTITIONED BY (age);
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE LIKE](sql-ref-syntax-ddl-create-table-like.md)