---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示数据库 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark SQL 2.x 语言的 SHOW DATABASES 和 SHOW SCHEMAS 语法。
ms.openlocfilehash: 9ec359380154b1c99102835f8f4df7769d7211ec
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060934"
---
# <a name="show-databases"></a>显示数据库

```sql
SHOW [DATABASES | SCHEMAS] [LIKE 'pattern']
```

返回所有数据库。 ``SHOW SCHEMAS`` 是 ``SHOW DATABASES``的同义词。

**``LIKE 'pattern'``**

要匹配的数据库名称。 在 ``pattern`` 中，``*`` 匹配任意数量的字符。