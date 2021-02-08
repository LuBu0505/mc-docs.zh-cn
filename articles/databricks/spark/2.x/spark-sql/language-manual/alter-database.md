---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 更改数据库 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 ALTER DATABASE 语法。
ms.openlocfilehash: cc14e9edd896410dd22d0fe9be37cc2f21aa336e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060947"
---
# <a name="alter-database"></a>更改数据库

```sql
ALTER [DATABASE|SCHEMA] db_name SET DBPROPERTIES (key=val, ...)
```

**``SET DBPROPERTIES``**

为数据库指定名为 ``key`` 的属性，并将该属性的值设置为 ``val``。 如果 ``key`` 已存在，则 ``val`` 将覆盖旧值。

## <a name="assign-owner"></a>分配所有者

```sql
ALTER DATABASE db_name OWNER TO `user_name@user_domain.com`
```

为数据库分配所有者。