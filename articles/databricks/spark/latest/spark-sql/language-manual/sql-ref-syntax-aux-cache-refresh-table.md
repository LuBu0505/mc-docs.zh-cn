---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: REFRESH TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 REFRESH TABLE 语法。
ms.openlocfilehash: fa47b2c469c48aeaa439dfcca63e9260f333d6d2
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060634"
---
# <a name="refresh-table"></a>REFRESH TABLE

使缓存条目失效，其中包含给定表或视图的数据和元数据。 当缓存表或与之关联的查询再次执行时，将以迟缓方式填充无效缓存。

## <a name="syntax"></a>语法

```sql
REFRESH [TABLE] table_identifier
```

## <a name="parameters"></a>参数

* **table_identifier**

  表名，它是指定表或视图的限定或非限定名称。 如果未提供任何数据库标识符，则它会引用当前数据库中的临时视图、表或视图。

  **语法：** ``[database_name.] table_name``

## <a name="examples"></a>示例

```sql
-- The cached entries of the table is refreshed
-- The table is resolved from the current database as the table name is unqualified.
REFRESH TABLE tbl1;

-- The cached entries of the view is refreshed or invalidated
-- The view is resolved from tempDB database, as the view name is qualified.
REFRESH TABLE tempDB.view1;
```

## <a name="related-statements"></a>相关语句

* [CACHE TABLE](sql-ref-syntax-aux-cache-cache-table.md)
* [CLEAR CACHE](sql-ref-syntax-aux-cache-clear-cache.md)
* [UNCACHE TABLE](sql-ref-syntax-aux-cache-uncache-table.md)
* [REFRESH](sql-ref-syntax-aux-cache-refresh.md)
* [REFRESH FUNCTION](sql-ref-syntax-aux-cache-refresh-function.md)