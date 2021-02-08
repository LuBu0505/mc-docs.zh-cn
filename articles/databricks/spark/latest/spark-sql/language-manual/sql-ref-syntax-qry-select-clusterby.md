---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CLUSTER BY 子句 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 CLUSTER BY 语法。
ms.openlocfilehash: 57505ec35e20283addbf9c6e20bd0bf08e992b1f
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061123"
---
# <a name="cluster-by-clause"></a>CLUSTER BY 子句

根据输入表达式对数据进行重新分区，然后对每个分区中的数据进行排序。 这在语义上等效于执行 [DISTRIBUTE BY](sql-ref-syntax-qry-select-distributeby.md)，后跟 [SORT BY](sql-ref-syntax-qry-select-sortby.md)。 此子句仅确保在每个分区内对生成行进行排序，并且不保证输出的总顺序。

## <a name="syntax"></a>语法

```
CLUSTER BY { expression [ , ... ] }
```

## <a name="parameters"></a>参数

* **expression**

  指定生成值的一个或多个值、运算符和 SQL 函数的组合。

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

-- Reduce the number of shuffle partitions to 2 to illustrate the behavior of `CLUSTER BY`.
-- It's easier to see the clustering and sorting behavior with less number of partitions.
SET spark.sql.shuffle.partitions = 2;

-- Select the rows with no ordering. Please note that without any sort directive, the results
-- of the query is not deterministic. It's included here to show the difference in behavior
-- of a query when `CLUSTER BY` is not used vs when it's used. The query below produces rows
-- where age column is not sorted.
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
-- In the query below, persons with age 18 and 25 are in first partition and the
-- persons with age 16 are in the second partition. The rows are sorted based
-- on age within each partition.
SELECT age, name FROM person CLUSTER BY age;
+---+-------+
|age|   name|
+---+-------+
| 18| John A|
| 18| Anil B|
| 25|Zen Hui|
| 25| Mike A|
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
* [DISTRIBUTE BY 子句](sql-ref-syntax-qry-select-distributeby.md)
* [LIMIT 子句](sql-ref-syntax-qry-select-limit.md)
* [CASE 子句](sql-ref-syntax-qry-select-case.md)
* [PIVOT 子句](sql-ref-syntax-qry-select-pivot.md)
* [LATERAL VIEW 子句](sql-ref-syntax-qry-select-lateral-view.md)