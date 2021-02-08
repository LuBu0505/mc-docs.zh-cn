---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 数据类型 - Azure Databricks
description: 了解 Azure Databricks 中支持的 SQL 语言构造中的 SQL 数据类型。
ms.openlocfilehash: 388ecc2a687533ac4ed5334f2f182a7f58ae98f5
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060823"
---
# <a name="data-types"></a>数据类型

## <a name="supported-data-types"></a>支持的数据类型

Apache Spark SQL 和 DataFrames 支持以下数据类型：

* 数字类型
  * ByteType：表示 1 个字节的带符号整数。 数字范围是从 ``-128`` 到 ``127``。
  * ShortType：表示 2 个字节的带符号整数。 数字范围是从 ``-32768`` 到 ``32767``。
  * IntegerType：表示 4 个字节的带符号整数。 数字范围是从 ``-2147483648`` 到 ``2147483647``。
  * LongType：表示 8 个字节的带符号整数。 数字范围是从 ``-9223372036854775808`` 到 ``9223372036854775807``。
  * FloatType：表示 4 个字节的单精度浮点数。
  * DoubleType：表示 8 个字节的双精度浮点数。
  * DecimalType：表示任意精度的带符号十进制数字。 由 ``java.math.BigDecimal`` 在内部提供支持。 ``BigDecimal`` 由一个任意精度的非标度整数值和一个 32 位整数标度构成。
* 字符串类型：
  * StringType：表示字符串值。
* 二进制类型：
  * BinaryType：表示字节序列值。
* 布尔类型：
  * BooleanType：表示布尔值。
* 日期/时间类型
  * TimestampType：表示由字段 year、month、day、hour、minute 和 second 的值构成的值，使用会话本地时区。 时间戳值表示绝对时间点。
  * DateType：表示由字段 year、month 和 day 的值构成的值，不包含时区。
* 复杂类型
  * ArrayType(elementType, containsNull)：表示由 elementType 类型的元素序列构成的值。 ``containsNull`` 指示 ArrayType 值中的元素是否可以具有 ``null`` 值。
  * MapType(keyType, valueType, valueContainsNull)：表示由一组键值对构成的值。 键的数据类型由 keyType 描述，而值的数据类型由 valueType 描述。 对于 MapType 值，不允许键具有 ``null`` 值。 ``valueContainsNull`` 指示 MapType 值的值是否可以具有 ``null`` 值。
  * StructType(fields)：表示多个值，其结构通过一系列 StructFields（字段）来描述。
    * StructField(name, dataType, nullable)：表示 StructType 内的字段。 字段的名称由 ``name`` 指示。 字段的数据类型由 dataType 指示。 ``nullable`` 指示这些字段的值是否可以具有 ``null`` 值。

## <a name="language-mappings"></a>语言映射

### <a name="scala"></a>Scala

Spark SQL 数据类型是在包 ``org.apache.spark.sql.types`` 中定义的。 可以通过导入此包来访问这些数据类型：

```scala
import org.apache.spark.sql.types._
```

| 数据类型     | 值类型                                                                                                       | 用于访问或创建数据类型的 API                                                                                  |
|---------------|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| ByteType      | Byte                                                                                                             | ByteType                                                                                                           |
| ShortType     | Short                                                                                                            | ShortType                                                                                                          |
| IntegerType   | int                                                                                                              | IntegerType                                                                                                        |
| LongType      | Long                                                                                                             | LongType                                                                                                           |
| FloatType     | Float                                                                                                            | FloatType                                                                                                          |
| DoubleType    | Double                                                                                                           | DoubleType                                                                                                         |
| DecimalType   | java.math.BigDecimal                                                                                             | DecimalType                                                                                                        |
| StringType    | 字符串                                                                                                           | StringType                                                                                                         |
| BinaryType    | Array[Byte]                                                                                                      | BinaryType                                                                                                         |
| BooleanType   | 布尔值                                                                                                          | BooleanType                                                                                                        |
| TimestampType | java.sql.Timestamp                                                                                               | TimestampType                                                                                                      |
| DateType      | java.sql.Date                                                                                                    | DateType                                                                                                           |
| ArrayType     | scala.collection.Seq                                                                                             | ArrayType(elementType, [containsNull])，注意：containsNull 的默认值为 true。                        |
| MapType       | scala.collection.Map                                                                                             | MapType(keyType, valueType, valueContainsNull)，注意：valueContainsNull 的默认值为 true。         |
| StructType    | org.apache.spark.sql.Row                                                                                         | StructType(fields)，注意：字段是一系列 StructFields。 此外，不允许使用名称相同的两个字段。 |
| StructField   | 此字段的数据类型的值类型（例如，数据类型为 IntegerType 的 StructField 的 Int） | StructField(name, dataType, nullable)，注意：nullable 的默认值为 true。                           |

