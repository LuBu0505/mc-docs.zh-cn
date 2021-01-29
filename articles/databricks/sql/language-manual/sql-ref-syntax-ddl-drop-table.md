---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DROP TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 DROP TABLE 语法。
ms.openlocfilehash: b1a715ca7e804bf6db578d7e72a01b7247f6c88f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692331"
---
# <a name="drop-table"></a>DROP TABLE

如果表不是 ``EXTERNAL`` 表，请删除该表并从文件系统中删除与该表关联的目录。 如果该表不存在，则会引发异常。

对于外部表，仅从元存储数据库中删除关联的元数据信息。

## <a name="syntax"></a>语法

```sql
DROP TABLE [ IF EXISTS ] table_identifier
```

## <a name="parameter"></a>参数

* IF EXISTS

  如果已指定，则当表不存在时，不会引发异常。

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

## <a name="examples"></a>示例

```sql
-- Assumes a table named `employeetable` exists.
DROP TABLE employeetable;

-- Assumes a table named `employeetable` exists in the `userdb` database
DROP TABLE userdb.employeetable;

-- Assumes a table named `employeetable` does not exist.
-- Throws exception
DROP TABLE employeetable;
Error: org.apache.spark.sql.AnalysisException: Table or view not found: employeetable;
(state=,code=0)

-- Assumes a table named `employeetable` does not exist,Try with IF EXISTS
-- this time it will not throw exception
DROP TABLE IF EXISTS employeetable;
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [DROP DATABASE](sql-ref-syntax-ddl-drop-database.md)