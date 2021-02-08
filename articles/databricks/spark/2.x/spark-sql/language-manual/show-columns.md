---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: Show Columns - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SHOW COLUMNS 语法。
ms.openlocfilehash: 86b20e70f3470178b02b3acd1e7454f5741b8eb4
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060937"
---
# <a name="show-columns"></a>显示列

```sql
SHOW COLUMNS (FROM | IN) [db_name.]table_name
```

返回表中的列的列表。 如果该表不存在，则会引发异常。