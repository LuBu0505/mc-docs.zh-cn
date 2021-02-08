---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 删除 Bloom 筛选器索引（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 DROP BLOOMFILTER INDEX 语法。
ms.openlocfilehash: 8fff8840ba844800ce4536811d9e99b5cdaca9cc
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061184"
---
# <a name="drop-bloom-filter-index-delta-lake-on-azure-databricks"></a>删除 Bloom 筛选器索引（Azure Databricks 上的 Delta Lake）

```sql
DROP BLOOMFILTER INDEX
ON [TABLE] table_name
[FOR COLUMNS(columnName1, columnName2, ...)]
```

删除 Bloom 筛选器索引。

如果表名或其中一列不存在，该命令就会失败。 所有与 Bloom 筛选器相关的元数据都将从指定列中删除。

如果表中没有任何 Bloom 筛选器，则在清理表时将清除基础索引文件。