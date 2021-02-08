---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: NULL 语义 - Azure Databricks
description: 了解 Azure Databricks 中支持的 SQL 语言构造中的 SQL NULL 语义。
ms.openlocfilehash: 5b4b7c1ee8e97b50f45f5bbf872d6905a6dbe9dc
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060647"
---
# <a name="null-semantics"></a>NULL 语义

表包含一组行，每行包含一组列。
列与数据类型相关联，表示实体的特定属性（例如，``age`` 是名为 ``person`` 的实体的列）。 有时，特定于某行的列的值在该行存在时是未知的。
在 ``SQL`` 中，此类值表示为 ``NULL``。 本部分详细介绍了 ``NULL`` 值在各种运算符、表达式和其他 ``SQL`` 构造中的语义。

以下内容说明了名为 ``person`` 的表的架构布局和数据。 该数据包含 ``age`` 列中的 ``NULL`` 值，下面各部分中的各种示例中都使用了该表。

```
|Id |Name|Age|
|---|----|---|
|100|Joe|30|
|200|Marry|NULL|
|300|Mike|18|
|400|Fred|50|
|500|Albert|NULL|
|600|Michelle|30|
|700|Dan|50|
```

## <a name="comparison-operators"></a>比较运算符

Apache Spark 支持标准比较运算符，例如 ``>``、``>=``、``=``、``<`` 和 ``<=``。
当其中一个操作数或两个操作数未知或为 ``NULL`` 时，这些运算符的结果未知或为 ``NULL``。 为了比较 ``NULL`` 值以确定结果是否相等，Spark 提供了对 null 安全的“等于”运算符 (``<=>``)，当其中一个操作数为 ``NULL`` 时返回 ``False``，当两个操作数均为 ``NULL`` 时返回 ``True``。 下表说明了当其中一个或两个操作数为 ``NULL`` 时比较运算符的行为：

| 左操作数 | 右操作数 |      | <br><br>= | =    | <    | <=   | <=>   |
|--------------|----------------|------|-----------|------|------|------|-------|
| Null         | 任何值      | Null | Null      | Null | Null | Null | False |
| 任何值    | Null           | Null | Null      | Null | Null | Null | False |
| Null         | Null           | Null | Null      | Null | Null | Null | True  |

### <a name="examples"></a>示例

```sql
-- Normal comparison operators return `NULL` when one of the operand is `NULL`.
SELECT 5 > null AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+

-- Normal comparison operators return `NULL` when both the operands are `NULL`.
SELECT null = null AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+

-- Null-safe equal operator return `False` when one of the operand is `NULL`
SELECT 5 <=> null AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|            false|
+-----------------+

-- Null-safe equal operator return `True` when one of the operand is `NULL`
SELECT NULL <=> NULL;
+-----------------+
|expression_output|
+-----------------+
|             true|
+-----------------+
```

## <a name="logical-operators"></a>逻辑运算符

Spark 支持标准逻辑运算符，例如 ``AND``、``OR`` 和 ``NOT``。 这些运算符采用 ``Boolean`` 表达式作为参数，并返回 ``Boolean`` 值。

下表说明了当其中一个或两个操作数为 ``NULL`` 时逻辑运算符的行为。

| 左操作数 | 右操作数 | 或   | AND   |
|--------------|---------------|------|-------|
| 正确         | Null          | True | Null  |
| False        | Null          | Null | False |
| Null         | True          | True | Null  |
| Null         | False         | Null | False |
| Null         | Null          | Null | Null  |

| 操作数 | NOT  |
|---------|------|
| Null    | Null |

### <a name="examples"></a>示例

```sql
-- Normal comparison operators return `NULL` when one of the operands is `NULL`.
SELECT (true OR null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             true|
+-----------------+

-- Normal comparison operators return `NULL` when both the operands are `NULL`.
SELECT (null OR false) AS expression_output
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+

-- Null-safe equal operator returns `False` when one of the operands is `NULL`
SELECT NOT(null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+
```

## <a name="expressions"></a>表达式

在 Spark 中，比较运算符和逻辑运算符被看作是表达式。 除了这两种表达式，Spark 还支持其他形式的表达式，例如函数表达式、强制转换表达式等。Spark 中的表达式大致可分为：

* 不接受 Null 的表达式
* 可处理 ``NULL`` 值操作数的表达式
  * 这些表达式的结果取决于表达式本身。

### <a name="null-intolerant-expressions"></a>不接受 Null 的表达式

当表达式的一个或多个参数为 ``NULL``，且大多数表达式都属于此类别时，不接受 Null 的表达式将返回 ``NULL``。

#### <a name="examples"></a>示例

```sql
SELECT concat('John', null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+

SELECT positive(null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+

SELECT to_date(null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+
```

### <a name="expressions-that-can-process-null-value-operands"></a>可处理 Null 值操作数的表达式

