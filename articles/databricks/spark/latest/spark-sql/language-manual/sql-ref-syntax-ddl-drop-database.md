---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DROP DATABASE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 DROP DATABASE 语法。
ms.openlocfilehash: dd43c7ed5584e3f1dc95461962e13279a441a9b6
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061126"
---
# <a name="drop-database"></a>DROP DATABASE

删除数据库并从文件系统中删除与该数据库关联的目录。 如果系统中不存在该数据库，则会引发异常。

## <a name="syntax"></a>语法

```
DROP { DATABASE | SCHEMA } [ IF EXISTS ] dbname [ RESTRICT | CASCADE ]
```

## <a name="parameters"></a>参数

* **DATABASE | SCHEMA**

  ``DATABASE`` 和 ``SCHEMA`` 意思是相同的，你都可以使用它们。

* IF EXISTS

  如果已指定，则仅在数据库存在时才会引发异常。

* **RESTRICT**

  如果已指定，将限制删除非空数据库，并且默认情况下处于启用状态。

* **CASCADE**

  如果已指定，将删除所有关联的表和函数。

## <a name="examples"></a>示例

```sql
-- Create `inventory_db` Database
CREATE DATABASE inventory_db COMMENT 'This database is used to maintain Inventory';

-- Drop the database and it's tables
DROP DATABASE inventory_db CASCADE;

-- Drop the database using IF EXISTS
DROP DATABASE IF EXISTS inventory_db CASCADE;
```

## <a name="related-statements"></a>相关语句

* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [DESCRIBE DATABASE](sql-ref-syntax-aux-describe-database.md)
* [SHOW DATABASES](sql-ref-syntax-aux-show-databases.md)