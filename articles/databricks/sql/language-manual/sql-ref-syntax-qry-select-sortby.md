---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SORT BY 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 SORT BY 语法。
ms.openlocfilehash: 696ec1e3c616e174003762098cdafd19a57f8086
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692171"
---
# <a name="sort-by-clause"></a>SORT BY 子句

返回按用户指定顺序在每个分区内排序的结果行。 如果有多个分区，``SORT BY`` 可能返回部分有序的结果。 这与 [ORDER BY](sql-ref-syntax-qry-select-orderby.md) 子句不同，后者可保证输出的总序。

## <a name="syntax"></a>语法

```
SORT BY { expression [ sort_direction | nulls_sort_order ] [ , ... ] }
```

## <a name="parameters"></a>参数

* **SORT BY**

  用逗号分隔的表达式列表以及可选参数 ``sort_direction`` 和 ``nulls_sort_order``（用于对每个分区中的行进行排序）。

* **sort_direction**

  可选择指定是按升序还是降序对行进行排序。 排序方向的有效值为 ``ASC``（升序）和 ``DESC``（降序）。 如果未显式指定排序方向，则默认按升序对行排序。

  语法：``[ ASC | DESC ]``

* **nulls_sort_order**

  可选择指定是在非 NULL 值之前还是之后返回 NULL 值。 如果未指定 ``null_sort_order``，则在排序顺序为 ``ASC`` 时，Null 排在前面；排序顺序为 ``DESC`` 时，Null 排在后面。

  1. 如果指定了 ``NULLS FIRST``，则首先返回 NULL 值，而不考虑排序顺序。
  2. 如果指定了 ``NULLS LAST``，则最后返回 NULL 值，而不考虑排序顺序。

  语法：``[ NULLS { FIRST | LAST } ]``

## <a name="examples"></a>示例

```sql
CREATE TABLE person (zip_code INT, name STRING, age INT);
INSERT INTO person VALUES
    (94588, 'Zen Hui', 50),
    (94588, 'Dan Li', 18),
    (94588, 'Anil K', 27),
    (94588, 'John V', NULL),
    (94511, 'David K', 42),
    (94511, 'Aryan B.', 18),
    (94511, 'Lalit B.', NULL);

-- Use `REPARTITION` hint to partition the data by `zip_code` to
-- examine the `SORT BY` behavior. This is used in rest of the
-- examples.

-- Sort rows by `name` within each partition in ascending manner
SELECT /*+ REPARTITION(zip_code) */ name, age, zip_code FROM person SORT BY name;
+--------+----+--------+
|    name| age|zip_code|
+--------+----+--------+
|  Anil K|  27|   94588|
|  Dan Li|  18|   94588|
|  John V|null|   94588|
| Zen Hui|  50|   94588|
|Aryan B.|  18|   94511|
| David K|  42|   94511|
|Lalit B.|null|   94511|
+--------+----+--------+

-- Sort rows within each partition using column position.
SELECT /*+ REPARTITION(zip_code) */ name, age, zip_code FROM person SORT BY 1;
+--------+----+--------+
|    name| age|zip_code|
+--------+----+--------+
|  Anil K|  27|   94588|
|  Dan Li|  18|   94588|
|  John V|null|   94588|
| Zen Hui|  50|   94588|
|Aryan B.|  18|   94511|
| David K|  42|   94511|
|Lalit B.|null|   94511|
+--------+----+--------+

-- Sort rows within partition in ascending manner keeping null values to be last.
SELECT /*+ REPARTITION(zip_code) */ age, name, zip_code FROM person SORT BY age NULLS LAST;
+----+--------+--------+
| age|    name|zip_code|
+----+--------+--------+
|  18|  Dan Li|   94588|
|  27|  Anil K|   94588|
|  50| Zen Hui|   94588|
|null|  John V|   94588|
|  18|Aryan B.|   94511|
|  42| David K|   94511|
|null|Lalit B.|   94511|
+----+--------+--------+

-- Sort rows by age within each partition in descending manner, which defaults to NULL LAST.
SELECT /*+ REPARTITION(zip_code) */ age, name, zip_code FROM person SORT BY age DESC;
+----+--------+--------+
| age|    name|zip_code|
+----+--------+--------+
|  50| Zen Hui|   94588|
|  27|  Anil K|   94588|
|  18|  Dan Li|   94588|
|null|  John V|   94588|
|  42| David K|   94511|
|  18|Aryan B.|   94511|
|null|Lalit B.|   94511|
+----+--------+--------+

-- Sort rows by age within each partition in descending manner keeping null values to be first.
SELECT /*+ REPARTITION(zip_code) */ age, name, zip_code FROM person SORT BY age DESC NULLS FIRST;
+----+--------+--------+
| age|    name|zip_code|
+----+--------+--------+
|null|  John V|   94588|
|  50| Zen Hui|   94588|
|  27|  Anil K|   94588|
|  18|  Dan Li|   94588|
|null|Lalit B.|   94511|
|  42| David K|   94511|
|  18|Aryan B.|   94511|
+----+--------+--------+

-- Sort rows within each partition based on more than one column with each column having
-- different sort direction.
SELECT /*+ REPARTITION(zip_code) */ name, age, zip_code FROM person
    SORT BY name ASC, age DESC;
+--------+----+--------+
|    name| age|zip_code|
+--------+----+--------+
|  Anil K|  27|   94588|
|  Dan Li|  18|   94588|
|  John V|null|   94588|
| Zen Hui|  50|   94588|
|Aryan B.|  18|   94511|
| David K|  42|   94511|
|Lalit B.|null|   94511|
+--------+----+--------+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)