---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CLEAR CACHE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 CLEAR CACHE 语法。
ms.openlocfilehash: d39291a4e6afe142c54f662a81a40adf7f277e1c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060642"
---
# <a name="clear-cache"></a>CLEAR CACHE

从内存和/或磁盘缓存中删除所有缓存表和视图的条目和关联数据。

## <a name="syntax"></a>语法

```sql
CLEAR CACHE
```

## <a name="examples"></a>示例

```sql
CLEAR CACHE;
```

## <a name="related-statements"></a>相关语句

* [CACHE TABLE](sql-ref-syntax-aux-cache-cache-table.md)
* [UNCACHE TABLE](sql-ref-syntax-aux-cache-uncache-table.md)
* [REFRESH TABLE](sql-ref-syntax-aux-cache-refresh-table.md)
* [REFRESH](sql-ref-syntax-aux-cache-refresh.md)
* [REFRESH FUNCTION](sql-ref-syntax-aux-cache-refresh-function.md)