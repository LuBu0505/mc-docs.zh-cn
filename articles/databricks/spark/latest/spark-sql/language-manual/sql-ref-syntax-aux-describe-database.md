---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DESCRIBE DATABASE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 DESCRIBE DATABASE 语法。
ms.openlocfilehash: 23f01defd1a6e3b9a0fe8848511c6aaa5f12b37e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060630"
---
# <a name="describe-database"></a>DESCRIBE DATABASE

返回现有数据库的元数据。 元数据信息包括数据库名称、数据库注释和文件系统上的数据库位置。 如果指定了可选的 ``EXTENDED`` 选项，则它将返回基本元数据信息以及数据库属性。 ``DATABASE`` 和 ``SCHEMA`` 是可互换的。

## <a name="syntax"></a>语法

```
{ DESC | DESCRIBE } DATABASE [ EXTENDED ] db_name
```

## <a name="parameters"></a>参数

* **db_name**：系统中现有数据库或现有架构的名称。 如果该名称不存在，则会引发异常。

## <a name="examples"></a>示例

```sql
-- Create employees DATABASE
CREATE DATABASE employees COMMENT 'For software companies';

-- Describe employees DATABASE.
-- Returns Database Name, Description and Root location of the filesystem
-- for the employees DATABASE.
DESCRIBE DATABASE employees;
+-------------------------+-----------------------------+
|database_description_item|   database_description_value|
+-------------------------+-----------------------------+
|            Database Name|                    employees|
|              Description|       For software companies|
|                 Location|file:/you/Temp/employees.db  |
+-------------------------+-----------------------------+

-- Create employees DATABASE
CREATE DATABASE employees COMMENT 'For software companies';

-- Alter employees database to set DBPROPERTIES
ALTER DATABASE employees SET DBPROPERTIES ('Create-by' = 'Kevin', 'Create-date' = '09/01/2019');

-- Describe employees DATABASE with EXTENDED option to return additional database properties
DESCRIBE DATABASE EXTENDED employees;
+-------------------------+---------------------------------------------+
|database_description_item|                   database_description_value|
+-------------------------+---------------------------------------------+
|            Database Name|                                    employees|
|              Description|                       For software companies|
|                 Location|                file:/you/Temp/employees.db  |
|               Properties|((Create-by,kevin), (Create-date,09/01/2019))|
+-------------------------+---------------------------------------------+

-- Create deployment SCHEMA
CREATE SCHEMA deployment COMMENT 'Deployment environment';

-- Describe deployment, the DATABASE and SCHEMA are interchangeable, your meaning are the same.
DESC DATABASE deployment;
+-------------------------+------------------------------+
|database_description_item|database_description_value    |
+-------------------------+------------------------------+
|            Database Name|                    deployment|
|              Description|        Deployment environment|
|                 Location|file:/you/Temp/deployment.db  |
+-------------------------+------------------------------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE FUNCTION](sql-ref-syntax-aux-describe-function.md)
* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)
* [DESCRIBE QUERY](sql-ref-syntax-aux-describe-query.md)