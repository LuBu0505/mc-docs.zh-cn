---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: INSERT OVERWRITE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 INSERT OVERWRITE 语法。
ms.openlocfilehash: bde8d92b156f552ad0648f6d06497b399af0c285
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692343"
---
# <a name="insert-overwrite"></a>INSERT OVERWRITE

使用新值覆盖表中的现有数据。 通过值表达式或查询的结果指定插入的行。

## <a name="syntax"></a>语法

```
INSERT OVERWRITE [ TABLE ] table_identifier [ partition_spec [ IF NOT EXISTS ] ]
    { VALUES ( { value | NULL } [ , ... ] ) [ , ( ... ) ] | query }
```

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **partition_spec**

  一个可选参数，用于指定分区键值对的逗号分隔列表。

  **语法：** ``PARTITION ( partition_col_name [ = partition_col_val ] [ , ... ] )``

* **VALUES ( { value | NULL } [ , … ] ) [ , ( … ) ]**

  要插入的值。 可以插入显式指定的值或 NULL。
  必须使用逗号分隔子句中的每个值。 可以指定多个值集以插入多行。

* **查询**

  生成要插入的行的查询。 可以采用以下格式之一：

  * ``SELECT`` 语句
  * ``TABLE`` 语句
  * ``FROM`` 语句

## <a name="examples"></a>示例

### <a name="in-this-section"></a>本节内容：

* [使用 ``VALUES`` 子句进行插入](#insert-using-a-values-clause)
* [使用 ``SELECT`` 语句进行插入](#insert-using-a-select-statement)
* [使用 ``TABLE`` 语句进行插入](#insert-using-a-table-statement)
* [使用 ``FROM`` 语句进行插入](#insert-using-a-from-statement)
* [插入覆盖目录](#insert-overwrite-a-directory)

### <a name="insert-using-a-values-clause"></a>使用 ``VALUES`` 子句进行插入

```sql
-- Assuming the students table has already been created and populated.
SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|    Amy Smith|   123 Park Ave, San Jose|    111111|
|    Bob Brown| 456 Taylor St, Cupertino|    222222|
|Cathy Johnson|  789 Race Ave, Palo Alto|    333333|
|Dora Williams|134 Forest Ave, Melo Park|    444444|
|Fleur Laurent|    345 Copper St, London|    777777|
|Gordon Martin|     779 Lake Ave, Oxford|    888888|
|  Helen Davis|469 Mission St, San Diego|    999999|
|   Jason Wang|    908 Bird St, Saratoga|    121212|
+-------------+-------------------------+----------+

INSERT OVERWRITE students VALUES
    ('Ashua Hill', '456 Erica Ct, Cupertino', 111111),
    ('Brian Reed', '723 Kern Ave, Palo Alto', 222222);

SELECT * FROM students;
+----------+-----------------------+----------+
|      name|                address|student_id|
+----------+-----------------------+----------+
|Ashua Hill|456 Erica Ct, Cupertino|    111111|
|Brian Reed|723 Kern Ave, Palo Alto|    222222|
+----------+-----------------------+----------+
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
|  Eddie Davis|   245 Market St,Milpitas|345678901|
+-------------+-------------------------+---------+

INSERT OVERWRITE students PARTITION (student_id = 222222)
    SELECT name, address FROM persons WHERE name = "Dora Williams";

SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|   Ashua Hill|  456 Erica Ct, Cupertino|    111111|
+-------------+-------------------------+----------+
|Dora Williams|134 Forest Ave, Melo Park|    222222|
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

INSERT OVERWRITE students TABLE visiting_students;

SELECT * FROM students;
+-------------+---------------------+----------+
|         name|              address|student_id|
+-------------+---------------------+----------+
|Fleur Laurent|345 Copper St, London|    777777|
+-------------+---------------------+----------+
|Gordon Martin| 779 Lake Ave, Oxford|    888888|
+-------------+---------------------+----------+
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

INSERT OVERWRITE students
    FROM applicants SELECT name, address, id applicants WHERE qualified = true;

SELECT * FROM students;
+-----------+-------------------------+----------+
|       name|                  address|student_id|
+-----------+-------------------------+----------+
|Helen Davis|469 Mission St, San Diego|    999999|
+-----------+-------------------------+----------+
| Jason Wang|    908 Bird St, Saratoga|    121212|
+-----------+-------------------------+----------+
```

### <a name="insert-overwrite-a-directory"></a>插入覆盖目录

```sql
CREATE TABLE students (name VARCHAR(64), address VARCHAR(64), student_id INT)
    PARTITIONED BY (student_id)
    LOCATION "/mnt/user1/students";

INSERT OVERWRITE delta.`/mnt/user1/students` VALUES
    ('Amy Smith', '123 Park Ave, San Jose', 111111);
SELECT * FROM students;
+-------------+-------------------------+----------+
|         name|                  address|student_id|
+-------------+-------------------------+----------+
|    Amy Smith|   123 Park Ave, San Jose|    111111|
+-------------+-------------------------+----------+
```

## <a name="related-statements"></a>相关语句

* [INSERT INTO](sql-ref-syntax-dml-insert-into.md)