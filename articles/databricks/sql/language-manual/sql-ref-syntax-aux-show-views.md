---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW VIEWS - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 SHOW VIEWS 语法。
ms.openlocfilehash: 89b6608f1a7efe98d332e897d1de393f34bdc535
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692382"
---
# <a name="show-views"></a>SHOW VIEWS

返回可选指定数据库的所有视图。
此外，此语句的输出可以通过可选的匹配模式进行筛选。 如果未指定数据库，则从当前数据库返回视图。 如果指定的数据库是全局临时视图数据库，将列出全局临时视图。 请注意，该命令还会列出本地临时视图，而不考虑给定的数据库。

## <a name="syntax"></a>语法

```
SHOW VIEWS [ { FROM | IN } database_name ] [ LIKE regex_pattern ]
```

## <a name="parameters"></a>参数

* **{ FROM | IN } database_name**

  从中列出视图的数据库名称。

* **regex_pattern**

  用于筛选掉不需要的视图的正则表达式模式。

  * 除 ``*`` 和 ``|`` 字符外，该模式的工作方式类似于正则表达式。
  * 只有 ``*`` 则匹配 0 个或多个字符，``|`` 用于分隔多个不同的正则表达式，其中任何一个表达式都可以匹配。
  * 在处理前，在输入模式中删除前导空格和尾随空格。 模式匹配不区分大小写。

## <a name="examples"></a>示例

```sql
-- Create views in different databases, also create global/local temp views.
CREATE VIEW sam AS SELECT id, salary FROM employee WHERE name = 'sam';
CREATE VIEW sam1 AS SELECT id, salary FROM employee WHERE name = 'sam1';
CREATE VIEW suj AS SELECT id, salary FROM employee WHERE name = 'suj';
USE userdb;
CREATE VIEW user1 AS SELECT id, salary FROM default.employee WHERE name = 'user1';
CREATE VIEW user2 AS SELECT id, salary FROM default.employee WHERE name = 'user2';
USE default;
CREATE GLOBAL TEMP VIEW temp1 AS SELECT 1 AS col1;
CREATE TEMP VIEW temp2 AS SELECT 1 AS col1;

-- List all views in default database
SHOW VIEWS;
+-------------+------------+--------------+
| namespace   | viewName   | isTemporary  |
+-------------+------------+--------------+
| default     | sam        | false        |
| default     | sam1       | false        |
| default     | suj        | false        |
|             | temp2      | true         |
+-------------+------------+--------------+

-- List all views from userdb database
SHOW VIEWS FROM userdb;
+-------------+------------+--------------+
| namespace   | viewName   | isTemporary  |
+-------------+------------+--------------+
| userdb      | user1      | false        |
| userdb      | user2      | false        |
|             | temp2      | true         |
+-------------+------------+--------------+

-- List all views in global temp view database
SHOW VIEWS IN global_temp;
+-------------+------------+--------------+
| namespace   | viewName   | isTemporary  |
+-------------+------------+--------------+
| global_temp | temp1      | true         |
|             | temp2      | true         |
+-------------+------------+--------------+

-- List all views from default database matching the pattern `sam*`
SHOW VIEWS FROM default LIKE 'sam*';
+-----------+------------+--------------+
| namespace | viewName   | isTemporary  |
+-----------+------------+--------------+
| default   | sam        | false        |
| default   | sam1       | false        |
+-----------+------------+--------------+

-- List all views from the current database matching the pattern `sam|suj｜temp*`
SHOW VIEWS LIKE 'sam|suj|temp*';
+-------------+------------+--------------+
| namespace   | viewName   | isTemporary  |
+-------------+------------+--------------+
| default     | sam        | false        |
| default     | suj        | false        |
|             | temp2      | true         |
+-------------+------------+--------------+
```

## <a name="related-statements"></a>相关语句

* [CREATE VIEW](sql-ref-syntax-ddl-create-view.md)
* [DROP VIEW](sql-ref-syntax-ddl-drop-view.md)
* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [DROP DATABASE](sql-ref-syntax-ddl-drop-database.md)