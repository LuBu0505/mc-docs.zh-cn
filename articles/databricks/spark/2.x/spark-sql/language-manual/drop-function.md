---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 删除函数 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 DROP FUNCTION 语法。
ms.openlocfilehash: 83ba7e3e31f689bece8f2839ba2b0174a93deda4
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061180"
---
# <a name="drop-function"></a>删除函数

```sql
DROP [TEMPORARY] FUNCTION [IF EXISTS] [db_name.]function_name
```

删除现有函数。 如果要删除的函数不存在，则会引发异常。

> [!NOTE]
>
> 仅当启用了 Hive 支持时，才支持此命令。

**``TEMPORARY``**

要删除的函数是否是临时函数。

**``IF EXISTS``**

如果要删除的函数不存在，则什么都不会发生。