---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: USE DATABASE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 USE DATABASE 语法。
ms.openlocfilehash: 1ced8135931d84c748799d4b86092e0dc288a08a
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692345"
---
# <a name="use-database"></a>USE DATABASE

设置当前数据库。 设置当前数据库后，将从当前数据库解析由 SQL 引用的非限定数据库项目，例如表、函数和视图。
默认数据库名称为 ``default``。

## <a name="syntax"></a>语法

```sql
USE database_name
```

## <a name="parameter"></a>参数

* **database_name**

  要使用的数据库的名称。 如果该数据库不存在，则会引发异常。

## <a name="examples"></a>示例

```sql
-- Use the 'userdb' which exists.
USE userdb;

-- Use the 'userdb1' which doesn't exist
USE userdb1;
Error: org.apache.spark.sql.catalyst.analysis.NoSuchDatabaseException: Database 'userdb1' not found;
(state=,code=0)
```

## <a name="related-statements"></a>相关语句

* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [DROP DATABASE](sql-ref-syntax-ddl-drop-database.md)
* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)