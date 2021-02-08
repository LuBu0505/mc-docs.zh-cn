---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ALTER DATABASE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 ALTER DATABASE 语法。
ms.openlocfilehash: 0db600ebc7340f5a02f1c00982d209875256d935
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061136"
---
# <a name="alter-database"></a>ALTER DATABASE

通过设置 ``DBPROPERTIES`` 更改与数据库关联的元数据。 指定的属性值将替代具有相同属性名的任何现有值。 ``SCHEMA`` 和 ``DATABASE`` 可互换使用，可用一个替代另一个。 如果在系统中找不到该数据库，则系统会发出一条错误消息。 此命令主要用于记录数据库的元数据，可用于审核目的。

## <a name="syntax"></a>语法

```
ALTER { DATABASE | SCHEMA } database_name
    SET DBPROPERTIES ( property_name = property_value [ , ... ] )
```

## <a name="parameters"></a>参数

* **database_name**

  要更改的数据库的名称。

## <a name="examples"></a>示例

```sql
-- Creates a database named `inventory`.
CREATE DATABASE inventory;

-- Alters the database to set properties `Edited-by` and `Edit-date`.
ALTER DATABASE inventory SET DBPROPERTIES ('Edited-by' = 'John', 'Edit-date' = '01/01/2001');

-- Verify that properties are set.
DESCRIBE DATABASE EXTENDED inventory;
+-------------------------+------------------------------------------+
|database_description_item|                database_description_value|
+-------------------------+------------------------------------------+
|            Database Name|                                 inventory|
|              Description|                                          |
|                 Location|   file:/temp/spark-warehouse/inventory.db|
|               Properties|((Edit-date,01/01/2001), (Edited-by,John))|
+-------------------------+------------------------------------------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)