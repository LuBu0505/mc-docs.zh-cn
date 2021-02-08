---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示函数 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SHOW FUNCTIONS 语法。
ms.openlocfilehash: b82b1c5434165345957210bf3b2a8126c93ffed7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060935"
---
# <a name="show-functions"></a>显示函数

```sql
SHOW [USER | SYSTEM |ALL ] FUNCTIONS ([LIKE] regex | [db_name.]function_name)
```

显示与给定 regex 或函数名称匹配的函数。 如果未提供 regex 或名称，则显示所有函数。 如果声明 ``USER`` 或 ``SYSTEM``，则这些函数将只分别显示用户定义的 Spark SQL 函数和系统定义的 Spark SQL 函数。

**``LIKE``**

此限定符仅用于兼容性，不起作用。