---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 加载数据 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 LOAD DATA 语法。
ms.openlocfilehash: a7d4e6432ff82aa5f41fe2f7207e1be53599e796
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060734"
---
# <a name="load-data"></a>加载数据

```sql
LOAD DATA [LOCAL] INPATH path [OVERWRITE] INTO TABLE [db_name.]table_name [PARTITION part_spec]

part_spec:
    : (part_col_name1=val1, part_col_name2=val2, ...)
```

将数据从文件加载到表或表中的分区中。 目标表不能是临时表。 当且仅当目标表已分区时，才必须提供分区规范。

> [!NOTE]
>
> 仅使用 Hive 格式创建的表支持此功能。

**``LOCAL``**

从本地文件系统加载路径。 否则，将使用默认文件系统。

**``OVERWRITE``**

删除表中的现有数据。 否则，新数据将追加到该表中。