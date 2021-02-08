---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 分析表 - Azure Databricks
description: 了解如何使用“ANALYZE TABLE”... Azure Databricks 中 Apache Spark 2.x SQL 语言的 STATISTICS 语法。
ms.openlocfilehash: e891a858c717d8d520044118444f90b48c388339
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061191"
---
# <a name="analyze-table"></a>分析表

```sql
ANALYZE TABLE [db_name.]table_name COMPUTE STATISTICS [analyze_option]
```

收集有关查询优化器可以使用的表的统计信息，以查找更好的计划。

## <a name="table-statistics"></a>表统计信息

```sql
ANALYZE TABLE [db_name.]table_name COMPUTE STATISTICS [NOSCAN]
```

仅收集表的基本统计信息（行数和以字节为单位的大小）。

**``NOSCAN``**

仅收集不需要扫描整个表的统计信息（即，以字节为单位的大小）。

## <a name="column-statistics"></a>列统计信息

```sql
ANALYZE TABLE [db_name.]table_name COMPUTE STATISTICS FOR COLUMNS col1 [, col2, ...]
```

除表统计信息外，还将收集指定列的列统计信息。

> [!TIP]
>
> 请尽可能使用该命令，因为它能收集更多的统计信息，以便优化器可以找到更好的计划。 请确保收集查询使用的所有列的统计信息。

**请参阅：**

* 使用[描述表](describe-table.md)检查现有统计信息
* [基于成本的优化器](../../../latest/spark-sql/cbo.md)