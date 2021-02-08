---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: Set - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SET 属性语法。
ms.openlocfilehash: dabb7ed066abb78b23974e51e10a07c98ac70777
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060938"
---
# <a name="set"></a>设置

```sql
SET [-v]
SET property_key[=property_value]
```

设置属性、返回现有属性的值或列出所有现有属性。 如果为现有属性键提供值，则将替代旧值。

**``-v``**

输出现有属性的含义。

**``property_key``**

设置或返回单个属性的值。