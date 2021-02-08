---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: UNCACHE TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 UNCACHE TABLE 语法。
ms.openlocfilehash: 03043a66853434199cf0ca2a3bcdf97698385f30
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060632"
---
# <a name="uncache-table"></a>UNCACHE TABLE

从给定表或视图的内存和/或磁盘缓存中删除条目和关联数据。 基础条目应该已通过上一 ``CACHE TABLE`` 操作进入缓存。 如果未指定 ``IF EXISTS``，则不存在的表上的 ``UNCACHE TABLE`` 会引发异常。

## <a name="syntax"></a>语法

```sql
UNCACHE TABLE [ IF EXISTS ] table_identifier
```

## <a name="parameters"></a>参数

* **table_identifier**

  要取消缓存的表或视图名称。 可以选择使用数据库名称来限定表或视图的名称。

  **语法：** ``[database_name.] table_name``

## <a name="examples"></a>示例

```sql
UNCACHE TABLE t1;
```

## <a name="related-statements"></a>相关语句

* [CACHE TABLE](sql-ref-syntax-aux-cache-cache-table.md)
* [CLEAR CACHE](sql-ref-syntax-aux-cache-clear-cache.md)
* [REFRESH TABLE](sql-ref-syntax-aux-cache-refresh-table.md)
* [REFRESH](sql-ref-syntax-aux-cache-refresh.md)
* [REFRESH FUNCTION](sql-ref-syntax-aux-cache-refresh-function.md)