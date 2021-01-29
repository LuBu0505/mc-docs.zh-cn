---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: INSERT INTO - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 INSERT INTO 语法。
ms.openlocfilehash: 413df9cbacfe4a5d66c1b66f8080b9e758230e63
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692344"
---
# <a name="insert-into"></a>INSERT INTO

将新行插入到表中。 通过值表达式或查询的结果指定插入的行。

## <a name="syntax"></a>语法

```
INSERT INTO [ TABLE ] table_identifier [ partition_spec ]
    { VALUES ( { value | NULL } [ , ... ] ) [ , ( ... ) ] | query }
```

> [!NOTE]
>
> 当你 ``INSERT INTO`` Delta 表时，支持架构强制和演变。 如果列的数据类型不能安全地强制转换为 Delta 表的数据类型，则会引发运行时异常。 如果启用了[架构演变](../../delta/delta-batch.md#automatic-schema-update)，则新列可以作为架构的最后一列（或嵌套列）存在，以便架构得以演变。

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **partition_spec**

  一个可选参数，用于指定分区键值对的逗号分隔列表。

  **语法：** ``PARTITION ( partition_col_name  = partition_col_val [ , ... ] )``

* **VALUES ( { value | NULL } [ , … ] ) [ , ( … ) ]**

  要插入的值。 显式指定的值或 ``NULL``。
  使用逗号分隔子句中的每个值。 可以指定多个值集来插入多行。

* **查询**

  生成要插入的行的查询。 可采用以下格式之一：

  * ``SELECT`` 语句
  * ``TABLE`` 语句
  * ``FROM`` 语句

## <a name="examples"></a>示例

### <a name="in-this-section"></a>本节内容：

* [使用 ``VALUES`` 子句进行单行插入](#single-row-insert-using-a-values-clause)
* [使用 ``VALUES`` 子句进行多行插入](#multi-row-insert-using-a-values-clause)
* [使用 ``SELECT`` 语句进行插入](#insert-using-a-select-statement)
* [使用 ``TABLE`` 语句进行插入](#insert-using-a-table-statement)
* [使用 ``FROM`` 语句进行插入](#insert-using-a-from-statement)

### <a name="single-row-insert-using-a-values-clause"></a>使用 ``VALUES`` 子句进行单行插入

```sql
CREATE TABLE students (name VARCHAR(64), address VARCHAR(64), student_id INT)
  PARTITIONED BY (student_id);

INSERT INTO students VALUES
    ('Amy Smith', '123 Park Ave, San Jose', 111111);

SELECT * FROM students;
+---------+---------------------+----------+
|     name|              address|student_id|
+---------+---------------------+----------+
|Amy Smith|123 Park Ave,San Jose|    111111|
+---------+---------------------+----------+
```

### <a name="multi-row-insert-using-a-values-clause"></a>使用 ``VALUES`` 子句进行多行插入

```sql
INSERT INTO students VALUES
    ('Bob Brown', '456 Taylor St, Cupertino', 222222),
    ('Cathy Johnson', '789 Race Ave, Palo Alto', 333333);

SELECT * FROM students;
+-------------+------------------------+----------+
|         name|                 address|student_id|
+-------------+------------------------+----------+
|    Amy Smith|  123 Park Ave, San Jose|    111111|
+-------------+------------------------+----------+
|    Bob Brown|456 Taylor St, Cupertino|    222222|
+-------------+------------------------+----------+
|Cathy Johnson| 789 Race Ave, Palo Alto|    333333|
+--------------+-----------------------+----------+
```

### <a name="insert-using-a-select-statement"></a>使用 ``SELECT`` 语句进行插入

```sql
-- Assuming the persons table has already been created and populated.
SELECT * FROM persons;
+-------------+-------------------------+---------+
|         name|                  address|      ssn|
+-------------+-------------------------+---------+
|Dora Williams|134 Forest Ave, Melo Park|123456789|
+-------------+-------------------------+---------+
|  Eddie Davis|  245 Market St, Milpitas|345678901|
+-------------+-------------------------+---------+

INSERT INTO students PARTITION (student_id = 444444)
    SELECT name, address FROM persons WHERE name = "Dora Williams";

SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|    Amy Smith|   123 Park Ave, San Jose|    111111|
+-------------+-------------------------+----------+
|    Bob Brown| 456 Taylor St, Cupertino|    222222|
+-------------+-------------------------+----------+
|Cathy Johnson|  789 Race Ave, Palo Alto|    333333|
+-------------+-------------------------+----------+
|Dora Williams|134 Forest Ave, Melo Park|    444444|
+-------------+-------------------------+----------+
```

### <a name="insert-using-a-table-statement"></a>使用 ``TABLE`` 语句进行插入

```sql
-- Assuming the visiting_students table has already been created and populated.
SELECT * FROM visiting_students;
+-------------+---------------------+----------+
|         name|              address|student_id|
+-------------+---------------------+----------+
|Fleur Laurent|345 Copper St, London|    777777|
+-------------+---------------------+----------+
|Gordon Martin| 779 Lake Ave, Oxford|    888888|
+-------------+---------------------+----------+

INSERT INTO students TABLE visiting_students;

SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|    Amy Smith|    123 Park Ave,San Jose|    111111|
+-------------+-------------------------+----------+
|    Bob Brown| 456 Taylor St, Cupertino|    222222|
+-------------+-------------------------+----------+
|Cathy Johnson|  789 Race Ave, Palo Alto|    333333|
+-------------+-------------------------+----------+
|Dora Williams|134 Forest Ave, Melo Park|    444444|
+-------------+-------------------------+----------+
|Fleur Laurent|    345 Copper St, London|    777777|
+-------------+-------------------------+----------+
|Gordon Martin|     779 Lake Ave, Oxford|    888888|
+-------------+-------------------------+----------+
```

### <a name="insert-using-a-from-statement"></a>使用 ``FROM`` 语句进行插入

```sql
-- Assuming the applicants table has already been created and populated.
SELECT * FROM applicants;
+-----------+--------------------------+----------+---------+
|       name|                   address|student_id|qualified|
+-----------+--------------------------+----------+---------+
|Helen Davis| 469 Mission St, San Diego|    999999|     true|
+-----------+--------------------------+----------+---------+
|   Ivy King|367 Leigh Ave, Santa Clara|    101010|    false|
+-----------+--------------------------+----------+---------+
| Jason Wang|     908 Bird St, Saratoga|    121212|     true|
+-----------+--------------------------+----------+---------+

INSERT INTO students
     FROM applicants SELECT name, address, id applicants WHERE qualified = true;

SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|    Amy Smith|   123 Park Ave, San Jose|    111111|
+-------------+-------------------------+----------+
|    Bob Brown| 456 Taylor St, Cupertino|    222222|
+-------------+-------------------------+----------+
|Cathy Johnson|  789 Race Ave, Palo Alto|    333333|
+-------------+-------------------------+----------+
|Dora Williams|134 Forest Ave, Melo Park|    444444|
+-------------+-------------------------+----------+
|Fleur Laurent|    345 Copper St, London|    777777|
+-------------+-------------------------+----------+
|Gordon Martin|     779 Lake Ave, Oxford|    888888|
+-------------+-------------------------+----------+
|  Helen Davis|469 Mission St, San Diego|    999999|
+-------------+-------------------------+----------+
|   Jason Wang|    908 Bird St, Saratoga|    121212|
+-------------+-------------------------+----------+
```

#### <a name="insert-into-a-directory"></a>插入到目录中

```sql
CREATE TABLE students (name VARCHAR(64), address VARCHAR(64), student_id INT)
    PARTITIONED BY (student_id)
    LOCATION "/mnt/user1/students";

INSERT INTO delta.`/mnt/user1/students` VALUES
    ('Amy Smith', '123 Park Ave, San Jose', 111111);
SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|    Amy Smith|   123 Park Ave, San Jose|    111111|
+-------------+-------------------------+----------+
```

## <a name="related-statements"></a>相关语句

* [INSERT OVERWRITE](sql-ref-syntax-dml-insert-overwrite-table.md)