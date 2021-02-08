---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ALTER VIEW - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 ALTER VIEW 语法。
ms.openlocfilehash: 15dd45507e38e97ad119de2d35f4696e4a535eb0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060877"
---
# <a name="alter-view"></a>ALTER VIEW

更改与视图关联的元数据。 它可以更改视图的定义、对视图重命名，还可通过设置 ``TBLPROPERTIES`` 来设置和取消设置视图的元数据。

## <a name="rename"></a>RENAME

重命名现有视图。 如果源数据库中已存在新视图名称，则会引发 ``TableAlreadyExistsException``。 此操作不支持跨数据库移动视图。

### <a name="syntax"></a>语法

```sql
ALTER VIEW view_identifier RENAME TO view_identifier
```

### <a name="parameters"></a>参数

* **view_identifier**

  视图名称，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] view_name``

## <a name="set"></a>SET

设置现有视图的一个或多个属性。 这些属性是键值对。 如果属性的键存在，则对应的值将替换为新的值。 如果属性的键不存在，则键值对将被添加到属性中。

### <a name="syntax"></a>语法

```sql
ALTER VIEW view_identifier SET TBLPROPERTIES ( property_key = property_val [ , ... ] )
```

### <a name="parameters"></a>参数

* **view_identifier**

  视图名称，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] view_name``

* **property_key**

  属性键。 键可以包含用点分隔的多个部分。

  **语法：** ``[ key_part1 ] [ .key_part2 ] [ ... ]``

### <a name="unset-view-properties"></a>UNSET 视图属性

删除现有视图的一个或多个属性。 如果指定的键不存在，会引发异常。 请使用 ``IF EXISTS`` 来避免出现异常。

### <a name="syntax"></a>语法

```sql
ALTER VIEW view_identifier UNSET TBLPROPERTIES [ IF EXISTS ]  ( property_key [ , ... ] )
```

### <a name="parameters"></a>参数

* **view_identifier**

  视图名称，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] view_name``

* **property_key**

  属性键。 键可以包含用点分隔的多个部分。

  **语法：** ``[ key_part1 ] [ .key_part2 ] [ ... ]``

## <a name="alter-view-as-select"></a>ALTER VIEW AS SELECT

更改视图的定义。 ``SELECT`` 语句必须有效，且 ``view_identifier`` 必须存在。

### <a name="syntax"></a>语法

```sql
ALTER VIEW view_identifier AS select_statement
```

``ALTER VIEW`` 语句不支持 ``SET SERDE`` 和 ``SET SERDEPROPERTIES`` 属性。

### <a name="parameters"></a>参数

* **view_identifier**

  视图名称，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] view_name``

* select_statement

  视图的定义。 有关详细信息，请参阅 [SELECT](sql-ref-syntax-qry-select.md)。

### <a name="examples"></a>示例

```sql
-- Rename only changes the view name.
-- The source and target databases of the view have to be the same.
-- Use qualified or unqualified name for the source and target view.
ALTER VIEW tempdb1.v1 RENAME TO tempdb1.v2;

-- Verify that the new view is created.
DESCRIBE TABLE EXTENDED tempdb1.v2;
+----------------------------+----------+-------+
|                    col_name|data_type |comment|
+----------------------------+----------+-------+
|                          c1|       int|   null|
|                          c2|    string|   null|
|                            |          |       |
|# Detailed Table Information|          |       |
|                    Database|   tempdb1|       |
|                       Table|        v2|       |
+----------------------------+----------+-------+

-- Before ALTER VIEW SET TBLPROPERTIES
DESC TABLE EXTENDED tempdb1.v2;
+----------------------------+----------+-------+
|                    col_name| data_type|comment|
+----------------------------+----------+-------+
|                          c1|       int|   null|
|                          c2|    string|   null|
|                            |          |       |
|# Detailed Table Information|          |       |
|                    Database|   tempdb1|       |
|                       Table|        v2|       |
|            Table Properties|    [....]|       |
+----------------------------+----------+-------+

-- Set properties in TBLPROPERTIES
ALTER VIEW tempdb1.v2 SET TBLPROPERTIES ('created.by.user' = "John", 'created.date' = '01-01-2001' );

-- Use `DESCRIBE TABLE EXTENDED tempdb1.v2` to verify
DESC TABLE EXTENDED tempdb1.v2;
+----------------------------+-----------------------------------------------------+-------+
|                    col_name|                                            data_type|comment|
+----------------------------+-----------------------------------------------------+-------+
|                          c1|                                                  int|   null|
|                          c2|                                               string|   null|
|                            |                                                     |       |
|# Detailed Table Information|                                                     |       |
|                    Database|                                              tempdb1|       |
|                       Table|                                                   v2|       |
|            Table Properties|[created.by.user=John, created.date=01-01-2001, ....]|       |
+----------------------------+-----------------------------------------------------+-------+

-- Remove the key `created.by.user` and `created.date` from `TBLPROPERTIES`
ALTER VIEW tempdb1.v2 UNSET TBLPROPERTIES ('created.by.user', 'created.date');

--Use `DESC TABLE EXTENDED tempdb1.v2` to verify the changes
DESC TABLE EXTENDED tempdb1.v2;
+----------------------------+----------+-------+
|                    col_name| data_type|comment|
+----------------------------+----------+-------+
|                          c1|       int|   null|
|                          c2|    string|   null|
|                            |          |       |
|# Detailed Table Information|          |       |
|                    Database|   tempdb1|       |
|                       Table|        v2|       |
|            Table Properties|    [....]|       |
+----------------------------+----------+-------+

-- Change the view definition
ALTER VIEW tempdb1.v2 AS SELECT * FROM tempdb1.v1;

-- Use `DESC TABLE EXTENDED` to verify
DESC TABLE EXTENDED tempdb1.v2;
+----------------------------+---------------------------+-------+
|                    col_name|                  data_type|comment|
+----------------------------+---------------------------+-------+
|                          c1|                        int|   null|
|                          c2|                     string|   null|
|                            |                           |       |
|# Detailed Table Information|                           |       |
|                    Database|                    tempdb1|       |
|                       Table|                         v2|       |
|                        Type|                       VIEW|       |
|                   View Text|   select * from tempdb1.v1|       |
|          View Original Text|   select * from tempdb1.v1|       |
+----------------------------+---------------------------+-------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)
* [CREATE VIEW](sql-ref-syntax-ddl-create-view.md)
* [DROP VIEW](sql-ref-syntax-ddl-drop-view.md)
* [SHOW VIEWS](sql-ref-syntax-aux-show-views.md)