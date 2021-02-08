---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示表属性 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SHOW TBLPROPERTIES 语法。
ms.openlocfilehash: f16a4d2dac8364ace980f7f12ca84abada7d485e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060829"
---
# <a name="show-table-properties"></a>显示表属性

```sql
SHOW TBLPROPERTIES [db_name.]table_name [property_key]
```

返回表中的所有属性或特定属性集的值。 如果该表不存在，会引发异常。