---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示分区 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SHOW PARTITIONS 语法。
ms.openlocfilehash: a00f69b42b93549299e409b270c3eb3411f9c92b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060834"
---
# <a name="show-partitions"></a>显示分区

```sql
SHOW PARTITIONS [db_name.]table_name [PARTITION part_spec]

part_spec:
  : (part_col_name1=val1, part_col_name2=val2, ...)
```

列出表的分区，按给定的分区值进行筛选。 启用 Hive 支持后，只有使用 Delta Lake 格式或 Hive 格式创建的表才支持列出分区。