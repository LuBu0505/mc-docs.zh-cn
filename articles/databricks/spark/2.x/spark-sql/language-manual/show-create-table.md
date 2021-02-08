---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示用于创建表的命令 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SHOW CREATE TABLE 语法。
ms.openlocfilehash: 6771f6f351fda521c93fb05ba7ca127708b2d2c0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060936"
---
# <a name="show-create-table"></a>显示用于创建表的命令

```sql
SHOW CREATE TABLE [db_name.]table_name
```

返回用于创建现有表的命令。 如果该表不存在，则会引发异常。