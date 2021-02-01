---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 显示授权 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW GRANT 语法。
ms.openlocfilehash: 39b4927c8f4b27513568fa66cdcbba9228b289b8
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060833"
---
# <a name="show-grant"></a>显示授权

```sql
SHOW GRANT [user] ON [CATALOG | DATABASE <database-name> | TABLE <table-name> | VIEW <view-name> | FUNCTION <function-name> | ANONYMOUS FUNCTION | ANY FILE]
```

显示会影响指定对象的所有权限（包括已继承、已拒绝和已授予的）。

**示例**

```sql
SHOW GRANT `<user>@<domain-name>` ON DATABASE <database-name>
```