### <a name="java"></a>Java

Spark SQL 数据类型是在包 ``org.apache.spark.sql.types`` 中定义的。 若要访问或创建数据类型，请使用 ``org.apache.spark.sql.types.DataTypes`` 中提供的工厂方法。

| 数据类型     | 值类型                                                                                                        | 用于访问或创建数据类型的 API                                                                                                                             |
|---------------|-------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ByteType      | byte 或 Byte                                                                                                      | DataTypes.ByteType                                                                                                                                            |
| ShortType     | short 或 Short                                                                                                    | DataTypes.ShortType                                                                                                                                           |
| IntegerType   | int 或 Integer                                                                                                    | DataTypes.IntegerType                                                                                                                                         |
| LongType      | long 或 Long                                                                                                      | DataTypes.LongType                                                                                                                                            |
| FloatType     | float 或 Float                                                                                                    | DataTypes.FloatType                                                                                                                                           |
| DoubleType    | double 或 Double                                                                                                  | DataTypes.DoubleType                                                                                                                                          |
| DecimalType   | java.math.BigDecimal                                                                                              | DataTypes.createDecimalType() DataTypes.createDecimalType(precision, scale)。                                                                                  |
| StringType    | 字符串                                                                                                            | DataTypes.StringType                                                                                                                                          |
| BinaryType    | byte[]                                                                                                            | DataTypes.BinaryType                                                                                                                                          |
| BooleanType   | boolean 或 Boolean                                                                                                | DataTypes.BooleanType                                                                                                                                         |
| TimestampType | java.sql.Timestamp                                                                                                | DataTypes.TimestampType                                                                                                                                       |
| DateType      | java.sql.Date                                                                                                     | DataTypes.DateType                                                                                                                                            |
| ArrayType     | java.util.List                                                                                                    | DataTypes.createArrayType(elementType)，注意：containsNull 的值为 true。 DataTypes.createArrayType(elementType, containsNull)。                     |
| MapType       | java.util.Map                                                                                                     | DataTypes.createMapType(keyType, valueType)，注意：valueContainsNull 的值为 true。 DataTypes.createMapType(keyType, valueType, valueContainsNull) |
| StructType    | org.apache.spark.sql.Row                                                                                          | DataTypes.createStructType(fields)，注意：字段是 StructFields 的列表或数组。此外，不允许使用名称相同的两个字段。                |
| StructField   | 此字段的数据类型的值类型（例如，数据类型为 IntegerType 的 StructField 的 int） | DataTypes.createStructField(name, dataType, nullable)                                                                                                         |

### <a name="python"></a>Python

Spark SQL 数据类型是在包 ``pyspark.sql.types`` 中定义的。 可以通过导入此包来访问这些数据类型：

```python
from pyspark.sql.types import *
```

