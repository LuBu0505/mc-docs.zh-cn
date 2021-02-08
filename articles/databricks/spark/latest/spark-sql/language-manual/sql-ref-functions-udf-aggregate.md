---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 用户定义聚合函数 (UDAF) - Azure Databricks
description: 了解 Azure Databricks 支持的 SQL 语言构造中的 SQL 用户定义聚合函数。
ms.openlocfilehash: 47f11027ba3b6c709f90626a4197fb6cbb0be051
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060661"
---
# <a name="user-defined-aggregate-functions-udafs"></a>用户定义的聚合函数 (UDAF)

用户定义聚合函数 (UDAF) 是一次作用于多个行的用户可编程例程，它返回单个聚合值作为结果。 此文档列出了创建和注册 UDAF 所需的类。 它还包含演示了如何在 Scala 中定义和注册 UDAF 以及如何在 Spark SQL 中调用它们的示例。

## <a name="aggregator"></a>聚合器

**语法** ``Aggregator[-IN, BUF, OUT]``

用户定义的聚合的基类，可在数据集操作中使用，以获取组的所有元素并将其缩减为单个值。

* IN：聚合的输入类型。
* BUF：约简的中间值的类型。
* OUT：最终输出结果的类型。
* bufferEncoder:Encoder[BUF]

  中间值类型的编码器。

* finish(reduction:BUF):OUT

  转换约减的输出。

* merge(b1:BUF, b2:BUF):BUF

  合并两个中间值。

* outputEncoder:Encoder[OUT]

  最终输出值类型的编码器。

* reduce(b:BUF, a:IN):BUF

  将输入值 ``a`` 聚合为当前中间值。 为了提高性能，函数可以修改 ``b`` 并返回它，而不是为 ``b`` 构造新的对象。

* zero:BUF

  此聚合的中间结果的初始值。

## <a name="examples"></a>示例

### <a name="type-safe-user-defined-aggregate-functions"></a>类型安全的用户定义聚合函数

强类型化数据集的用户定义聚合围绕 ``Aggregator`` 抽象类进行。
例如，类型安全的用户定义平均值可以如下所示：

### <a name="untyped-user-defined-aggregate-functions"></a>非类型化的用户定义聚合函数

如上所述，类型化的聚合也可以注册为与数据帧配合使用的非类型化聚合 UDF。
例如，针对非类型化数据帧的用户定义平均值可能如下所示：

#### <a name="scala"></a>Scala

```scala
import org.apache.spark.sql.{Encoder, Encoders, SparkSession}
import org.apache.spark.sql.expressions.Aggregator
import org.apache.spark.sql.functions

case class Average(var sum: Long, var count: Long)

object MyAverage extends Aggregator[Long, Average, Double] {
  // A zero value for this aggregation. Should satisfy the property that any b + zero = b
  def zero: Average = Average(0L, 0L)
  // Combine two values to produce a new value. For performance, the function may modify `buffer`
  // and return it instead of constructing a new object
  def reduce(buffer: Average, data: Long): Average = {
    buffer.sum += data
    buffer.count += 1
    buffer
  }
  // Merge two intermediate values
  def merge(b1: Average, b2: Average): Average = {
    b1.sum += b2.sum
    b1.count += b2.count
    b1
  }
  // Transform the output of the reduction
  def finish(reduction: Average): Double = reduction.sum.toDouble / reduction.count
  // The Encoder for the intermediate value type
  def bufferEncoder: Encoder[Average] = Encoders.product
  // The Encoder for the final output value type
  def outputEncoder: Encoder[Double] = Encoders.scalaDouble
}

// Register the function to access it
spark.udf.register("myAverage", functions.udaf(MyAverage))

val df = spark.read.json("examples/src/main/resources/employees.json")
df.createOrReplaceTempView("employees")
df.show()
// +-------+------+
// |   name|salary|
// +-------+------+
// |Michael|  3000|
// |   Andy|  4500|
// | Justin|  3500|
// |  Berta|  4000|
// +-------+------+

val result = spark.sql("SELECT myAverage(salary) as average_salary FROM employees")
result.show()
// +--------------+
// |average_salary|
// +--------------+
// |        3750.0|
// +--------------+
```

#### <a name="java"></a>Java

```java
import java.io.Serializable;

import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Encoder;
import org.apache.spark.sql.Encoders;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.expressions.Aggregator;
import org.apache.spark.sql.functions;

public static class Average implements Serializable  {
    private long sum;
    private long count;

    // Constructors, getters, setters...

}

public static class MyAverage extends Aggregator<Long, Average, Double> {
  // A zero value for this aggregation. Should satisfy the property that any b + zero = b
  public Average zero() {
    return new Average(0L, 0L);
  }
  // Combine two values to produce a new value. For performance, the function may modify `buffer`
  // and return it instead of constructing a new object
  public Average reduce(Average buffer, Long data) {
    long newSum = buffer.getSum() + data;
    long newCount = buffer.getCount() + 1;
    buffer.setSum(newSum);
    buffer.setCount(newCount);
    return buffer;
  }
  // Merge two intermediate values
  public Average merge(Average b1, Average b2) {
    long mergedSum = b1.getSum() + b2.getSum();
    long mergedCount = b1.getCount() + b2.getCount();
    b1.setSum(mergedSum);
    b1.setCount(mergedCount);
    return b1;
  }
  // Transform the output of the reduction
  public Double finish(Average reduction) {
    return ((double) reduction.getSum()) / reduction.getCount();
  }
  // The Encoder for the intermediate value type
  public Encoder<Average> bufferEncoder() {
    return Encoders.bean(Average.class);
  }
  // The Encoder for the final output value type
  public Encoder<Double> outputEncoder() {
    return Encoders.DOUBLE();
  }
}

// Register the function to access it
spark.udf().register("myAverage", functions.udaf(new MyAverage(), Encoders.LONG()));

Dataset<Row> df = spark.read().json("examples/src/main/resources/employees.json");
df.createOrReplaceTempView("employees");
df.show();
// +-------+------+
// |   name|salary|
// +-------+------+
// |Michael|  3000|
// |   Andy|  4500|
// | Justin|  3500|
// |  Berta|  4000|
// +-------+------+

Dataset<Row> result = spark.sql("SELECT myAverage(salary) as average_salary FROM employees");
result.show();
// +--------------+
// |average_salary|
// +--------------+
// |        3750.0|
// +--------------+
```

#### <a name="sql"></a>SQL

```sql
-- Compile and place UDAF MyAverage in a JAR file called `MyAverage.jar` in /tmp.
CREATE FUNCTION myAverage AS 'MyAverage' USING JAR '/tmp/MyAverage.jar';

SHOW USER FUNCTIONS;
+------------------+
|          function|
+------------------+
| default.myAverage|
+------------------+

CREATE TEMPORARY VIEW employees
USING org.apache.spark.sql.json
OPTIONS (
    path "examples/src/main/resources/employees.json"
);

SELECT * FROM employees;
+-------+------+
|   name|salary|
+-------+------+
|Michael|  3000|
|   Andy|  4500|
| Justin|  3500|
|  Berta|  4000|
+-------+------+

SELECT myAverage(salary) as average_salary FROM employees;
+--------------+
|average_salary|
+--------------+
|        3750.0|
+--------------+
```

## <a name="related-statements"></a>相关语句

* [标量用户定义函数 (UDF)](sql-ref-functions-udf-scalar.md)
* [与 Hive UDF、UDAF 和 UDTF 的集成](sql-ref-functions-udf-hive.md)