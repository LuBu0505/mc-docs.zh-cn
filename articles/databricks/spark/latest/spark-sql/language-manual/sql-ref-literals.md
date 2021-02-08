---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 字面量 - Azure Databricks
description: 了解 Azure Databricks 支持的 SQL 语言构造中的 SQL 文本。
ms.openlocfilehash: 44188e3d61358a4860c2ad15fde1c66452e3097c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060648"
---
# <a name="literals"></a>文本

字面量（也称为常量）表示固定数据值。

## <a name="string-literals"></a>字符串文本

字符串字面量用于指定字符串值。

### <a name="syntax"></a>语法

```sql
'char [ ... ]' | "char [ ... ]"
```

### <a name="parameters"></a>参数

* **char**

  字符集中的一个字符。 使用 ``\`` 来转义特殊字符（例如 ``'`` 或 ``\``）。

### <a name="examples"></a>示例

```sql
SELECT 'Hello, World!' AS col;
```

```
+-------------+
|          col|
+-------------+
|Hello, World!|
+-------------+
```sql
SELECT "SPARK SQL" AS col;
```

```
+---------+
|      col|
+---------+
|Spark SQL|
+---------+
```

```
SELECT 'it\'s $10.' AS col;
```

```
+---------+
|      col|
+---------+
|It's $10.|
+---------+
```

## <a name="binary-literals"></a>二进制文本

二进制字面量用于指定字节序列值。

### <a name="syntax"></a>语法

```
X { 'num [ ... ]' | "num [ ... ]" }
```

### <a name="parameters"></a>参数

* **num**

  0 到 F 之间的任何十六进制数字。

### <a name="examples"></a>示例

```sql
SELECT X'123456' AS col;
+----------+
|       col|
+----------+
|[12 34 56]|
+----------+
```

## <a name="null-literals"></a>null 文本

null 字面量用于指定 null 值。

### <a name="syntax"></a>语法

```sql
NULL
```

### <a name="examples"></a>示例

```sql
SELECT NULL AS col;
+----+
| col|
+----+
|NULL|
+----+
```

## <a name="boolean-literals"></a>布尔值文字

布尔值字面量用于指定布尔值。

### <a name="syntax"></a>语法

```sql
TRUE | FALSE
```

### <a name="examples"></a>示例

```sql
SELECT TRUE AS col;
+----+
| col|
+----+
|true|
+----+
```

## <a name="numeric-literals"></a>数值文字

数字字面量用于指定固定值或浮点数。

### <a name="integer-literals"></a>整数文本

#### <a name="syntax"></a>语法

```sql
[ + | - ] digit [ ... ] [ L | S | Y ]
```

#### <a name="parameters"></a>参数

* **数字**：0 到 9 的任意数字。
* **L**：不区分大小写，指示 ``BIGINT``，一个 8 个字节的带符号整数。
* **S**：不区分大小写，指示 ``SMALLINT``，一个 2 个字节的带符号整数。
* **Y**：不区分大小写，指示 ``TINYINT``，一个 1 个字节的带符号整数。
* **默认值（无后缀）** ：指示 4 个字节的带符号整数。

#### <a name="examples"></a>示例

```sql
SELECT -2147483648 AS col;
+-----------+
|        col|
+-----------+
|-2147483648|
+-----------+

SELECT 9223372036854775807l AS col;
+-------------------+
|                col|
+-------------------+
|9223372036854775807|
+-------------------+

SELECT -32Y AS col;
+---+
|col|
+---+
|-32|
+---+

SELECT 482S AS col;
+---+
|col|
+---+
|482|
+---+
```

### <a name="fractional-literals"></a>小数型字面量

#### <a name="syntax"></a>语法

* 十进制字面量：

``decimal_digits { [ BD ] | [ exponent BD ] } | digit [ ... ] [ exponent ] BD``

* 双精度字面量：

``decimal_digits  { D | exponent [ D ] }  | digit [ ... ] { exponent [ D ] | [ exponent ] D }``

* 浮点数字面量：

``decimal_digits  { F | exponent [ F ] }  | digit [ ... ] { exponent [ F ] | [ exponent ] F }``

其中 ``decimal_digits`` 定义为 ``[ + | - ] { digit [ ... ] . [ digit [ ... ] ] | . digit [ ... ] }``

``exponent`` 定义为 ``E [ + | - ] digit [ ... ]``

#### <a name="parameters"></a>参数

* **数字**：0 到 9 的任意数字。
* **D**：不区分大小写，指示 ``DOUBLE``，一个 8 个字节的双精度浮点数。
* **F**：不区分大小写，指示 ``FLOAT``，一个 4 个字节的单精度浮点数。
* **BD**：不区分大小写，指示 ``DECIMAL``，其总位数为精度，小数点右边的位数为小数位数。

#### <a name="examples"></a>示例

```sql
SELECT 12.578 AS col;
+------+
|   col|
+------+
|12.578|
+------+

SELECT -0.1234567 AS col;
+----------+
|       col|
+----------+
|-0.1234567|
+----------+

SELECT -.1234567 AS col;
+----------+
|       col|
+----------+
|-0.1234567|
+----------+

SELECT 123. AS col;
+---+
|col|
+---+
|123|
+---+

SELECT 123.BD AS col;
+---+
|col|
+---+
|123|
+---+

SELECT 5E2 AS col;
+-----+
|  col|
+-----+
|500.0|
+-----+

SELECT 5D AS col;
+---+
|col|
+---+
|5.0|
+---+

SELECT -5BD AS col;
+---+
|col|
+---+
| -5|
+---+

