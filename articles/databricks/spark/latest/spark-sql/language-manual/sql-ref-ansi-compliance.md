---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ANSI 合规性 - Azure Databricks
description: 了解 Azure Databricks 中支持的 SQL 语言构造中的 ANSI 合规性。
ms.openlocfilehash: 39508abbc81548cbd86e9fa68e804937817c9f0d
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060825"
---
# <a name="ansi-compliance"></a>ANSI 合规性

Spark SQL 提供两个试验性选项以支持符合 ANSI SQL 标准：``spark.sql.ansi.enabled`` 和 ``spark.sql.storeAssignmentPolicy``。

如果 ``spark.sql.ansi.enabled`` 设置为 ``true``，Spark SQL 将遵循基本行为中的标准（例如，算术运算、类型转换、SQL 函数和 SQL 分析）。
此外，在表中插入行时，Spark SQL 提供独立的选项来控制隐式强制转换行为。
强制转换行为在标准为定义为存储分配规则。

将 ``spark.sql.storeAssignmentPolicy`` 设置为 ``ANSI`` 时，Spark SQL 将遵循 ANSI 存储分配规则。 这是一个单独的配置，因为其默认值为 ``ANSI``，而配置 ``spark.sql.ansi.enabled`` 默认处于禁用状态。

下表对此行为进行了汇总：

| 属性名称                       | 默认                           | 含义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|-------------------------------------|-----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ``spark.sql.ansi.enabled``          | false                             | （试验）为 true 时，Spark 会尝试遵循 ANSI SQL 规范：<br><br>* 如果在对整数或十进制数字段进行任何操作时发生溢出，则会引发运行时异常。<br>* 禁止在 SQL 分析器中使用 ANSI SQL 的保留关键字作为标识符。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ``spark.sql.storeAssignmentPolicy`` | ANSI                              | （试验）向具有不同数据类型的列中插入值时，Spark 会执行类型强制转换。  类型强制转换规则有三种策略：``ANSI``、``legacy`` 和 ``strict``。<br><br>* ``ANSI``：Spark 根据 ANSI SQL 执行类型强制转换。 实际上，此行为与 PostgreSQL 基本相同。  它不允许某些不合理的类型转换，例如将字符串转换为 int 或将 double 转换为布尔。<br>* ``legacy``：Spark 允许类型强制转换，前提是它是有效的强制转换，这非常宽松。  例如，允许将字符串转换为 int 或将 double 转换为布尔。  它也是 Spark 2.x 中的唯一行为，并与 Hive 兼容。<br>* ``strict``：Spark 不允许类型强制转换过程出现任何可能的精度受损或数据截断，例如，不允许将 double 转换为 int 或将 decimal 转换为 double。 |

以下各子部分显示了启用 ANSI 模式时算术运算、类型转换和 SQL 分析的行为变更。

## <a name="arithmetic-operations"></a>算术运算

在 Spark SQL 中，默认情况下，不会检查对数值类型（十进制数除外）执行的算术运算是否溢出。
这意味着，如果某个操作导致溢出，则结果与 Java 或 Scala 程序中相应的操作结果相同（例如，如果 2 个整数的和大于可表示的最大值，则结果为负数）。 另一方面，对于十进制数溢出，Spark SQL 会返回 NULL。
将 ``spark.sql.ansi.enabled`` 设置为 ``true``，并且数值和间隔算术运算中发生溢出时，它会在运行时引发算术异常。

```sql
-- `spark.sql.ansi.enabled=true`
SELECT 2147483647 + 1;
java.lang.ArithmeticException: integer overflow

-- `spark.sql.ansi.enabled=false`
SELECT 2147483647 + 1;
+----------------+
|(2147483647 + 1)|
+----------------+
|     -2147483648|
+----------------+
```

## <a name="type-conversion"></a>类型转换

Spark SQL 具有三种类型的转换：显式强制转换、类型强制转换和存储分配转换。
将 ``spark.sql.ansi.enabled`` 设置为 ``true`` 时，通过 ``CAST`` 语法进行显式强制转换会在标准中定义的非法强制转换模式下引发运行时异常，例如将字符串转换为整数。
另一方面，通过 ``spark.sql.storeAssignmentPolicy=ANSI`` 启用 ANSI 模式时，``INSERT INTO`` 语法会引发分析异常。

