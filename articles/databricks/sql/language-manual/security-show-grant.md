---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: SHOW GRANT - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW GRANT 语法。
ms.openlocfilehash: dca0471fa2378326677f75a13bca5fac9acbcef9
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692137"
---
# <a name="show-grant"></a>SHOW GRANT

显示会影响指定对象的所有权限（包括已继承、已拒绝和已授予的权限）。

## <a name="syntax"></a>语法

```sql
SHOW GRANT [user] ON [CATALOG | DATABASE <database-name> | TABLE <table-name> | VIEW <view-name> | FUNCTION <function-name> | ANONYMOUS FUNCTION | ANY FILE]
```

## <a name="example"></a>示例

```sql
SHOW GRANT `<user>@<domain-name>` ON DATABASE <database-name>
```