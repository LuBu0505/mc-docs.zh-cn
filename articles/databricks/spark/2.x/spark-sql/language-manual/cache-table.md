---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 缓存表 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 CACHE TABLE 语法。
ms.openlocfilehash: 527dc695e54f3876d5b3bd745dc9fe93e45782de
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060746"
---
# <a name="cache-table"></a>缓存表

```sql
CACHE [LAZY] TABLE [db_name.]table_name
```

使用 RDD 缓存将表的内容缓存在内存中。 这使得后续查询可以尽可能避免扫描原始文件。

**``LAZY``**

延迟缓存该表，而不是急于扫描整个表。

请参阅[增量和 Apache Spark 缓存](../../../../delta/optimizations/delta-cache.md#delta-and-rdd-cache-comparison)以了解 RDD 缓存和 Databricks IO 缓存之间的差异。