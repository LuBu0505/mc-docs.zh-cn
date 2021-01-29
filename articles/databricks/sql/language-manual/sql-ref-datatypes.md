---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 数据类型 - Azure Databricks
description: 了解 SQL 数据类型。
ms.openlocfilehash: cdf64f9e57f65fc959ccaf666f3582626cd219c9
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692317"
---
# <a name="data-types"></a>数据类型

## <a name="supported-data-types"></a>支持的数据类型

SQL Analytics 支持以下数据类型：

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

## <a name="language-mapping"></a>语言映射

下表显示了每种数据类型的 SQL Analytics 分析器中使用的类型名称和别名。

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

SQL Analytics 支持多个特殊的浮点值（不区分大小写）：

* Inf、+Inf、Infinity、+Infinity：正无穷大
* -Inf、-Infinity：负无穷大
* NaN：非数值

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