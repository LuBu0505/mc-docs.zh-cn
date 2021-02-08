---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 用户定义的标量函数 (UDF) - Azure Databricks
description: 了解 Azure Databricks 支持的 SQL 语言构造中的 SQL 标量用户定义函数。
ms.openlocfilehash: 3aeeb0a17994f82fd331c9f51cc1ff4f79d67274
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060655"
---
# <a name="user-defined-scalar-functions-udfs"></a>用户定义的标量函数 (UDF)

用户定义的标量函数 (UDF) 是作用于一行的用户可编程例程。 此文档列出了创建和注册 UDF 所需的类。 它还包含演示如何在 Spark SQL 中定义和注册 UDF 以及调用它们的示例。

## <a name="userdefinedfunction-class"></a>``UserDefinedFunction`` 类

若要定义用户定义函数的属性，可以使用此类中定义的某些方法。

* **asNonNullable():UserDefinedFunction**：将 ``UserDefinedFunction`` 更新为不可为 null。
* **asNondeterministic():UserDefinedFunction**：将 ``UserDefinedFunction`` 更新为具有不确定性。
* **withName(name:String):UserDefinedFunction**：使用给定名称更新 ``UserDefinedFunction``。

## <a name="examples"></a>示例

### <a name="scala"></a>Scala

```scala
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions.udf

val spark = SparkSession
      .builder()
      .appName("Spark SQL UDF scalar example")
      .getOrCreate()

// Define and register a zero-argument non-deterministic UDF
// UDF is deterministic by default, i.e. produces the same result for the same input.
val random = udf(() => Math.random())
spark.udf.register("random", random.asNondeterministic())
spark.sql("SELECT random()").show()
// +-------+
// |UDF()  |
// +-------+
// |xxxxxxx|
// +-------+

// Define and register a one-argument UDF
val plusOne = udf((x: Int) => x + 1)
spark.udf.register("plusOne", plusOne)
spark.sql("SELECT plusOne(5)").show()
// +------+
// |UDF(5)|
// +------+
// |     6|
// +------+

// Define a two-argument UDF and register it with Spark in one step
spark.udf.register("strLenScala", (_: String).length + (_: Int))
spark.sql("SELECT strLenScala('test', 1)").show()
// +--------------------+
// |strLenScala(test, 1)|
// +--------------------+
// |                   5|
// +--------------------+

// UDF in a WHERE clause
spark.udf.register("oneArgFilter", (n: Int) => { n > 5 })
spark.range(1, 10).createOrReplaceTempView("test")
spark.sql("SELECT * FROM test WHERE oneArgFilter(id)").show()
// +---+
// | id|
// +---+
// |  6|
// |  7|
// |  8|
// |  9|
// +---+
```

### <a name="java"></a>Java

```java
import org.apache.spark.sql.*;
import org.apache.spark.sql.api.java.UDF1;
import org.apache.spark.sql.expressions.UserDefinedFunction;
import static org.apache.spark.sql.functions.udf;
import org.apache.spark.sql.types.DataTypes;

SparkSession spark = SparkSession
      .builder()
      .appName("Java Spark SQL UDF scalar example")
      .getOrCreate();

// Define and register a zero-argument non-deterministic UDF
// UDF is deterministic by default, i.e. produces the same result for the same input.
UserDefinedFunction random = udf(
  () -> Math.random(), DataTypes.DoubleType
);
random.asNondeterministic();
spark.udf().register("random", random);
spark.sql("SELECT random()").show();
// +-------+
// |UDF()  |
// +-------+
// |xxxxxxx|
// +-------+

// Define and register a one-argument UDF
spark.udf().register("plusOne", new UDF1<Integer, Integer>() {
  @Override
  public Integer call(Integer x) {
    return x + 1;
  }
}, DataTypes.IntegerType);
spark.sql("SELECT plusOne(5)").show();
// +----------+
// |plusOne(5)|
// +----------+
// |         6|
// +----------+

// Define and register a two-argument UDF
UserDefinedFunction strLen = udf(
  (String s, Integer x) -> s.length() + x, DataTypes.IntegerType
);
spark.udf().register("strLen", strLen);
spark.sql("SELECT strLen('test', 1)").show();
// +------------+
// |UDF(test, 1)|
// +------------+
// |           5|
// +------------+

// UDF in a WHERE clause
spark.udf().register("oneArgFilter", new UDF1<Long, Boolean>() {
  @Override
  public Boolean call(Long x) {
    return  x > 5;
  }
}, DataTypes.BooleanType);
spark.range(1, 10).createOrReplaceTempView("test");
spark.sql("SELECT * FROM test WHERE oneArgFilter(id)").show();
// +---+
// | id|
// +---+
// |  6|
// |  7|
// |  8|
// |  9|
// +---+
```

## <a name="related-statements"></a>相关语句

* [用户定义的聚合函数 (UDAF)](sql-ref-functions-udf-aggregate.md)
* [与 Hive UDF、UDAF 和 UDTF 的集成](sql-ref-functions-udf-hive.md)