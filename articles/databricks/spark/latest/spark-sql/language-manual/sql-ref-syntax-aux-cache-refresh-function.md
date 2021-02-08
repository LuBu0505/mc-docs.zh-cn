---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: REFRESH FUNCTION - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 REFRESH FUNCTION 语法。
ms.openlocfilehash: d1a8a89d5986dd669779130e2411752f89f9d317
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060641"
---
# <a name="refresh-function"></a>REFRESH FUNCTION

使缓存的函数项（包括给定函数的类名和资源位置）无效。 立即填充无效缓存。
请注意，``REFRESH FUNCTION`` 仅适用于永久函数。 刷新本机函数或临时函数将导致异常。

## <a name="syntax"></a>语法

```sql
REFRESH FUNCTION function_identifier
```

## <a name="parameters"></a>参数

* **function_identifier**

  函数名称（限定名称或非限定名称）。 如果未提供数据库标识符，则使用当前数据库。

  **语法：** ``[database_name.] function_name``

## <a name="examples"></a>示例

```sql
-- The cached entry of the function is refreshed
-- The function is resolved from the current database as the function name is unqualified.
REFRESH FUNCTION func1;

-- The cached entry of the function is refreshed
-- The function is resolved from tempDB database as the function name is qualified.
REFRESH FUNCTION db1.func1;
```

## <a name="related-statements"></a>相关语句

* [CACHE TABLE](sql-ref-syntax-aux-cache-cache-table.md)
* [CLEAR CACHE](sql-ref-syntax-aux-cache-clear-cache.md)
* [UNCACHE TABLE](sql-ref-syntax-aux-cache-uncache-table.md)
* [REFRESH TABLE](sql-ref-syntax-aux-cache-refresh-table.md)
* [REFRESH](sql-ref-syntax-aux-cache-refresh.md)