此类表达式旨在处理 ``NULL`` 值。 这些表达式的结果取决于表达式本身。 例如，函数表达式 ``isnull`` 对 Null 输入返回 ``true``，对非 Null 输入返回 ``false``，而函数 ``coalesce`` 则返回其操作数列表中的第一个非 ``NULL`` 值。 不过，``coalesce`` 在其所有操作数均为 ``NULL`` 时会返回 ``NULL``。 以下是此类别表达式的部分列表。

* COALESCE
* NULLIF
* IFNULL
* NVL
* NVL2
* ISNAN
* NANVL
* ISNULL
* ISNOTNULL
* ATLEASTNNONNULLS
* IN

#### <a name="examples"></a>示例

```sql
SELECT isnull(null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             true|
+-----------------+

-- Returns the first occurrence of non `NULL` value.
SELECT coalesce(null, null, 3, null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|                3|
+-----------------+

-- Returns `NULL` as all its operands are `NULL`.
SELECT coalesce(null, null, null, null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|             null|
+-----------------+

SELECT isnan(null) AS expression_output;
+-----------------+
|expression_output|
+-----------------+
|            false|
+-----------------+
```

### <a name="built-in-aggregate-expressions"></a>内置聚合表达式

聚合函数通过处理一组输入行来计算单个结果。 以下规则规定聚合函数如何处理 ``NULL`` 值。

* 所有聚合函数均不处理 ``NULL`` 值。
  * 唯一的例外是 COUNT(*) 函数。
* 所有输入值均为 ``NULL`` 或输入数据集为空时，某些聚合函数将返回 ``NULL``。  这些函数的列表如下：
  * ``MAX``
  * ``MIN``
  * ``SUM``
  * ``AVG``
  * ``EVERY``
  * ``ANY``
  * ``SOME``

### <a name="examples"></a>示例

```sql
-- `count(*)` does not skip `NULL` values.
SELECT count(*) FROM person;
+--------+
|count(1)|
+--------+
|       7|
+--------+

-- `NULL` values in column `age` are skipped from processing.
SELECT count(age) FROM person;
+----------+
|count(age)|
+----------+
|         5|
+----------+

-- `count(*)` on an empty input set returns 0. This is unlike the other
-- aggregate functions, such as `max`, which return `NULL`.
SELECT count(*) FROM person where 1 = 0;
+--------+
|count(1)|
+--------+
|       0|
+--------+

-- `NULL` values are excluded from computation of maximum value.
SELECT max(age) FROM person;
+--------+
|max(age)|
+--------+
|      50|
+--------+

-- `max` returns `NULL` on an empty input set.
SELECT max(age) FROM person where 1 = 0;
+--------+
|max(age)|
+--------+
|    null|
+--------+
```

## <a name="condition-expressions-in-where-having-and-join-clauses"></a>``WHERE``、``HAVING`` 和 ``JOIN`` 子句中的条件表达式

``WHERE`` 和 ``HAVING`` 运算符根据用户指定的条件对行进行筛选。
``JOIN`` 运算符用于根据联接条件合并两个表中的行。
对于这三个运算符来说，条件表达式是布尔表达式，可返回 ``True``、``False`` 或 ``Unknown (NULL)``。 如果条件的结果为 ``True``，则它们“符合标准”。

### <a name="examples"></a>示例

```sql
-- Persons whose age is unknown (`NULL`) are filtered out from the result set.
SELECT * FROM person WHERE age > 0;
+--------+---+
|    name|age|
+--------+---+
|Michelle| 30|
|    Fred| 50|
|    Mike| 18|
|     Dan| 50|
|     Joe| 30|
+--------+---+

-- `IS NULL` expression is used in disjunction to select the persons
-- with unknown (`NULL`) records.
SELECT * FROM person WHERE age > 0 OR age IS NULL;
+--------+----+
|    name| age|
+--------+----+
|  Albert|null|
|Michelle|  30|
|    Fred|  50|
|    Mike|  18|
|     Dan|  50|
|   Marry|null|
|     Joe|  30|
+--------+----+

-- Person with unknown(`NULL`) ages are skipped from processing.
SELECT * FROM person GROUP BY age HAVING max(age) > 18;
+---+--------+
|age|count(1)|
+---+--------+
| 50|       2|
| 30|       2|
+---+--------+

-- A self join case with a join condition `p1.age = p2.age AND p1.name = p2.name`.
-- The persons with unknown age (`NULL`) are filtered out by the join operator.
SELECT * FROM person p1, person p2
    WHERE p1.age = p2.age
    AND p1.name = p2.name;
+--------+---+--------+---+
|    name|age|    name|age|
+--------+---+--------+---+
|Michelle| 30|Michelle| 30|
|    Fred| 50|    Fred| 50|
|    Mike| 18|    Mike| 18|
|     Dan| 50|     Dan| 50|
|     Joe| 30|     Joe| 30|
+--------+---+--------+---+

-- The age column from both legs of join are compared using null-safe equal which
-- is why the persons with unknown age (`NULL`) are qualified by the join.
SELECT * FROM person p1, person p2
    WHERE p1.age <=> p2.age
    AND p1.name = p2.name;
+--------+----+--------+----+
|    name| age|    name| age|
+--------+----+--------+----+
|  Albert|null|  Albert|null|
|Michelle|  30|Michelle|  30|
|    Fred|  50|    Fred|  50|
|    Mike|  18|    Mike|  18|
|     Dan|  50|     Dan|  50|
|   Marry|null|   Marry|null|
|     Joe|  30|     Joe|  30|
+--------+----+--------+----+
```

