---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 采样查询 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 TABLESAMPLE 语法。
ms.openlocfilehash: dcd14e1914e781ff18f4e8802258d3a8c2a62935
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060895"
---
# <a name="sampling-queries"></a>采样查询

``TABLESAMPLE`` 语句用于对表进行采样。 它支持以下采样方法：

* ``TABLESAMPLE``(x ``ROWS``)：对表采样给定行数。
* ``TABLESAMPLE``(x ``PERCENT``)：对表采样给定百分比。 请注意，百分比定义为介于 0 到 100 之间的数字。
* ``TABLESAMPLE``(``BUCKET`` x ``OUT OF`` y)：对表采样 ``x``/``y`` 部分。

> [!NOTE]
>
> ``TABLESAMPLE`` 返回所请求的大约行数或分数。

## <a name="syntax"></a>语法

```
TABLESAMPLE ({ integer_expression | decimal_expression } PERCENT)
    | TABLESAMPLE ( integer_expression ROWS )
    | TABLESAMPLE ( BUCKET integer_expression OUT OF integer_expression )
```

## <a name="examples"></a>示例

```sql
SELECT * FROM test;
+--+----+
|id|name|
+--+----+
| 5|Alex|
| 8|Lucy|
| 2|Mary|
| 4|Fred|
| 1|Lisa|
| 9|Eric|
|10|Adam|
| 6|Mark|
| 7|Lily|
| 3|Evan|
+--+----+

SELECT * FROM test TABLESAMPLE (50 PERCENT);
+--+----+
|id|name|
+--+----+
| 5|Alex|
| 2|Mary|
| 4|Fred|
| 9|Eric|
|10|Adam|
| 3|Evan|
+--+----+

SELECT * FROM test TABLESAMPLE (5 ROWS);
+--+----+
|id|name|
+--+----+
| 5|Alex|
| 8|Lucy|
| 2|Mary|
| 4|Fred|
| 1|Lisa|
+--+----+

SELECT * FROM test TABLESAMPLE (BUCKET 4 OUT OF 10);
+--+----+
|id|name|
+--+----+
| 8|Lucy|
| 2|Mary|
| 9|Eric|
| 6|Mark|
+--+----+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)