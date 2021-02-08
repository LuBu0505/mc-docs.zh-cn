---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: REFRESH - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 REFRESH 语法。
ms.openlocfilehash: 294bd820cccdac308ffa5fb17d22daaca0d3d4ea
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060635"
---
# <a name="refresh"></a>REFRESH

使包含给定数据源路径的所有数据集的所有缓存数据（以及关联的元数据）无效并刷新。 路径匹配通过前缀进行，也就是说，``/`` 会使缓存的所有内容无效。

## <a name="syntax"></a>语法

```sql
REFRESH resource_path
```

## <a name="parameters"></a>参数

* **resource_path**

  要刷新的资源的路径。

## <a name="examples"></a>示例

```sql
-- The Path is resolved using the datasource's File Index.
CREATE TABLE test(ID INT) using parquet;
INSERT INTO test SELECT 1000;
CACHE TABLE test;
INSERT INTO test SELECT 100;
REFRESH "hdfs://path/to/table";
```

## <a name="related-statements"></a>相关语句

* [CACHE TABLE](sql-ref-syntax-aux-cache-cache-table.md)
* [CLEAR CACHE](sql-ref-syntax-aux-cache-clear-cache.md)
* [UNCACHE TABLE](sql-ref-syntax-aux-cache-uncache-table.md)
* [REFRESH TABLE](sql-ref-syntax-aux-cache-refresh-table.md)
* [REFRESH FUNCTION](sql-ref-syntax-aux-cache-refresh-function.md)