| 数据类型     | 值类型                                                                                                                                                                                                                                        | 用于访问或创建数据类型的 API                                                                                  |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| ByteType      | int 或 long，注意：数字在运行时会转换为 1 个字节的带符号整数。 请确保数字在 -128 到 127 的范围内。                                                                                         | ByteType()                                                                                                         |
| ShortType     | int 或 long，注意：数字在运行时会转换为 2 个字节的带符号整数。 请确保数字在 -32768 到 32767 的范围内。                                                                                     | ShortType()                                                                                                        |
| IntegerType   | int 或 long                                                                                                                                                                                                                                       | IntegerType()                                                                                                      |
| LongType      | long，注意：数字在运行时会转换为 8 个字节的带符号整数。 请确保数字在 -9223372036854775808 到 9223372036854775807 的范围内。否则，数据将转换为 decimal.Decimal 并使用 DecimalType。 | LongType()                                                                                                         |
| FloatType     | float，注意：数字在运行时会转换为 4 个字节的单精度浮点。                                                                                                                                               | FloatType()                                                                                                        |
| DoubleType    | FLOAT                                                                                                                                                                                                                                             | DoubleType()                                                                                                       |
| DecimalType   | decimal.Decimal                                                                                                                                                                                                                                   | DecimalType()                                                                                                      |
| StringType    | string                                                                                                                                                                                                                                            | StringType()                                                                                                       |
| BinaryType    | bytearray                                                                                                                                                                                                                                         | BinaryType()                                                                                                       |
| BooleanType   | bool                                                                                                                                                                                                                                              | BooleanType()                                                                                                      |
| TimestampType | datetime.datetime                                                                                                                                                                                                                                 | TimestampType()                                                                                                    |
| DateType      | datetime.date                                                                                                                                                                                                                                     | DateType()                                                                                                         |
| ArrayType     | list、tuple 或 array                                                                                                                                                                                                                             | ArrayType(elementType, [containsNull])，注意：containsNull 的默认值为 True。                         |
| MapType       | dict                                                                                                                                                                                                                                              | MapType(keyType, valueType, [valueContainsNull])，注意：valueContainsNull 的默认值为 True。          |
| StructType    | list 或 tuple                                                                                                                                                                                                                                     | StructType(fields)，注意：字段是一系列 StructFields。 此外，不允许使用名称相同的两个字段。 |
| StructField   | 此字段的数据类型的值类型（例如，数据类型为 IntegerType 的 StructField 的 Int）                                                                                                                                 | StructField(name, dataType, nullable)，注意：nullable 的默认值为 True。                           |

### <a name="r"></a>R

| 数据类型     | 值类型                                                                                                                                                                                                                                               | 用于访问或创建数据类型的 API                                                                                                                       |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| ByteType      | integer，注意：数字在运行时会转换为 1 个字节的带符号整数。  请确保数字在 -128 到 127 的范围内。                                                                                                  | “byte”                                                                                                                                                  |
| ShortType     | integer，注意：数字在运行时会转换为 2 个字节的带符号整数。  请确保数字在 -32768 到 32767 的范围内。                                                                                              | “short”                                                                                                                                                 |
| IntegerType   | integer                                                                                                                                                                                                                                                  | “integer”                                                                                                                                               |
| LongType      | integer，注意：数字在运行时会转换为 8 个字节的带符号整数。  请确保数字在 -9223372036854775808 到 9223372036854775807 的范围内。  否则，数据将转换为 decimal.Decimal 并使用 DecimalType。 | “long”                                                                                                                                                  |
| FloatType     | numeric，注意：数字在运行时会转换为 4 个字节的单精度浮点。                                                                                                                                                   | “float”                                                                                                                                                 |
| DoubleType    | numeric                                                                                                                                                                                                                                                  | “double”                                                                                                                                                |
| DecimalType   | 不支持                                                                                                                                                                                                                                            | 不支持                                                                                                                                           |
| StringType    | character                                                                                                                                                                                                                                                | "string"                                                                                                                                                |
| BinaryType    | raw                                                                                                                                                                                                                                                      | “binary”                                                                                                                                                |
| BooleanType   | 逻辑                                                                                                                                                                                                                                                  | “bool”                                                                                                                                                  |
| TimestampType | POSIXct                                                                                                                                                                                                                                                  | “timestamp”                                                                                                                                             |
| DateType      | 日期                                                                                                                                                                                                                                                     | “date”                                                                                                                                                  |
| ArrayType     | vector 或 list                                                                                                                                                                                                                                           | list(type=”array”, elementType=elementType, containsNull=[containsNull])，注意：containsNull 的默认值为 TRUE。                           |
| MapType       | 环境                                                                                                                                                                                                                                              | list(type=”map”, keyType=keyType, valueType=valueType, valueContainsNull=[valueContainsNull])，注意：valueContainsNull 的默认值为 TRUE。 |
| StructType    | named list                                                                                                                                                                                                                                               | list(type=”struct”, fields=fields)，注意：字段是一系列 StructFields。 此外，不允许使用名称相同的两个字段。                      |
| StructField   | 此字段的数据类型的值类型（例如，数据类型为 IntegerType 的 StructField 的 integer）                                                                                                                                    | list(name=name, type=dataType, nullable=[nullable])，注意：nullable 的默认值为 TRUE。                                                    |

### <a name="sql"></a>SQL

下表显示了每种数据类型的 Spark SQL 分析器中使用的类型名称和别名。

