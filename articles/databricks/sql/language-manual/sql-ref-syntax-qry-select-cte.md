---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 公用表表达式 (CTE) - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的公用表表达式。
ms.openlocfilehash: 5d33e0189fb1fcef180878da442a026a6a065640
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692184"
---
# <a name="common-table-expression-cte"></a>公用表表达式 (CTE)

定义一个可以在 SQL 语句的范围内多次引用的临时结果集。 CTE 主要在 ``SELECT`` 语句中使用。

## <a name="syntax"></a>语法

```sql
WITH common_table_expression [ , ... ]
```

而 ``common_table_expression`` 被定义为

```sql
expression_name [ ( column_name [ , ... ] ) ] [ AS ] ( query )
```

## <a name="parameters"></a>参数

* **expression_name**

  公用表表达式的名称。

* **查询**

  [SELECT 语句](sql-ref-syntax-qry-select.md)。

## <a name="examples"></a>示例

```sql
-- CTE with multiple column aliases
WITH t(x, y) AS (SELECT 1, 2)
SELECT * FROM t WHERE x = 1 AND y = 2;
+---+---+
|  x|  y|
+---+---+
|  1|  2|
+---+---+

-- CTE in CTE definition
WITH t AS (
    WITH t2 AS (SELECT 1)
    SELECT * FROM t2
)
SELECT * FROM t;
+---+
|  1|
+---+
|  1|
+---+

-- CTE in subquery
SELECT max(c) FROM (
    WITH t(c) AS (SELECT 1)
    SELECT * FROM t
);
+------+
|max(c)|
+------+
|     1|
+------+

-- CTE in subquery expression
SELECT (
    WITH t AS (SELECT 1)
    SELECT * FROM t
);
+----------------+
|scalarsubquery()|
+----------------+
|               1|
+----------------+

-- CTE in CREATE VIEW statement
CREATE VIEW v AS
    WITH t(a, b, c, d) AS (SELECT 1, 2, 3, 4)
    SELECT * FROM t;
SELECT * FROM v;
+---+---+---+---+
|  a|  b|  c|  d|
+---+---+---+---+
|  1|  2|  3|  4|
+---+---+---+---+

-- If name conflict is detected in nested CTE, then AnalysisException is thrown by default.
-- SET spark.sql.legacy.ctePrecedencePolicy = CORRECTED (which is recommended),
-- inner CTE definitions take precedence over outer definitions.
SET spark.sql.legacy.ctePrecedencePolicy = CORRECTED;
WITH
    t AS (SELECT 1),
    t2 AS (
        WITH t AS (SELECT 2)
        SELECT * FROM t
    )
SELECT * FROM t2;
+---+
|  2|
+---+
|  2|
+---+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)