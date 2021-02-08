---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DISTRIBUTE BY 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 DISTRIBUTE BY 语法。
ms.openlocfilehash: bd28e23e8a9b11089b35e1fee3b35446e969ad3e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060906"
---
# <a name="distribute-by-clause"></a>DISTRIBUTE BY 子句

根据输入表达式对数据进行重新分区。 与 [CLUSTER BY](sql-ref-syntax-qry-select-clusterby.md) 子句不同，不会对每个分区中的数据进行排序。

## <a name="syntax"></a>语法

```
DISTRIBUTE BY { expression [ , ... ] }
```

## <a name="parameters"></a>参数

* **expression**

  生成值的一个或多个值、运算符和 SQL 函数的组合。

## <a name="examples"></a>示例

```sql
CREATE TABLE person (name STRING, age INT);
INSERT INTO person VALUES
    ('Zen Hui', 25),
    ('Anil B', 18),
    ('Shone S', 16),
    ('Mike A', 25),
    ('John A', 18),
    ('Jack N', 16);

-- Reduce the number of shuffle partitions to 2 to illustrate the behavior of `DISTRIBUTE BY`.
-- It's easier to see the clustering and sorting behavior with less number of partitions.
SET spark.sql.shuffle.partitions = 2;

-- Select the rows with no ordering. Please note that without any sort directive, the result
-- of the query is not deterministic. It's included here to just contrast it with the
-- behavior of `DISTRIBUTE BY`. The query below produces rows where age columns are not
-- clustered together.
SELECT age, name FROM person;
+---+-------+
|age|   name|
+---+-------+
| 16|Shone S|
| 25|Zen Hui|
| 16| Jack N|
| 25| Mike A|
| 18| John A|
| 18| Anil B|
+---+-------+

-- Produces rows clustered by age. Persons with same age are clustered together.
-- Unlike `CLUSTER BY` clause, the rows are not sorted within a partition.
SELECT age, name FROM person DISTRIBUTE BY age;
+---+-------+
|age|   name|
+---+-------+
| 25|Zen Hui|
| 25| Mike A|
| 18| John A|
| 18| Anil B|
| 16|Shone S|
| 16| Jack N|
+---+-------+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)
* [WHERE 子句](sql-ref-syntax-qry-select-where.md)
* [GROUP BY 子句](sql-ref-syntax-qry-select-groupby.md)
* [HAVING 子句](sql-ref-syntax-qry-select-having.md)
* [ORDER BY 子句](sql-ref-syntax-qry-select-orderby.md)
* [SORT BY 子句](sql-ref-syntax-qry-select-sortby.md)
* [CLUSTER BY 子句](sql-ref-syntax-qry-select-clusterby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)