目前，ANSI 模式仅影响显式强制转换和分配强制转换。
在将来的版本中，类型强制转换的行为可能会随其他两种类型转换规则一起更改。

```sql
-- Examples of explicit casting

-- `spark.sql.ansi.enabled=true`
SELECT CAST('a' AS INT);
java.lang.NumberFormatException: invalid input syntax for type numeric: a

SELECT CAST(2147483648L AS INT);
java.lang.ArithmeticException: Casting 2147483648 to int causes overflow

-- `spark.sql.ansi.enabled=false` (This is a default behavior)
SELECT CAST('a' AS INT);
+--------------+
|CAST(a AS INT)|
+--------------+
|          null|
+--------------+

SELECT CAST(2147483648L AS INT);
+-----------------------+
|CAST(2147483648 AS INT)|
+-----------------------+
|            -2147483648|
+-----------------------+

-- Examples of store assignment rules
CREATE TABLE t (v INT);

-- `spark.sql.storeAssignmentPolicy=ANSI`
INSERT INTO t VALUES ('1');
org.apache.spark.sql.AnalysisException: Cannot write incompatible data to table '`default`.`t`':
- Cannot safely cast 'v': string to int;

-- `spark.sql.storeAssignmentPolicy=LEGACY` (This is a legacy behavior until Spark 2.x)
INSERT INTO t VALUES ('1');
SELECT * FROM t;
+---+
|  v|
+---+
|  1|
+---+
```

## <a name="sql-functions"></a>SQL 函数

在 ANSI 模式 (``spark.sql.ansi.enabled=true``) 下，某些 SQL 函数的行为可能会有所不同。

* ``size``：在 ANSI 模式下，对于 NULL 输入，此函数将返回 NULL。

## <a name="sql-keywords"></a>SQL 关键字

如果 ``spark.sql.ansi.enabled`` 为 true，Spark SQL 将使用 ANSI 模式分析器。
在此模式下，Spark SQL 有两种关键字：
* 保留关键字：保留的关键字，不能用作表、视图、列、函数、别名等的标识符。
* 非保留关键字：仅在特定上下文中具有特殊含义的关键字，在其他上下文中可用作标识符。 例如，``EXPLAIN SELECT ...`` 是一个命令，但 EXPLAIN 在其他位置可用作标识符。

禁用 ANSI 模式时，Spark SQL 有两种关键字：

* 非保留关键字：与 ANSI 模式处于启用状态下的定义相同。
* 严格非保留关键字：非保留关键字的 strict 版本，无法用作表别名。

默认情况下，``spark.sql.ansi.enabled`` 为 false。

下面是 Spark SQL 中所有关键字的列表。