| 数据类型            | SQL 名称                                                        |
|----------------------|-----------------------------------------------------------------|
| BooleanType          | BOOLEAN                                                         |
| ByteType             | BYTE, TINYINT                                                   |
| ShortType            | SHORT, SMALLINT                                                 |
| IntegerType          | INT、INTEGER                                                    |
| LongType             | LONG, BIGINT                                                    |
| FloatType            | FLOAT、REAL                                                     |
| DoubleType           | DOUBLE                                                          |
| DateType             | DATE                                                            |
| TimestampType        | TIMESTAMP                                                       |
| StringType           | STRING                                                          |
| BinaryType           | BINARY                                                          |
| DecimalType          | DECIMAL, DEC, NUMERIC                                           |
| CalendarIntervalType | INTERVAL                                                        |
| ArrayType            | ARRAY<element_type>                                             |
| StructType           | STRUCT<field1_name: field1_type, field2_name: field2_type, …>   |
| MapType              | MAP<key_type, value_type>                                       |

## <a name="special-floating-point-values"></a>特殊浮点值

Spark SQL 支持多个特殊的浮点值（不区分大小写）：

* Inf、+Inf、Infinity、+Infinity：正无穷大
  * FloatType：等效于 Scala ``Float.PositiveInfinity``。
  * DoubleType：等效于 Scala ``Double.PositiveInfinity``。
* -Inf、-Infinity：负无穷大
  * FloatType：等效于 Scala ``Float.NegativeInfinity``。
  * DoubleType：等效于 Scala ``Double.NegativeInfinity``。
* NaN：非数值
  * FloatType：等效于 Scala ``Float.NaN``。
  * DoubleType：等效于 Scala ``Double.NaN``。

### <a name="positive-and-negative-infinity-semantics"></a>正负无穷大语义

正负无穷大有特殊的处理方式。 它们具有以下语义：

* 正无穷大乘以任何正值都会返回正无穷大。
* 负无穷大乘以任何正值都会返回负无穷大。
* 正无穷大乘以任何负值都会返回负无穷大。
* 负无穷大乘以任何负值都会返回正无穷大。
* 正/负无穷大乘以 0 返回 NaN。
* 正/负无穷大等于自身。
* 在聚合中，所有正无穷大值都分组在一起。 同样，所有负无穷大值都分组在一起。
* 正无穷大和负无穷大被视为联接键中的正常值。
* 正无穷大小于 NaN，并且大于任何其他值。
* 负无穷大小于任何其他值。

### <a name="nan-semantics"></a>NaN 语义

在处理与标准浮点语义不完全匹配的 ``float`` 或 ``double`` 类型时，NaN 具有特殊处理方式。 尤其是在下列情况下：

* NaN = NaN 返回 true。
* 在聚合中，所有 NaN 值都分组在一起。
* NaN 被视为联接键中的正常值。
* NaN 值按升序排列后为最后一个值，大于任何其他数值。

### <a name="examples"></a>示例

```sql
SELECT double('infinity') AS col;
+--------+
|     col|
+--------+
|Infinity|
+--------+

SELECT float('-inf') AS col;
+---------+
|      col|
+---------+
|-Infinity|
+---------+

SELECT float('NaN') AS col;
+---+
|col|
+---+
|NaN|
+---+

SELECT double('infinity') * 0 AS col;
+---+
|col|
+---+
|NaN|
+---+

SELECT double('-infinity') * (-1234567) AS col;
+--------+
|     col|
+--------+
|Infinity|
+--------+

SELECT double('infinity') < double('NaN') AS col;
+----+
| col|
+----+
|true|
+----+

SELECT double('NaN') = double('NaN') AS col;
+----+
| col|
+----+
|true|
+----+

SELECT double('inf') = double('infinity') AS col;
+----+
| col|
+----+
|true|
+----+

CREATE TABLE test (c1 int, c2 double);
INSERT INTO test VALUES (1, double('infinity'));
INSERT INTO test VALUES (2, double('infinity'));
INSERT INTO test VALUES (3, double('inf'));
INSERT INTO test VALUES (4, double('-inf'));
INSERT INTO test VALUES (5, double('NaN'));
INSERT INTO test VALUES (6, double('NaN'));
INSERT INTO test VALUES (7, double('-infinity'));
SELECT COUNT(*), c2 FROM test GROUP BY c2;
+---------+---------+
| count(1)|       c2|
+---------+---------+
|        2|      NaN|
|        2|-Infinity|
|        3| Infinity|
+---------+---------+
```