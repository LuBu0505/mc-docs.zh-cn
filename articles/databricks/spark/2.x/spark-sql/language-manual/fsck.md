---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/08/2020
title: Fsck Repair Table（Azure Databricks 上的 Delta Lake） - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 FSCK REPAIR TABLE 语法。
ms.openlocfilehash: 7312478817687fe607af815536d36e3ac45b0ba7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061114"
---
# <a name="fsck-repair-table--delta-lake-on-azure-databricks"></a>对表进行 Fsck 修复（Azure Databricks 上的 Delta Lake）

```sql
FSCK REPAIR TABLE [db_name.]table_name [DRY RUN]
```

从 Delta 表的事务日志中删除无法再从基础文件系统中找到的文件条目。 手动删除这些文件时，可能会发生这种情况。

**``DRY RUN``**

返回要从事务日志中删除的文件的列表。