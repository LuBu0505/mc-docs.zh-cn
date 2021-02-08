---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: Describe Database - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 DESCRIBE DATABASE 语法。
ms.openlocfilehash: 486aca8cb14f256d96eb50f98aee57fff1efea36
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061080"
---
# <a name="describe-database"></a>描述数据库

```sql
DESCRIBE DATABASE [EXTENDED] db_name
```

返回现有数据库的元数据（名称、注释和位置）。 如果该数据库不存在，则会引发异常。

**``EXTENDED``**

显示数据库属性。