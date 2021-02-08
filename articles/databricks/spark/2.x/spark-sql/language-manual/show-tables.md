---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示表 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 SHOW TABLES 语法。
ms.openlocfilehash: 4b611ed16bcc48ee7049dd3d7d32fe0dc8484686
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060830"
---
# <a name="show-tables"></a>显示表

```sql
SHOW TABLES [FROM | IN] db_name [LIKE 'pattern']
```

返回所有表。 显示表的数据库以及表是否是临时表。

**``FROM | IN``**

返回数据库中的所有表。

**``LIKE 'pattern'``**

指示要匹配的表名称。 在 ``pattern`` 中，``*`` 匹配任意数量的字符。