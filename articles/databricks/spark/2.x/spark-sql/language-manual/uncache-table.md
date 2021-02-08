---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: Uncache Table - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 UNCACHE TABLE 语法。
ms.openlocfilehash: b55ccf73efad10fb354588e069379b86cedc7f8c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060724"
---
# <a name="uncache-table"></a>取消缓存表

```sql
UNCACHE TABLE [db_name.]table_name
```

从 RDD 缓存中删除与该表关联的所有缓存条目。