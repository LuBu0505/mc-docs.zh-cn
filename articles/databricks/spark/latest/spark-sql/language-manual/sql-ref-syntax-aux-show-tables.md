---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW TABLES - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW TABLES 语法。
ms.openlocfilehash: 768bb0378b7e39454e4247a54daeffb7f4285eaa
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061141"
---
# <a name="show-tables"></a>SHOW TABLES

返回选择性指定的数据库的所有表。
此外，此语句的输出可以通过可选的匹配模式进行筛选。 如果未指定数据库，则从当前数据库返回表。

## <a name="syntax"></a>语法

```
SHOW TABLES [ { FROM | IN } database_name ] [ LIKE regex_pattern ]
```

## <a name="parameters"></a>参数

* **{ FROM | IN } database_name**

  列出表的数据库名称。

* **regex_pattern**

  用于筛选掉不需要的表的正则表达式模式。

  * 除 ``*`` 和 ``|`` 字符外，该模式的工作方式类似于正则表达式。
  * 只有 ``*`` 则匹配 0 个或多个字符，``|`` 用于分隔多个不同的正则表达式，其中任何一个表达式都可以匹配。
  * 在处理前，在输入模式中删除前导空格和尾随空格。 模式匹配不区分大小写。

## <a name="examples"></a>示例

```sql
-- List all tables in default database
SHOW TABLES;
+--------+---------+-----------+
|database|tableName|isTemporary|
+--------+---------+-----------+
| default|      sam|      false|
| default|     sam1|      false|
| default|      suj|      false|
+--------+---------+-----------+

-- List all tables from userdb database
SHOW TABLES FROM userdb;
+--------+---------+-----------+
|database|tableName|isTemporary|
+--------+---------+-----------+
|  userdb|    user1|      false|
|  userdb|    user2|      false|
+--------+---------+-----------+

-- List all tables in userdb database
SHOW TABLES IN userdb;
+--------+---------+-----------+
|database|tableName|isTemporary|
+--------+---------+-----------+
|  userdb|    user1|      false|
|  userdb|    user2|      false|
+--------+---------+-----------+

-- List all tables from default database matching the pattern `sam*`
SHOW TABLES FROM default LIKE 'sam*';
+--------+---------+-----------+
|database|tableName|isTemporary|
+--------+---------+-----------+
| default|      sam|      false|
| default|     sam1|      false|
+--------+---------+-----------+

-- List all tables matching the pattern `sam*|suj`
SHOW TABLES LIKE 'sam*|suj';
+--------+---------+-----------+
|database|tableName|isTemporary|
+--------+---------+-----------+
| default|      sam|      false|
| default|     sam1|      false|
| default|      suj|      false|
+--------+---------+-----------+
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [DROP TABLE](sql-ref-syntax-ddl-drop-table.md)
* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [DROP DATABASE](sql-ref-syntax-ddl-drop-database.md)