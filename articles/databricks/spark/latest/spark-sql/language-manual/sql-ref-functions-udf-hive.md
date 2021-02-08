---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 与 Hive UDF、UDAF 和 UDTF 的集成 - Azure Databricks
description: 了解有关 Azure Databricks 支持的 SQL 语言结构中的 SQL 与 Hive UDF、UDAF 和 UDTF 的集成的信息。
ms.openlocfilehash: 802bedee5011ab8dc3fae9b954878b737e44413f
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060656"
---
# <a name="integration-with-hive-udfs-udafs-and-udtfs"></a>与 Hive UDF、UDAF 和 UDTF 的集成

Spark SQL 支持 Hive UDF、UDAF 和 UDTF 的集成。 与 Spark UDF 和 UDAF 类似，Hive UDF 在一行上工作并作为输入，然后生成一行作为输出，而 Hive UDAF 在多行上工作并由此返回一个聚合行。 此外，Hive 还支持 UDTF（用户定义的表格函数），它们在一行上运作并作为输入，然后返回多行作为输出。 若要使用 Hive UDF/UDAF/UTF，用户应在 Spark 中注册它们，然后在 Spark SQL 查询中使用它们。

## <a name="examples"></a>示例

Hive 具有两个 UDF 接口：[UDF](https://github.com/apache/hive/blob/master/udf/src/java/org/apache/hadoop/hive/ql/exec/UDF.java) 和 [GenericUDF](https://github.com/apache/hive/blob/master/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDF.java)。
下面的示例使用从 ``GenericUDF`` 派生的 [GenericUDFAbs](https://github.com/apache/hive/blob/master/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDFAbs.java)。

```sql
-- Register `GenericUDFAbs` and use it in Spark SQL.
-- Note that, if you use your own programmed one, you need to add a JAR containing it
-- into a classpath,
-- e.g., ADD JAR yourHiveUDF.jar;
CREATE TEMPORARY FUNCTION testUDF AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDFAbs';

SELECT * FROM t;
+-----+
|value|
+-----+
| -1.0|
|  2.0|
| -3.0|
+-----+

SELECT testUDF(value) FROM t;
+--------------+
|testUDF(value)|
+--------------+
|           1.0|
|           2.0|
|           3.0|
+--------------+
```

下面的示例使用从 [GenericUDTF](https://github.com/apache/hive/blob/master/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDF.java) 派生的 [GenericUDTFExplode](https://github.com/apache/hive/blob/master/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDTFExplode.java)。

```sql
-- Register `GenericUDTFExplode` and use it in Spark SQL
CREATE TEMPORARY FUNCTION hiveUDTF
    AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDTFExplode';

SELECT * FROM t;
+------+
| value|
+------+
|[1, 2]|
|[3, 4]|
+------+

SELECT hiveUDTF(value) FROM t;
+---+
|col|
+---+
|  1|
|  2|
|  3|
|  4|
+---+
```

Hive 具有两个 UDAF 接口：[UDAF](https://github.com/apache/hive/blob/master/udf/src/java/org/apache/hadoop/hive/ql/exec/UDAF.java) 和 [GenericUDAFResolver](https://github.com/apache/hive/blob/master/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDAFResolver.java)。
下面的示例使用从 ``GenericUDAFResolver`` 派生的 [GenericUDAFSum](https://github.com/apache/hive/blob/master/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDAFSum.java)。

```sql
-- Register `GenericUDAFSum` and use it in Spark SQL
CREATE TEMPORARY FUNCTION hiveUDAF
    AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDAFSum';

SELECT * FROM t;
+---+-----+
|key|value|
+---+-----+
|  a|    1|
|  a|    2|
|  b|    3|
+---+-----+

SELECT key, hiveUDAF(value) FROM t GROUP BY key;
+---+---------------+
|key|hiveUDAF(value)|
+---+---------------+
|  b|              3|
|  a|              3|
+---+---------------+
```