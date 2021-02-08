---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 删除数据库 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 DROP DATABASE 和 DROP SCHEMA 语法。
ms.openlocfilehash: 4a2eef02fafd8eb71b0231d52921b998d9b0778a
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061181"
---
# <a name="drop-database"></a>删除数据库

```sql
DROP [DATABASE | SCHEMA] [IF EXISTS] db_name [RESTRICT | CASCADE]
```

删除数据库并从文件系统中删除与该数据库关联的目录。 如果该数据库不存在，则会引发异常。

**``IF EXISTS``**

如果要删除的数据库不存在，则什么都不会发生。

**``RESTRICT``**

删除非空数据库会触发异常。 默认情况下启用。

**``CASCADE``**

删除非空数据库也会导致所有关联的表和函数被删除。