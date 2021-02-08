---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 使用数据库 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 USE (database) 语法。
ms.openlocfilehash: b6032f7e8c4ee901af7da179424b5a0b834bfaca
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060668"
---
# <a name="use-database"></a>使用数据库

```sql
USE db_name
```

设置当前数据库。 未显式指定数据库的所有后续命令将使用此数据库。 如果提供的数据库不存在，则会引发异常。 默认的当前数据库是 ``default``。