---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE TABLE USING - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 CREATE TABLE USING 语法。
ms.openlocfilehash: cdb9c048a517f11954ab0d747493472d7c254b28
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060928"
---
# <a name="create-table-using"></a>CREATE TABLE USING

使用数据源来定义表。

## <a name="syntax"></a>语法

```sql
CREATE TABLE [ IF NOT EXISTS ] table_identifier
    [ ( col_name1 col_type1 [ COMMENT col_comment1 ], ... ) ]
    USING data_source
    [ OPTIONS ( key1=val1, key2=val2, ... ) ]
    [ PARTITIONED BY ( col_name1, col_name2, ... ) ]
    [ CLUSTERED BY ( col_name3, col_name4, ... )
        [ SORTED BY ( col_name [ ASC | DESC ], ... ) ]
        INTO num_buckets BUCKETS ]
    [ LOCATION path ]
    [ COMMENT table_comment ]
    [ TBLPROPERTIES ( key1=val1, key2=val2, ... ) ]
    [ AS select_statement ]
```

``USING`` 子句和 ``AS SELECT`` 子句之间的子句可按任意顺序出现。 例如，可在 ``TBLPROPERTIES`` 之后写入 ``COMMENT table_comment``。

## <a name="parameters"></a>参数

* **table_identifier**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

* **USING data_source**

  将要用于表的文件格式。 ``data_source`` 必须是 ``TEXT``、``CSV``、``JSON``、``JDBC``、``PARQUET``、``ORC``、``HIVE``、``DELTA`` 或 ``LIBSVM`` 中的一个，或 ``org.apache.spark.sql.sources.DataSourceRegister`` 的自定义实现的完全限定的类名。 当 ``data_source`` 为 ``DELTA`` 时，请参阅[创建 Delta 表](#create-table-delta)中的其他选项。

  支持使用 ``HIVE`` 创建 Hive SerDe 表。 你可以使用 ``OPTIONS`` 子句指定 Hive 特定的 ``file_format`` 和 ``row_format``，这是不区分大小写的字符串映射。 选项键为 ``FILEFORMAT``、``INPUTFORMAT``、``OUTPUTFORMAT``、``SERDE``、``FIELDDELIM``、``ESCAPEDELIM``、``MAPKEYDELIM`` 和 ``LINEDELIM``。

* **PARTITIONED BY**

  请按指定的列对表进行分区。

* **CLUSTERED BY**

  根据指定的列将表上创建的分区使用 Bucket 存储到固定存储桶。 Delta Lake 不支持此子句。

  > [!NOTE]
  >
  > 桶存储是一项优化技术，它使用 Bucket（和桶存储列）来确定数据分区并避免数据无序。

* **SORTED BY**

  数据在 Bucket 中的存储顺序。 默认为升序。 Delta Lake 不支持此子句。

* **LOCATION**

  用于存储表数据的目录路径，可以是分布式存储上的一个路径。

* **OPTIONS**

  用于优化表的行为或配置 ``HIVE`` 表的表选项。 Delta Lake 不支持此子句。

* **COMMENT**

  用于描述表的字符串字面量。

* **TBLPROPERTIES**

  用于标记表定义的键值对列表。

* **AS select_statement**

  使用来自 ``select_statement`` 的数据填充表。

## <a name="create-delta-table"></a><a id="as"> </a><a id="create-delta-table"> </a><a id="create-table-delta"> </a>创建 Delta 表

除标准 ``CREATE TABLE`` 选项外，Delta 表还支持本部分中介绍的选项。

> [!NOTE]
>
> 在 Databricks Runtime 7.0 及更高版本中可用。

```
CREATE [OR REPLACE] table_identifier
  [(col_name1 col_type1 [NOT NULL] [COMMENT col_comment1], ...)]
  USING DELTA
  [LOCATION <path-to-delta-files>]
```

### <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * (Delta Lake) `` delta.`<path-to-table>` ``：在指定路径创建表，无需在元存储中创建条目。
* **OR REPLACE**

  如果已存在表，则将表替换为新的配置。

  > [!NOTE]
  >
  > Databricks 强烈建议使用 ``REPLACE``，而不是删除再重新创建表。

* **NOT NULL**

  指示列值不能为 ``NULL``。 默认设置为允许 ``NULL`` 值。 如果指定了该设置，则对 Delta 表的任何更改都将检查这些 ``NOT NULL`` 约束。

  有关详细信息，请参阅 [NOT NULL 约束](../../../../delta/delta-constraints.md#not-null-constraint)。

* **LOCATION <path-to-delta-files>**

  如果指定的 ``LOCATION`` 已包含存储在 Delta Lake 中的数据，Delta Lake 会执行以下操作：

  * 如果仅指定了表名称和位置，例如：

    ```sql
    CREATE TABLE events
      USING DELTA
      LOCATION '/mnt/delta/events'
    ```

    Hive 元存储中的表会自动继承现有数据的架构、分区和表属性。 此功能可用于将数据“导入”到元存储中。

  * 如果你指定了任何配置（架构、分区或表属性），则 Delta Lake 会验证指定的内容是否与现有数据的配置完全匹配。

  > [!WARNING]
  >
  > 如果指定的配置与数据的配置并非完全匹配，则 Delta Lake 会引发一个描述差异的异常。

## <a name="data-source-interaction"></a>数据源交互

数据源表的作用类似于指向基础数据源的指针。 例如，你可以在 Azure Databricks 中创建表 ``foo``该表指向 MySQL 中使用 JDBC 数据源的表 ``bar``。 读取和写入表 ``foo`` 时，实际上是读取和写入表 ``bar``。

通常 ``CREATE TABLE`` 用于创建“指针”，因此必须确保它指向存在的内容。 文件源（如 Parquet、JSON）例外。 如果未指定 ``LOCATION``，Azure Databricks 会创建默认的表位置。

对于 ``CREATE TABLE AS SELECT``，Azure Databricks 会用输入查询的数据覆盖基础数据源，确保创建的表包含与输入查询完全相同的数据。

## <a name="examples"></a>示例

```sql
--Use data source
CREATE TABLE student (id INT, name STRING, age INT) USING CSV;

--Use data from another table
CREATE TABLE student_copy USING CSV
    AS SELECT * FROM student;

--Omit the USING clause, which uses the default data source (parquet by default)
CREATE TABLE student (id INT, name STRING, age INT);

--Specify table comment and properties
CREATE TABLE student (id INT, name STRING, age INT) USING CSV
    COMMENT 'this is a comment'
    TBLPROPERTIES ('foo'='bar');

--Specify table comment and properties with different clauses order
CREATE TABLE student (id INT, name STRING, age INT) USING CSV
    TBLPROPERTIES ('foo'='bar')
    COMMENT 'this is a comment';

--Create partitioned and bucketed table
CREATE TABLE student (id INT, name STRING, age INT)
    USING CSV
    PARTITIONED BY (age)
    CLUSTERED BY (Id) INTO 4 buckets;
```

## <a name="related-statements"></a>相关语句

* [采用 Hive 格式的 CREATE TABLE](sql-ref-syntax-ddl-create-table-hiveformat.md)
* [CREATE TABLE LIKE](sql-ref-syntax-ddl-create-table-like.md)