---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/06/2020
title: CACHE（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 CACHE SELECT 语法。
ms.openlocfilehash: 142a58aa5d29bf3f12d4d02d4f2be6cbf57285dc
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061175"
---
# <a name="cache-delta-lake-on-azure-databricks"></a>CACHE（Azure Databricks 上的 Delta Lake）

在[增量缓存](../../../../delta/optimizations/delta-cache.md)中缓存由指定的简单 ``SELECT`` 查询访问的数据。 可以通过提供列名称列表来选择要缓存的列的子集，并通过提供谓词来选择行的子集。 这使得后续查询可以尽可能避免扫描原始文件。 此构造仅适用于 Parquet 表。 如上所述，还支持视图，但扩展的查询仅限于简单查询。

## <a name="syntax"></a>语法

```
CACHE SELECT column_name[, column_name, ...] FROM table_identifier [ WHERE boolean_expression ]
```

请参阅[增量和 Apache Spark 缓存](../../../../delta/optimizations/delta-cache.md#delta-and-rdd-cache-comparison)以了解 RDD 缓存和 Databricks IO 缓存之间的差异。

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

## <a name="examples"></a>示例

```sql
CACHE SELECT * FROM boxes
CACHE SELECT width, length FROM boxes WHERE height=3
```