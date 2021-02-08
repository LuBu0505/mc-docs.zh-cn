---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 描述函数 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 DESCRIBE FUNCTION 语法。
ms.openlocfilehash: ac3fbd964d4660278c9c8507d73e74dbbf661281
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061079"
---
# <a name="describe-function"></a>描述函数

```sql
DESCRIBE FUNCTION [EXTENDED] [db_name.]function_name
```

返回现有函数（实现类和用法）的元数据。 如果该函数不存在，则会引发异常。

**``EXTENDED``**

显示扩展的用法信息。