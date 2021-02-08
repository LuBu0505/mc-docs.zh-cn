---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: Reset - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 RESET 语法。
ms.openlocfilehash: 9bc99f5b4be304f85f9d707d31fabc44c389c031
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060879"
---
# <a name="reset"></a>重置

```sql
RESET
```

将所有属性重置为其默认值。 此后，[Set](set.md) 命令的输出将为空。