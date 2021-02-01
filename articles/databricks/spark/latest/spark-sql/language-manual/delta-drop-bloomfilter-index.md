---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/06/2020
title: DROP BLOOM FILTER INDEX（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 CREATE BLOOMFILTER INDEX 语法。
ms.openlocfilehash: 683f183dfc7be04d8b58712193a233ba0a108a6c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061160"
---
# <a name="drop-bloom-filter-index-delta-lake-on-azure-databricks"></a>DROP BLOOM FILTER INDEX（Azure Databricks 上的 Delta Lake）

删除 Bloom 筛选器索引。

## <a name="syntax"></a>语法

```
DROP BLOOMFILTER INDEX
ON [TABLE] table_identifier
[FOR COLUMNS(columnName1, columnName2, ...)]
```

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

如果表名或其中一列不存在，该命令就会失败。 所有与 Bloom 筛选器相关的元数据都将从指定列中删除。

如果表中没有任何 Bloom 筛选器，则在清理表时将清除基础索引文件。