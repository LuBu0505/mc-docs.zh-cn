---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW DATABASES - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW DATABASES 语法。
ms.openlocfilehash: f7fbbae329e14011228044abadd567ea7907ee26
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061154"
---
# <a name="show-databases"></a>SHOW DATABASES

列出与选择性提供的正则表达式模式匹配的数据库。 如果未提供模式，则命令将列出系统中的所有数据库。
``SCHEMAS`` 和 ``DATABASES`` 的使用可互换，这意味着它们是相同的。

## <a name="syntax"></a>语法

```
SHOW { DATABASES | SCHEMAS } [ LIKE regex_pattern ]
```

## <a name="parameters"></a>参数

* **regex_pattern**

  用于筛选语句结果的正则表达式模式。
  除了 `_`` and ``|`` 字符，模式的工作方式类似于正则表达式。
  * 只有 ``*`` 则匹配 0 个或多个字符，``|` 用于分隔多个不同的正则表达式，其中任何一个表达式都可以匹配。
  * 在处理前，在输入模式中删除前导空格和尾随空格。 模式匹配不区分大小写。

## <a name="examples"></a>示例

```sql
-- Create database. Assumes a database named `default` already exists in
-- the system.
CREATE DATABASE payroll_db;
CREATE DATABASE payments_db;

-- Lists all the databases.
SHOW DATABASES;
+------------+
|databaseName|
+------------+
|     default|
| payments_db|
|  payroll_db|
+------------+

-- Lists databases with name starting with string pattern `pay`
SHOW DATABASES LIKE 'pay*';
+------------+
|databaseName|
+------------+
| payments_db|
|  payroll_db|
+------------+

-- Lists all databases. Keywords SCHEMAS and DATABASES are interchangeable.
SHOW SCHEMAS;
+------------+
|databaseName|
+------------+
|     default|
| payments_db|
|  payroll_db|
+------------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)
* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [ALTER DATABASE](sql-ref-syntax-ddl-alter-database.md)