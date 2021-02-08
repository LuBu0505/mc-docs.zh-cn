---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 删除视图 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 DROP VIEW 语法。
ms.openlocfilehash: 53d26c168882313025ceab69379b9eadb4236ae5
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061178"
---
# <a name="drop-view"></a>删除视图

```sql
DROP VIEW [db_name.]view_name
```

删除基于一个或多个表的逻辑视图。

## <a name="examples"></a>示例

```sql
-- Drop the global temp view, temp view, and persistent view.
DROP VIEW global_temp.global_DeptSJC;
DROP VIEW temp_DeptSFO;
DROP VIEW database1.view_deptDetails;
```