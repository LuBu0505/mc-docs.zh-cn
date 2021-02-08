---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 刷新表 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 REFRESH TABLE 语法。
ms.openlocfilehash: 5374212648b3e69618480d2b2ce74deb58ad50a2
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060729"
---
# <a name="refresh-table"></a>刷新表

```sql
REFRESH TABLE [db_name.]table_name
```

刷新与该表关联的所有缓存条目。 如果以前缓存过该表，则会在下次扫描时延迟缓存该表。