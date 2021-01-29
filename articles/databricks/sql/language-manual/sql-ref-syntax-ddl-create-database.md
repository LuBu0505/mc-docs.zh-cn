---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE DATABASE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 CREATE DATABASE 语法。
ms.openlocfilehash: c972c9875bb922f1858ed1a3446014771ab3f895
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692234"
---
# <a name="create-database"></a>CREATE DATABASE

创建具有指定名称的数据库。 如果已存在同名数据库，会引发异常。

## <a name="syntax"></a>语法

```
CREATE { DATABASE | SCHEMA } [ IF NOT EXISTS ] database_name
    [ COMMENT database_comment ]
    [ LOCATION database_directory ]
    [ WITH DBPROPERTIES ( property_name = property_value [ , ... ] ) ]
```

## <a name="parameters"></a>参数

* **database_name**

  要创建的数据库的名称。

* **IF NOT EXISTS**

  创建具有给定名称的数据库（如果不存在）。 如果已存在同名数据库，则不会执行任何操作。

* **database_directory**

  要在其中创建指定数据库的文件系统的路径。 如果基础文件系统中不存在指定的路径，则使用该路径创建一个目录。 如果未指定位置，则在默认仓库目录中创建数据库，其路径由静态配置 ``spark.sql.warehouse.dir`` 进行配置。

* **database_comment**

  数据库的描述。

* **WITH DBPROPERTIES ( property_name=property_value [ , … ] )**

  以键值对形式表示的数据库属性。

## <a name="examples"></a>示例

```sql
-- Create database `customer_db`. This throws exception if database with name customer_db
-- already exists.
CREATE DATABASE customer_db;

-- Create database `customer_db` only if database with same name doesn't exist.
CREATE DATABASE IF NOT EXISTS customer_db;

-- Create database `customer_db` only if database with same name doesn't exist with
-- `Comments`,`Specific Location` and `Database properties`.
CREATE DATABASE IF NOT EXISTS customer_db COMMENT 'This is customer database' LOCATION '/user'
    WITH DBPROPERTIES (ID=001, Name='John');

-- Verify that properties are set.
DESCRIBE DATABASE EXTENDED customer_db;
+-------------------------+--------------------------+
|database_description_item|database_description_value|
+-------------------------+--------------------------+
|            Database Name|               customer_db|
|              Description| This is customer database|
|                 Location|     hdfs://hacluster/user|
|               Properties|   ((ID,001), (Name,John))|
+-------------------------+--------------------------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)
* [DROP DATABASE](sql-ref-syntax-ddl-drop-database.md)