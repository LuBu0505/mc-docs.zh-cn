---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/08/2020
title: 描述历史记录（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 DESCRIBE TABLE 语法。
ms.openlocfilehash: ae46e8c841c6324a17c099403e6584de91ffcc97
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060848"
---
# <a name="describe-history-delta-lake-on-azure-databricks"></a><a id="describe-history"> </a><a id="describe-history-delta-lake-on-azure-databricks"> </a>描述历史记录（Azure Databricks 上的 Delta Lake）

```sql
DESCRIBE HISTORY [db_name.]table_name

DESCRIBE HISTORY delta.`<path-to-table>`
```

返回对表的每次写入的出处信息，包括操作、用户等。  表历史记录会保留 30 天。

有关详细信息，请参阅[检索 Delta 表历史记录](../../../../delta/delta-utility.md#delta-history)。