---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 清除缓存 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 CLEAR CACHE 语法。
ms.openlocfilehash: 4b2c345ad77ba2920a44b3574b65e82c804a3737
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060745"
---
# <a name="clear-cache"></a>清除缓存

```sql
CLEAR CACHE
```

清除与 SQLContext 关联的 RDD 缓存。