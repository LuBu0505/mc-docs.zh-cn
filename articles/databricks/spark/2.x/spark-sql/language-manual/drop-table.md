---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 删除表 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 DROP TABLE 语法。
ms.openlocfilehash: 0be43494db9804b1c3d3b9f5565298876e539c7b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061179"
---
# <a name="drop-table"></a>删除表

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
```

如果表不是 ``EXTERNAL`` 表，请删除该表并从文件系统中删除与该表关联的目录。 如果要删除的表不存在，则会引发异常。

**``IF EXISTS``**

如果该表不存在，则不会执行任何操作。