| 关键字           | Spark SQL ANSI 模式 | Spark SQL 默认模式 | SQL-2016      |
|-------------------|---------------------|------------------------|---------------|
| ADD               | non-reserved        | non-reserved           | non-reserved  |
| AFTER             | non-reserved        | non-reserved           | non-reserved  |
| ALL               | reserved            | non-reserved           | reserved      |
| ALTER             | non-reserved        | non-reserved           | reserved      |
| 分析           | non-reserved        | non-reserved           | non-reserved  |
| AND               | reserved            | non-reserved           | reserved      |
| ANTI              | non-reserved        | strict-non-reserved    | non-reserved  |
| ANY               | reserved            | non-reserved           | reserved      |
| 存档           | non-reserved        | non-reserved           | non-reserved  |
| ARRAY             | non-reserved        | non-reserved           | reserved      |
| AS                | reserved            | non-reserved           | reserved      |
| ASC               | non-reserved        | non-reserved           | non-reserved  |
| AT                | non-reserved        | non-reserved           | reserved      |
| AUTHORIZATION     | reserved            | non-reserved           | reserved      |
| BETWEEN           | non-reserved        | non-reserved           | reserved      |
| BOTH              | reserved            | non-reserved           | reserved      |
| BUCKET            | non-reserved        | non-reserved           | non-reserved  |
| BUCKETS           | non-reserved        | non-reserved           | non-reserved  |
| BY                | non-reserved        | non-reserved           | reserved      |
| CACHE             | non-reserved        | non-reserved           | non-reserved  |
| CASCADE           | non-reserved        | non-reserved           | non-reserved  |
| CASE              | reserved            | non-reserved           | reserved      |
| CAST              | reserved            | non-reserved           | reserved      |
| 更改            | non-reserved        | non-reserved           | non-reserved  |
| CHECK             | reserved            | non-reserved           | reserved      |
| CLEAR             | non-reserved        | non-reserved           | non-reserved  |
| CLUSTER           | non-reserved        | non-reserved           | non-reserved  |
| CLUSTERED         | non-reserved        | non-reserved           | non-reserved  |
| CODEGEN           | non-reserved        | non-reserved           | non-reserved  |
| COLLATE           | reserved            | non-reserved           | reserved      |
| COLLECTION        | non-reserved        | non-reserved           | non-reserved  |
| COLUMN            | reserved            | non-reserved           | reserved      |
| COLUMNS           | non-reserved        | non-reserved           | non-reserved  |
| COMMENT           | non-reserved        | non-reserved           | non-reserved  |
| COMMIT            | non-reserved        | non-reserved           | reserved      |
| COMPACT           | non-reserved        | non-reserved           | non-reserved  |
| COMPACTIONS       | non-reserved        | non-reserved           | non-reserved  |
| COMPUTE           | non-reserved        | non-reserved           | non-reserved  |
| CONCATENATE       | non-reserved        | non-reserved           | non-reserved  |
| CONSTRAINT        | reserved            | non-reserved           | reserved      |
| COST              | non-reserved        | non-reserved           | non-reserved  |
| CREATE            | reserved            | non-reserved           | reserved      |
| CROSS             | reserved            | strict-non-reserved    | reserved      |
| CUBE              | non-reserved        | non-reserved           | reserved      |
| CURRENT           | non-reserved        | non-reserved           | reserved      |
| CURRENT_DATE      | reserved            | non-reserved           | reserved      |
| CURRENT_TIME      | reserved            | non-reserved           | reserved      |
| CURRENT_TIMESTAMP | reserved            | non-reserved           | reserved      |
| CURRENT_USER      | reserved            | non-reserved           | reserved      |
| DATA              | non-reserved        | non-reserved           | non-reserved  |
| DATABASE          | non-reserved        | non-reserved           | non-reserved  |
| 数据库         | non-reserved        | non-reserved           | non-reserved  |
| DAY               | reserved            | non-reserved           | reserved      |
| DBPROPERTIES      | non-reserved        | non-reserved           | non-reserved  |
| DEFINED           | non-reserved        | non-reserved           | non-reserved  |
| 删除            | non-reserved        | non-reserved           | reserved      |
| DELIMITED         | non-reserved        | non-reserved           | non-reserved  |
| DESC              | non-reserved        | non-reserved           | non-reserved  |
| DESCRIBE          | non-reserved        | non-reserved           | reserved      |
| DFS               | non-reserved        | non-reserved           | non-reserved  |
| DIRECTORIES       | non-reserved        | non-reserved           | non-reserved  |
| 目录         | non-reserved        | non-reserved           | non-reserved  |
| DISTINCT          | reserved            | non-reserved           | reserved      |
| DISTRIBUTE        | non-reserved        | non-reserved           | non-reserved  |
| DIV               | non-reserved        | non-reserved           | 非关键字 |
| DROP              | non-reserved        | non-reserved           | reserved      |
| ELSE              | reserved            | non-reserved           | reserved      |
| End               | reserved            | non-reserved           | reserved      |
| ESCAPE            | reserved            | non-reserved           | reserved      |
| ESCAPED           | non-reserved        | non-reserved           | non-reserved  |
| EXCEPT            | reserved            | strict-non-reserved    | reserved      |
| EXCHANGE          | non-reserved        | non-reserved           | non-reserved  |
| EXISTS            | non-reserved        | non-reserved           | reserved      |
| EXPLAIN           | non-reserved        | non-reserved           | non-reserved  |
| EXPORT            | non-reserved        | non-reserved           | non-reserved  |
| EXTENDED          | non-reserved        | non-reserved           | non-reserved  |
| EXTERNAL          | non-reserved        | non-reserved           | reserved      |
| EXTRACT           | non-reserved        | non-reserved           | reserved      |
| FALSE             | reserved            | non-reserved           | reserved      |
| FETCH             | reserved            | non-reserved           | reserved      |
| 字段            | non-reserved        | non-reserved           | non-reserved  |
| FILTER            | reserved            | non-reserved           | reserved      |
| FILEFORMAT        | non-reserved        | non-reserved           | non-reserved  |
| FIRST             | non-reserved        | non-reserved           | non-reserved  |
| FOLLOWING         | non-reserved        | non-reserved           | non-reserved  |
| FOR               | reserved            | non-reserved           | reserved      |
| FOREIGN           | reserved            | non-reserved           | reserved      |
| FORMAT            | non-reserved        | non-reserved           | non-reserved  |
| FORMATTED         | non-reserved        | non-reserved           | non-reserved  |
| FROM              | reserved            | non-reserved           | reserved      |
| FULL              | reserved            | strict-non-reserved    | reserved      |
| FUNCTION          | non-reserved        | non-reserved           | reserved      |
| FUNCTIONS         | non-reserved        | non-reserved           | non-reserved  |
| GLOBAL            | non-reserved        | non-reserved           | reserved      |
| GRANT             | reserved            | non-reserved           | reserved      |
| GROUP             | reserved            | non-reserved           | reserved      |
| GROUPING          | non-reserved        | non-reserved           | reserved      |
| HAVING            | reserved            | non-reserved           | reserved      |
| HOUR              | reserved            | non-reserved           | reserved      |
| IF                | non-reserved        | non-reserved           | 非关键字 |
| IGNORE            | non-reserved        | non-reserved           | non-reserved  |
| IMPORT            | non-reserved        | non-reserved           | non-reserved  |
| IN                | reserved            | non-reserved           | reserved      |
| INDEX             | non-reserved        | non-reserved           | non-reserved  |
| 索引           | non-reserved        | non-reserved           | non-reserved  |
| INNER             | reserved            | strict-non-reserved    | reserved      |
| INPATH            | non-reserved        | non-reserved           | non-reserved  |
| INPUTFORMAT       | non-reserved        | non-reserved           | non-reserved  |
| INSERT            | non-reserved        | non-reserved           | reserved      |
| INTERSECT         | reserved            | strict-non-reserved    | reserved      |
| INTERVAL          | non-reserved        | non-reserved           | reserved      |
| INTO              | reserved            | non-reserved           | reserved      |
| IS                | reserved            | non-reserved           | reserved      |
| 项             | non-reserved        | non-reserved           | non-reserved  |
| JOIN              | reserved            | strict-non-reserved    | reserved      |
| 密钥              | non-reserved        | non-reserved           | non-reserved  |
| LAST              | non-reserved        | non-reserved           | non-reserved  |
| LATERAL           | non-reserved        | non-reserved           | reserved      |
| LAZY              | non-reserved        | non-reserved           | non-reserved  |
| LEADING           | reserved            | non-reserved           | reserved      |
| LEFT              | reserved            | strict-non-reserved    | reserved      |
| LIKE              | non-reserved        | non-reserved           | reserved      |
| LIMIT             | non-reserved        | non-reserved           | non-reserved  |
| LINES             | non-reserved        | non-reserved           | non-reserved  |
| 列表              | non-reserved        | non-reserved           | non-reserved  |
| LOAD              | non-reserved        | non-reserved           | non-reserved  |
| LOCAL             | non-reserved        | non-reserved           | reserved      |
| LOCATION          | non-reserved        | non-reserved           | non-reserved  |
| LOCK              | non-reserved        | non-reserved           | non-reserved  |
| LOCKS             | non-reserved        | non-reserved           | non-reserved  |
| LOGICAL           | non-reserved        | non-reserved           | non-reserved  |
| MACRO             | non-reserved        | non-reserved           | non-reserved  |
| MAP               | non-reserved        | non-reserved           | non-reserved  |
| MATCHED           | non-reserved        | non-reserved           | non-reserved  |
| MERGE             | non-reserved        | non-reserved           | non-reserved  |
| MINUS             | non-reserved        | strict-non-reserved    | non-reserved  |
| MINUTE            | reserved            | non-reserved           | reserved      |
| MONTH             | reserved            | non-reserved           | reserved      |
| MSCK              | non-reserved        | non-reserved           | non-reserved  |
| 命名空间         | non-reserved        | non-reserved           | non-reserved  |
| 命名空间        | non-reserved        | non-reserved           | non-reserved  |
| NATURAL           | reserved            | strict-non-reserved    | reserved      |
| 是                | non-reserved        | non-reserved           | reserved      |
| NOT               | reserved            | non-reserved           | reserved      |
| Null              | reserved            | non-reserved           | reserved      |
| NULLS             | non-reserved        | non-reserved           | non-reserved  |
| OF                | non-reserved        | non-reserved           | reserved      |
| ON                | reserved            | strict-non-reserved    | reserved      |
| ONLY              | reserved            | non-reserved           | reserved      |
| OPTION            | non-reserved        | non-reserved           | non-reserved  |
| OPTIONS           | non-reserved        | non-reserved           | non-reserved  |
| 或                | reserved            | non-reserved           | reserved      |
| ORDER             | reserved            | non-reserved           | reserved      |
| OUT               | non-reserved        | non-reserved           | reserved      |
| OUTER             | reserved            | non-reserved           | reserved      |
| OUTPUTFORMAT      | non-reserved        | non-reserved           | non-reserved  |
| OVER              | non-reserved        | non-reserved           | non-reserved  |
| OVERLAPS          | reserved            | non-reserved           | reserved      |
| OVERLAY           | non-reserved        | non-reserved           | non-reserved  |
| OVERWRITE         | non-reserved        | non-reserved           | non-reserved  |
| PARTITION         | non-reserved        | non-reserved           | reserved      |
| PARTITIONED       | non-reserved        | non-reserved           | non-reserved  |
| 分区        | non-reserved        | non-reserved           | non-reserved  |
| PERCENT           | non-reserved        | non-reserved           | non-reserved  |
| PIVOT             | non-reserved        | non-reserved           | non-reserved  |
| PLACING           | non-reserved        | non-reserved           | non-reserved  |
| POSITION          | non-reserved        | non-reserved           | reserved      |
|  PRECEDING         | non-reserved        | non-reserved           | non-reserved  |
| PRIMARY           | reserved            | non-reserved           | reserved      |
| PRINCIPALS        | non-reserved        | non-reserved           | non-reserved  |
| PROPERTIES        | non-reserved        | non-reserved           | non-reserved  |
| PURGE             | non-reserved        | non-reserved           | non-reserved  |
| QUERY             | non-reserved        | non-reserved           | non-reserved  |
| RANGE             | non-reserved        | non-reserved           | reserved      |
| RECORDREADER      | non-reserved        | non-reserved           | non-reserved  |
| RECORDWRITER      | non-reserved        | non-reserved           | non-reserved  |
| RECOVER           | non-reserved        | non-reserved           | non-reserved  |
| REDUCE            | non-reserved        | non-reserved           | non-reserved  |
| REFERENCES        | reserved            | non-reserved           | reserved      |
| REFRESH           | non-reserved        | non-reserved           | non-reserved  |
| REGEXP            | non-reserved        | non-reserved           | 非关键字 |
| RENAME            | non-reserved        | non-reserved           | non-reserved  |
| REPAIR            | non-reserved        | non-reserved           | non-reserved  |
| REPLACE           | non-reserved        | non-reserved           | non-reserved  |
| RESET             | non-reserved        | non-reserved           | non-reserved  |
| RESTRICT          | non-reserved        | non-reserved           | non-reserved  |
| REVOKE            | non-reserved        | non-reserved           | reserved      |
| RIGHT             | reserved            | strict-non-reserved    | reserved      |
| RLIKE             | non-reserved        | non-reserved           | non-reserved  |
| ROLE              | non-reserved        | non-reserved           | non-reserved  |
| ROLES             | non-reserved        | non-reserved           | non-reserved  |
| ROLLBACK          | non-reserved        | non-reserved           | reserved      |
| ROLLUP            | non-reserved        | non-reserved           | reserved      |
| ROW               | non-reserved        | non-reserved           | reserved      |
| ROWS              | non-reserved        | non-reserved           | reserved      |
| SCHEMA            | non-reserved        | non-reserved           | non-reserved  |
| SCHEMAS           | non-reserved        | non-reserved           | 非关键字 |
| SECOND            | reserved            | non-reserved           | reserved      |
| SELECT            | reserved            | non-reserved           | reserved      |
| SEMI              | non-reserved        | strict-non-reserved    | non-reserved  |
| SEPARATED         | non-reserved        | non-reserved           | non-reserved  |
| SERDE             | non-reserved        | non-reserved           | non-reserved  |
| SERDEPROPERTIES   | non-reserved        | non-reserved           | non-reserved  |
| SESSION_USER      | reserved            | non-reserved           | reserved      |
| SET               | non-reserved        | non-reserved           | reserved      |
| SETS              | non-reserved        | non-reserved           | non-reserved  |
| SHOW              | non-reserved        | non-reserved           | non-reserved  |
| SKEWED            | non-reserved        | non-reserved           | non-reserved  |
| SOME              | reserved            | non-reserved           | reserved      |
| SORT              | non-reserved        | non-reserved           | non-reserved  |
| SORTED            | non-reserved        | non-reserved           | non-reserved  |
| START             | non-reserved        | non-reserved           | reserved      |
| STATISTICS        | non-reserved        | non-reserved           | non-reserved  |
| STORED            | non-reserved        | non-reserved           | non-reserved  |
| STRATIFY          | non-reserved        | non-reserved           | non-reserved  |
| STRUCT            | non-reserved        | non-reserved           | non-reserved  |
| SUBSTR            | non-reserved        | non-reserved           | non-reserved  |
| SUBSTRING         | non-reserved        | non-reserved           | non-reserved  |
| TABLE             | reserved            | non-reserved           | reserved      |
| TABLES            | non-reserved        | non-reserved           | non-reserved  |
| TABLESAMPLE       | non-reserved        | non-reserved           | reserved      |
| TBLPROPERTIES     | non-reserved        | non-reserved           | non-reserved  |
| TEMP              | non-reserved        | non-reserved           | 非关键字 |
| TEMPORARY         | non-reserved        | non-reserved           | non-reserved  |
| 已终止        | non-reserved        | non-reserved           | non-reserved  |
| THEN              | reserved            | non-reserved           | reserved      |
| TIME              | reserved            | non-reserved           | reserved      |
| TO                | reserved            | non-reserved           | reserved      |
| TOUCH             | non-reserved        | non-reserved           | non-reserved  |
| TRAILING          | reserved            | non-reserved           | reserved      |
| TRANSACTION       | non-reserved        | non-reserved           | non-reserved  |
| TRANSACTIONS      | non-reserved        | non-reserved           | non-reserved  |
| TRANSFORM         | non-reserved        | non-reserved           | non-reserved  |
| TRIM              | non-reserved        | non-reserved           | non-reserved  |
| TRUE              | non-reserved        | non-reserved           | reserved      |
| TRUNCATE          | non-reserved        | non-reserved           | reserved      |
| TYPE              | non-reserved        | non-reserved           | non-reserved  |
| UNARCHIVE         | non-reserved        | non-reserved           | non-reserved  |
| UNBOUNDED         | non-reserved        | non-reserved           | non-reserved  |
| UNCACHE           | non-reserved        | non-reserved           | non-reserved  |
| UNION             | reserved            | strict-non-reserved    | reserved      |
| UNIQUE            | reserved            | non-reserved           | reserved      |
| 未知           | reserved            | non-reserved           | reserved      |
| UNLOCK            | non-reserved        | non-reserved           | non-reserved  |
| UNSET             | non-reserved        | non-reserved           | non-reserved  |
| UPDATE            | non-reserved        | non-reserved           | reserved      |
| USE               | non-reserved        | non-reserved           | non-reserved  |
| USER              | reserved            | non-reserved           | reserved      |
| USING             | reserved            | strict-non-reserved    | reserved      |
| VALUES            | non-reserved        | non-reserved           | reserved      |
| VIEW              | non-reserved        | non-reserved           | non-reserved  |
| VIEWS             | non-reserved        | non-reserved           | non-reserved  |
| WHEN              | reserved            | non-reserved           | reserved      |
| WHERE             | reserved            | non-reserved           | reserved      |
| WINDOW            | non-reserved        | non-reserved           | reserved      |
| WITH              | reserved            | non-reserved           | reserved      |
| YEAR              | reserved            | non-reserved           | reserved      |
| ZONE              | non-reserved        | non-reserved           | non-reserved  |