## <a name="aggregate-operators-group-by-distinct"></a>聚合运算符（``GROUP BY``、``DISTINCT``）

如[比较运算符](#comparison-operators)中所述，两个 ``NULL`` 值不相等。 但为了分组和进行不同的处理，具有 ``NULL data`` 的两个或多个值会分组到同一 Bucket。 此行为符合 SQL 标准，适用于其他企业数据库管理系统。

### <a name="examples"></a>示例

```sql
-- `NULL` values are put in one bucket in `GROUP BY` processing.
SELECT age, count(*) FROM person GROUP BY age;
+----+--------+
| age|count(1)|
+----+--------+
|null|       2|
|  50|       2|
|  30|       2|
|  18|       1|
+----+--------+

-- All `NULL` ages are considered one distinct value in `DISTINCT` processing.
SELECT DISTINCT age FROM person;
+----+
| age|
+----+
|null|
|  50|
|  30|
|  18|
+----+
```

## <a name="sort-operator-order-by-clause"></a>Sort 运算符（``ORDER BY`` 子句）

Spark SQL 在 ``ORDER BY`` 子句中支持 Null 排序规范。 Spark 根据 Null 排序规范将所有 ``NULL`` 值置于开头或末尾，以处理 ``ORDER BY`` 子句。 默认情况下，所有 ``NULL`` 值都放在开头。

### <a name="examples"></a>示例

```sql
-- `NULL` values are shown at first and other values
-- are sorted in ascending way.
SELECT age, name FROM person ORDER BY age;
+----+--------+
| age|    name|
+----+--------+
|null|   Marry|
|null|  Albert|
|  18|    Mike|
|  30|Michelle|
|  30|     Joe|
|  50|    Fred|
|  50|     Dan|
+----+--------+

-- Column values other than `NULL` are sorted in ascending
-- way and `NULL` values are shown at the last.
SELECT age, name FROM person ORDER BY age NULLS LAST;
+----+--------+
| age|    name|
+----+--------+
|  18|    Mike|
|  30|Michelle|
|  30|     Joe|
|  50|     Dan|
|  50|    Fred|
|null|   Marry|
|null|  Albert|
+----+--------+

-- Columns other than `NULL` values are sorted in descending
-- and `NULL` values are shown at the last.
SELECT age, name FROM person ORDER BY age DESC NULLS LAST;
+----+--------+
| age|    name|
+----+--------+
|  50|    Fred|
|  50|     Dan|
|  30|Michelle|
|  30|     Joe|
|  18|    Mike|
|null|   Marry|
|null|  Albert|
+----+--------+
```

## <a name="set-operators-union-intersect-except"></a>Set 运算符（``UNION``、``INTERSECT``、``EXCEPT``）

在 set 操作的上下文中，以 Null-safe 方式比较 ``NULL`` 值是否相等。 这意味着在比较行时，两个 ``NULL`` 值被视为相等，这一点与常规的 ``EqualTo`` (``=``) 运算符不同。

### <a name="examples"></a>示例

```sql
CREATE VIEW unknown_age SELECT * FROM person WHERE age IS NULL;

-- Only common rows between two legs of `INTERSECT` are in the
-- result set. The comparison between columns of the row are done
-- in a null-safe manner.
SELECT name, age FROM person
    INTERSECT
    SELECT name, age from unknown_age;
+------+----+
|  name| age|
+------+----+
|Albert|null|
| Marry|null|
+------+----+

-- `NULL` values from two legs of the `EXCEPT` are not in output.
-- This basically shows that the comparison happens in a null-safe manner.
SELECT age, name FROM person
    EXCEPT
    SELECT age FROM unknown_age;
+---+--------+
|age|    name|
+---+--------+
| 30|     Joe|
| 50|    Fred|
| 30|Michelle|
| 18|    Mike|
| 50|     Dan|
+---+--------+

-- Performs `UNION` operation between two sets of data.
-- The comparison between columns of the row ae done in
-- null-safe manner.
SELECT name, age FROM person
    UNION
    SELECT name, age FROM unknown_age;
+--------+----+
|    name| age|
+--------+----+
|  Albert|null|
|     Joe|  30|
|Michelle|  30|
|   Marry|null|
|    Fred|  50|
|    Mike|  18|
|     Dan|  50|
+--------+----+
```

## <a name="exists-and-not-exists-subqueries"></a>``EXISTS`` 和 ``NOT EXISTS`` 子查询

在 Spark 中，在 ``WHERE`` 子句中可使用 ``EXISTS`` 和 ``NOT EXISTS`` 表达式。
这些是返回 ``TRUE`` 或 ``FALSE`` 的布尔表达式。 换句话说，``EXISTS`` 是成员身份条件，当它引用的子查询返回一个或多个行时，它将返回 ``TRUE``。 类似地，NOT EXISTS 是非成员身份条件，当子查询没有返回行时，它将返回 ``TRUE``。

子查询的结果中存在 NULL 不会对这两个表达式产生影响。 这两个表达式通常更快，因为它们可以在无需特别预配 Null 认知的情况下转换为半联接/反半联接。

### <a name="examples"></a>示例

```sql
-- Even if subquery produces rows with `NULL` values, the `EXISTS` expression
-- evaluates to `TRUE` as the subquery produces 1 row.
SELECT * FROM person WHERE EXISTS (SELECT null);
+--------+----+
|    name| age|
+--------+----+
|  Albert|null|
|Michelle|  30|
|    Fred|  50|
|    Mike|  18|
|     Dan|  50|
|   Marry|null|
|     Joe|  30|
+--------+----+

-- `NOT EXISTS` expression returns `FALSE`. It returns `TRUE` only when
-- subquery produces no rows. In this case, it returns 1 row.
SELECT * FROM person WHERE NOT EXISTS (SELECT null);
+----+---+
|name|age|
+----+---+
+----+---+

-- `NOT EXISTS` expression returns `TRUE`.
SELECT * FROM person WHERE NOT EXISTS (SELECT 1 WHERE 1 = 0);
+--------+----+
|    name| age|
+--------+----+
|  Albert|null|
|Michelle|  30|
|    Fred|  50|
|    Mike|  18|
|     Dan|  50|
|   Marry|null|
|     Joe|  30|
+--------+----+
```

## <a name="in-and-not-in-subqueries"></a>``IN`` 和 ``NOT IN`` 子查询

在 Spark 中，在查询的 ``WHERE`` 子句中可使用 ``IN`` 和 ``NOT IN`` 表达式。 与 ``EXISTS`` 表达式不同，``IN`` 表达式可以返回 ``TRUE``、``FALSE`` 或 ``UNKNOWN (NULL)`` 值。 从概念上讲，``IN`` 表达式在语义上等效于用析取运算符 (``OR``) 分隔的一组相等条件。
例如，c1 IN (1, 2, 3) 在语义上等效于 ``(C1 = 1 OR c1 = 2 OR c1 = 3)``。

至于处理 ``NULL`` 值，可根据比较运算符 (``=``) 和逻辑运算符 (``OR``) 中的 ``NULL`` 值处理推导出语义。
简而言之，以下规则规定如何计算 ``IN`` 表达式的结果。

* 在列表中发现提到的非 NULL 值时，返回 ``TRUE``
* 在列表中未发现非 NULL 值且列表不包含 NULL 值时，返回 ``FALSE``
* 值为 ``NULL``，或者在列表中未发现非 NULL 值且列表至少包含一个 ``NULL`` 值时，返回 ``UNKNOWN``

列表包含 ``NULL`` 时，无论输入值如何，``NOT IN`` 始终返回 UNKNOWN。
发生此情况的原因是：值不在包含 ``NULL`` 的列表中时 ``IN`` 返回 ``UNKNOWN``，并且 ``NOT UNKNOWN`` 还是 ``UNKNOWN``。

### <a name="examples"></a>示例

```sql
-- The subquery has only `NULL` value in its result set. Therefore,
-- the result of `IN` predicate is UNKNOWN.
SELECT * FROM person WHERE age IN (SELECT null);
+----+---+
|name|age|
+----+---+
+----+---+

-- The subquery has `NULL` value in the result set as well as a valid
-- value `50`. Rows with age = 50 are returned.
SELECT * FROM person
    WHERE age IN (SELECT age FROM VALUES (50), (null) sub(age));
+----+---+
|name|age|
+----+---+
|Fred| 50|
| Dan| 50|
+----+---+

-- Since subquery has `NULL` value in the result set, the `NOT IN`
-- predicate would return UNKNOWN. Hence, no rows are
-- qualified for this query.
SELECT * FROM person
    WHERE age NOT IN (SELECT age FROM VALUES (50), (null) sub(age));
+----+---+
|name|age|
+----+---+
+----+---+
```