SELECT 12.578e-2d AS col;
+-------+
|    col|
+-------+
|0.12578|
+-------+

SELECT -.1234567E+2BD AS col;
+---------+
|      col|
+---------+
|-12.34567|
+---------+

SELECT +3.e+3 AS col;
+------+
|   col|
+------+
|3000.0|
+------+

SELECT -3.E-3D AS col;
+------+
|   col|
+------+
|-0.003|
+------+
```

## <a name="datetime-literals"></a>日期/时间字面量

日期/时间字面量用于指定日期/时间值。

### <a name="date-literal"></a>日期字面量

#### <a name="syntax"></a>语法

```
DATE { 'yyyy' |
       'yyyy-[m]m' |
       'yyyy-[m]m-[d]d' |
       'yyyy-[m]m-[d]d[T]' }
```

> [!NOTE]
>
> 如果未指定月或日，则默认为 ``01``。

#### <a name="examples"></a>示例

```sql
SELECT DATE '1997' AS col;
+----------+
|       col|
+----------+
|1997-01-01|
+----------+

SELECT DATE '1997-01' AS col;
+----------+
|       col|
+----------+
|1997-01-01|
+----------+

SELECT DATE '2011-11-11' AS col;
+----------+
|       col|
+----------+
|2011-11-11|
+----------+
```

### <a name="timestamp-literals"></a>时间戳字面量

#### <a name="syntax"></a>语法

```
TIMESTAMP { 'yyyy' |
            'yyyy-[m]m' |
            'yyyy-[m]m-[d]d' |
            'yyyy-[m]m-[d]d ' |
            'yyyy-[m]m-[d]d[T][h]h[:]' |
            'yyyy-[m]m-[d]d[T][h]h:[m]m[:]' |
            'yyyy-[m]m-[d]d[T][h]h:[m]m:[s]s[.]' |
            'yyyy-[m]m-[d]d[T][h]h:[m]m:[s]s.[ms][ms][ms][us][us][us][zone_id]'}
```

> [!NOTE]
>
> 如果未指定小时、分钟或秒，则默认为 ``00``。

``zone_id`` 应为以下形式之一：

* Z - 祖鲁时区 UTC+0
* ``+|-[h]h:[m]m``
* 具有 UTC+、UTC-、GMT+、GMT-、UT+ 或 UT- 前缀的 ID，和具有以下格式的后缀：
  * ``+|-h[h]``
  * ``+|-hh[:]mm``
  * ``+|-hh:mm:ss``
  * ``+|-hhmmss``
* 格式为 ``area/city`` 的基于区域的区域 ID，如 ``Europe/Paris``

> [!NOTE]
>
> 如果未指定 ``zone_id``，则默认为会话本地时区（通过 ``spark.sql.session.timeZone`` 进行设置）。

#### <a name="examples"></a>示例

```sql
SELECT TIMESTAMP '1997-01-31 09:26:56.123' AS col;
+-----------------------+
|                    col|
+-----------------------+
|1997-01-31 09:26:56.123|
+-----------------------+

SELECT TIMESTAMP '1997-01-31 09:26:56.66666666UTC+08:00' AS col;
+--------------------------+
|                      col |
+--------------------------+
|1997-01-30 17:26:56.666666|
+--------------------------+

SELECT TIMESTAMP '1997-01' AS col;
+-------------------+
|                col|
+-------------------+
|1997-01-01 00:00:00|
+-------------------+
```

## <a name="interval-literals"></a>间隔字面量

间隔字面量用于指定固定时间段。

```sql
INTERVAL interval_value interval_unit [ interval_value interval_unit ... ] |
INTERVAL 'interval_value interval_unit [ interval_value interval_unit ... ]' |
INTERVAL interval_string_value interval_unit TO interval_unit
```

### <a name="parameters"></a>参数

* **interval_value**

  **语法：**

  [ + | - ] number_value | ‘[ + | - ] number_value’

* **interval_string_value**

  年月/日时间间隔字符串。

* **interval_unit**

  **语法：**

  YEAR[S] | MONTH[S] | WEEK[S] | DAY[S] | HOUR[S] | MINUTE[S] | SECOND[S] | MILLISECOND[S] | MICROSECOND[S]

### <a name="examples"></a>示例

```sql
SELECT INTERVAL 3 YEAR AS col;
+-------+
|    col|
+-------+
|3 years|
+-------+

SELECT INTERVAL -2 HOUR '3' MINUTE AS col;
+--------------------+
|                 col|
+--------------------+
|-1 hours -57 minutes|
+--------------------+

SELECT INTERVAL '1 YEAR 2 DAYS 3 HOURS';
+----------------------+
|                   col|
+----------------------+
|1 years 2 days 3 hours|
+----------------------+

SELECT INTERVAL 1 YEARS 2 MONTH 3 WEEK 4 DAYS 5 HOUR 6 MINUTES 7 SECOND 8
    MILLISECOND 9 MICROSECONDS AS col;
+-----------------------------------------------------------+
|                                                        col|
+-----------------------------------------------------------+
|1 years 2 months 25 days 5 hours 6 minutes 7.008009 seconds|
+-----------------------------------------------------------+

SELECT INTERVAL '2-3' YEAR TO MONTH AS col;
+----------------+
|             col|
+----------------+
|2 years 3 months|
+----------------+

SELECT INTERVAL '20 15:40:32.99899999' DAY TO SECOND AS col;
+---------------------------------------------+
|                                          col|
+---------------------------------------------+
|20 days 15 hours 40 minutes 32.998999 seconds|
+---------------------------------------------+
```