---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW TBLPROPERTIES - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW TBLPROPERTIES 语法。
ms.openlocfilehash: 3563195ed95a1cbb6c95ddc00b6a8a88016bb729
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061140"
---
# <a name="show-tblproperties"></a>SHOW TBLPROPERTIES

鉴于属性键的可选值，返回表属性的值。 如果未指定任何键，则返回所有属性。

## <a name="syntax"></a>语法

```sql
SHOW TBLPROPERTIES table_identifier
   [ ( unquoted_property_key | property_key_as_string_literal ) ]
```

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **unquoted_property_key**

  不加引号的属性键。 键可以包含用点分隔的多个部分。

  **语法：** ``[ key_part1 ] [ .key_part2 ] [ ... ]``

* **property_key_as_string_literal**

  字符串字面量形式的属性键值。

> [!NOTE]
>
> 此语句返回的属性值不包括 spark 和 hive 内部的某些属性。 排除的属性包括：
>
> * 以前缀 ``spark.sql`` 开头的所有属性
> * 属性键，例如 ``EXTERNAL``、``comment``
> * 由 hive 在内部生成的用于存储统计信息的所有属性。 其中一些属性包括：``numFiles``、``numPartitions``、``numRows``。

## <a name="examples"></a>示例

```sql
-- create a table `customer` in database `salesdb`
USE salesdb;
CREATE TABLE customer(cust_code INT, name VARCHAR(100), cust_addr STRING)
    TBLPROPERTIES ('created.by.user' = 'John', 'created.date' = '01-01-2001');

-- show all the user specified properties for table `customer`
SHOW TBLPROPERTIES customer;
+---------------------+----------+
|                  key|     value|
+---------------------+----------+
|      created.by.user|      John|
|         created.date|01-01-2001|
|transient_lastDdlTime|1567554931|
+---------------------+----------+

-- show all the user specified properties for a qualified table `customer`
-- in database `salesdb`
SHOW TBLPROPERTIES salesdb.customer;
+---------------------+----------+
|                  key|     value|
+---------------------+----------+
|      created.by.user|      John|
|         created.date|01-01-2001|
|transient_lastDdlTime|1567554931|
+---------------------+----------+

-- show value for unquoted property key `created.by.user`
SHOW TBLPROPERTIES customer (created.by.user);
+-----+
|value|
+-----+
| John|
+-----+

-- show value for property `created.date`` specified as string literal
SHOW TBLPROPERTIES customer ('created.date');
+----------+
|     value|
+----------+
|01-01-2001|
+----------+
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [ALTER TABLE](sql-ref-syntax-ddl-alter-table.md)
* [SHOW TABLES](sql-ref-syntax-aux-show-tables.md)
* [SHOW TABLE EXTENDED](sql-ref-syntax-aux-show-table.md)