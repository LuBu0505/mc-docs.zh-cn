---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE TABLE LIKE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 CREATE TABLE LIKE 语法。
ms.openlocfilehash: a5e3cb281eb838c357ff40274d8f2d1daa2d31ab
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692235"
---
# <a name="create-table-like"></a>CREATE TABLE LIKE

定义一个使用现有表或视图的定义和元数据的表。

Delta Lake 不支持 ``CREATE TABLE LIKE``。 请改用 ``CREATE TABLE AS``。 请参阅 [AS](sql-ref-syntax-ddl-create-table-using.md#as)。

## <a name="syntax"></a>语法

```sql
CREATE TABLE [ IF NOT EXISTS ] table_identifier LIKE source_table_identifier
    USING data_source
    [ TBLPROPERTIES ( key1=val1, key2=val2, ... ) ]
    LOCATION path
```

## <a name="parameters"></a>参数

* **table_identifier**

  表名，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

* **USING data_source**

  将要用于表的文件格式。 ``data_source`` 必须是 ``TEXT``、``CSV``、``JSON``、``JDBC``、``PARQUET`` 或 ``ORC`` 之一。 另外还必须指定 ``LOCATION``。

* **TBLPROPERTIES**

  用于标记表定义的键值对列表。

* **LOCATION**

  用于存储表数据的目录路径，可以是分布式存储上的一个路径。 创建外部表的位置。

## <a name="examples"></a>示例

```sql
-- Create table using a new location
CREATE TABLE Student_Dupli LIKE Student LOCATION '/mnt/data_files';

-- Create table like using a data source
CREATE TABLE Student_Dupli LIKE Student USING CSV LOCATION '/mnt/csv_files';
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE USING](sql-ref-syntax-ddl-create-table-using.md)