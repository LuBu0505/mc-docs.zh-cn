---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/05/2020
title: XML 文件 - Azure Databricks
description: 了解如何使用 Azure Databricks 读取 XML 文件。
ms.openlocfilehash: d15f9b5f32e9b32670e960d26227e60f83494887
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060915"
---
# <a name="xml-file"></a>XML 文件

本文介绍如何读取和写入 XML 文件作为 Apache Spark 数据源。

## <a name="requirements"></a>要求

1. 将 ``spark-xml`` 库创建为 [Maven 库](../../libraries/workspace-libraries.md#maven-libraries)。 针对 Maven 坐标，请指定：
   * Databricks Runtime 7.x：``com.databricks:spark-xml_2.12:<release>``
   * Databricks Runtime 5.5 LTS 和 6.x：``com.databricks:spark-xml_2.11:<release>``

   有关最新版 ``<release>``，请参阅 ``spark-xml`` [版本](https://github.com/databricks/spark-xml/releases)。

2. 在群集上[安装库](../../libraries/cluster-libraries.md#install-libraries)。

## <a name="example"></a>示例

本部分中的示例使用 [books](https://github.com/databricks/spark-xml/raw/master/src/test/resources/books.xml) XML 文件。

1. 检索 books XML 文件：

   ```bash
   $ wget https://github.com/databricks/spark-xml/raw/master/src/test/resources/books.xml
   ```

2. 将文件上传到 [DBFS](../databricks-file-system.md#user-interface)。

### <a name="read-and-write-xml-data"></a>读取和写入 XML 数据

#### <a name="sql"></a>SQL

```sql
/*Infer schema*/

CREATE TABLE books
USING xml
OPTIONS (path "dbfs:/books.xml", rowTag "book")

/*Specify column names and types*/

CREATE TABLE books (author string, description string, genre string, _id string, price double, publish_date string, title string)
USING xml
OPTIONS (path "dbfs:/books.xml", rowTag "book")
```

#### <a name="scala"></a>Scala

```scala
// Infer schema

import com.databricks.spark.xml._ // Add the DataFrame.read.xml() method

val df = spark.read
  .option("rowTag", "book")
  .xml("dbfs:/books.xml")

val selectedData = df.select("author", "_id")
selectedData.write
  .option("rootTag", "books")
  .option("rowTag", "book")
  .xml("dbfs:/newbooks.xml")

// Specify schema

import org.apache.spark.sql.types.{StructType, StructField, StringType, DoubleType}

val customSchema = StructType(Array(
  StructField("_id", StringType, nullable = true),
  StructField("author", StringType, nullable = true),
  StructField("description", StringType, nullable = true),
  StructField("genre", StringType, nullable = true),
  StructField("price", DoubleType, nullable = true),
  StructField("publish_date", StringType, nullable = true),
  StructField("title", StringType, nullable = true)))

val df = spark.read
  .option("rowTag", "book")
  .schema(customSchema)
  .xml("books.xml")

val selectedData = df.select("author", "_id")
selectedData.write
  .option("rootTag", "books")
  .option("rowTag", "book")
  .xml("dbfs:/newbooks.xml")
```

#### <a name="r"></a>R

```r
# Infer schema

library(SparkR)

sparkR.session("local[4]", sparkPackages = c("com.databricks:spark-xml_2.12:<release>"))

df <- read.df("dbfs:/books.xml", source = "xml", rowTag = "book")

# Default `rootTag` and `rowTag`
write.df(df, "dbfs:/newbooks.xml", "xml")

# Specify schema

customSchema <- structType(
  structField("_id", "string"),
  structField("author", "string"),
  structField("description", "string"),
  structField("genre", "string"),
  structField("price", "double"),
  structField("publish_date", "string"),
  structField("title", "string"))

df <- read.df("dbfs:/books.xml", source = "xml", schema = customSchema, rowTag = "book")

# In this case, `rootTag` is set to "ROWS" and `rowTag` is set to "ROW".
write.df(df, "dbfs:/newbooks.xml", "xml", "overwrite")
```

## <a name="options"></a>选项

* 读取
  * ``path``：XML 文件的位置。 接受标准 Hadoop 通配表达式。
  * ``rowTag``：该行标记将视为行。 例如，在此 XML ``<books><book><book>...</books>`` 中，该值为 ``book``。 默认值为 ``ROW``。
  * ``samplingRatio``：推理架构的采样率 (0.0-1)。 默认值为 1。 可用类型为 ``StructType``、``ArrayType``、``StringType``、``LongType``、``DoubleType``、``BooleanType``、``TimestampType`` 和 ``NullType``，除非提供架构。
  * ``excludeAttribute``：是否排除元素中的属性。 默认值为 false。
  * ``nullValue``：该值将视为 ``null`` 值。 默认值为 ``""``。
  * ``mode``：用于处理损坏记录的模式。 默认值为 ``PERMISSIVE``。
    * ``PERMISSIVE``:
      * 遇到损坏的记录时，会将所有字段设置为 ``null``，并将格式错误的字符串放入 ``columnNameOfCorruptRecord`` 配置的新字段中。
      * 遇到数据类型错误的字段时，会将有问题的字段设置为 ``null``。
    * ``DROPMALFORMED``：忽略损坏的记录。
    * ``FAILFAST``：检测到损坏的记录时引发异常。
  * ``inferSchema``：如果为 ``true``，则尝试为生成的每个 DataFrame 列推理一个相应的类型，例如布尔、数字或日期类型。 如果为 ``false``，则生成的所有列均为字符串类型。 默认值为 ``true``。
  * ``columnNameOfCorruptRecord``：存储格式错误的字符串的新字段的名称。 默认值为 ``_corrupt_record``。
  * ``attributePrefix``：属性的前缀，用于区分属性和元素。 这是字段名称的前缀。 默认值为 ``_``。
  * ``valueTag``：元素中没有子元素但有属性时用于值的标记。 默认值为 ``_VALUE``。
  * ``charset``：默认为 ``UTF-8``，但可设置为其他有效字符集名称。
  * ``ignoreSurroundingSpaces``：是否应跳过值周围的空格。 默认值为 false。
  * ``rowValidationXSDPath``：XSD 文件的路径，用于验证每行的 XML。 未能通过验证的行视为上述解析错误。 XSD 不会以其他方式影响提供或推理的架构。  如果群集中的执行程序上未显示上述本地路径，则应使用 [SparkContext.addFile](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.SparkContext@addFile(path:String):Unit) 将 XSD 及其依赖的其他任何路径添加到 Spark 执行程序。 在这种情况下，若要使用本地 XSD ``/foo/bar.xsd``，请调用 ``addFile("/foo/bar.xsd")`` 并将 ``"bar.xsd"`` 作为 ``rowValidationXSDPath`` 传递。
* 写入
  * ``path``：写入文件的位置。
  * ``rowTag``：该行标记将视为行。 例如，在此 XML ``<books><book><book>...</books>`` 中，该值为 ``book``。 默认值为 ``ROW``。
  * ``rootTag``：该根标记将视为根。 例如，在此 XML ``<books><book><book>...</books>`` 中，该值为 ``books``。 默认值为 ``ROWS``。
  * ``nullValue``：该值将写入 ``null`` 值。 默认值为字符串 ``"null"``。 当为 ``"null"`` 时，它不会为字段写入属性和元素。
  * ``attributePrefix``：属性的前缀，用于区分属性和元素。 这是字段名称的前缀。 默认值为 ``_``。
  * ``valueTag``：元素中没有子元素但有属性时用于值的标记。 默认值为 ``_VALUE``。
  * ``compression``：保存到文件时使用的压缩编解码器。 应是实现 ``org.apache.hadoop.io.compress.CompressionCodec`` 的类的完全限定名称，或是不区分大小写的短名称之一（``bzip2``、``gzip``、``lz4`` 和 ``snappy``）。 默认为不压缩。

支持使用短名称；可以使用 ``xml`` 代替 ``com.databricks.spark.xml``。

## <a name="xsd-support"></a>XSD 支持

可以使用 ``rowValidationXSDPath`` 针对 XSD 架构验证各行。

可以使用实用工具 ``com.databricks.spark.xml.util.XSDToSchema`` 从某些 XSD 文件中提取 Spark DataFrame 架构。 它仅支持简单类型、复杂类型和序列类型，仅支持基本 XSD 功能，且处于试验阶段。

```scala
import com.databricks.spark.xml.util.XSDToSchema
import java.nio.file.Paths

val schema = XSDToSchema.read(Paths.get("/path/to/your.xsd"))
val df = spark.read.schema(schema)....xml(...)
```

## <a name="parse-nested-xml"></a>分析嵌套 XML

尽管 ``from_xml`` 方法主要用于将 XML 文件转换为 DataFrame，但也可以使用它在现有的 DataFrame 中解析字符串值列中的 XML，然后将其添加为新列，并将解析结果作为结构：

```scala
import com.databricks.spark.xml.functions.from_xml
import com.databricks.spark.xml.schema_of_xml
import spark.implicits._

val df = ... /// DataFrame with XML in column 'payload'
val payloadSchema = schema_of_xml(df.select("payload").as[String])
val parsed = df.withColumn("parsed", from_xml($"payload", payloadSchema))
```

> [!NOTE]
>
> * ``mode``:
>   * 如果设置为默认值 ``PERMISSIVE``，则解析模式默认为 ``DROPMALFORMED``。 如果在 ``from_xml`` 的架构中加入与 ``columnNameOfCorruptRecord`` 匹配的列，则 ``PERMISSIVE`` 模式会将格式错误的记录输出到生成的结构中的该列。
>   * 如果设置为 ``DROPMALFORMED``，则未正确解析的 XML 值将为列生成 ``null`` 值。 不删除任何行。
> * ``from_xml`` 将包含 XML 的字符串数组转换为已解析结构的数组。 请改用 ``schema_of_xml_array``。
> * ``from_xml_string`` 是在 UDF 中使用的替代方法，可直接对字符串（而不是列）进行操作。

## <a name="conversion-rules"></a>转换规则

由于 DataFrame 和 XML 之间存在结构差异，因此对于从 XML 数据转换为 DataFrame 以及从 DataFrame 转换为 XML 数据来说，有一些转换规则。 可以使用选项 ``excludeAttribute`` 禁止处理某些属性。

### <a name="convert-xml-to-dataframe"></a>将 XML 转换为 DataFrame

* 属性：属性会转换为具有 ``attributePrefix`` 选项中指定的前缀的字段。 如果 ``attributePrefix`` 为 ``_``，则以下文档

  ```xml
  <one myOneAttrib="AAAA">
      <two>two</two>
      <three>three</three>
  </one>
  ```

  将生成以下架构：

  ```
  root
   |-- _myOneAttrib: string (nullable = true)
   |-- two: string (nullable = true)
   |-- three: string (nullable = true)
  ```

* 如果元素有属性但没有子元素，则属性值将位于 ``valueTag`` 选项中指定的独立字段。 如果 ``valueTag`` 为 ``_VALUE``，则以下文档

  ```xml
  <one>
      <two myTwoAttrib="BBBBB">two</two>
      <three>three</three>
  </one>
  ```

  将生成以下架构：

  ```
  root
   |-- two: struct (nullable = true)
   |    |-- _VALUE: string (nullable = true)
   |    |-- _myTwoAttrib: string (nullable = true)
   |-- three: string (nullable = true)
  ```

### <a name="convert-dataframe-to-xml"></a>将 DataFrame 转换为 XML

从具有 ``ArrayType`` 字段且其元素为 ``ArrayType`` 的 DataFrame 写入 XML 文件时，将为该元素提供额外的嵌套字段。 读写 XML 数据时不会发生这种情况，但写入从其他源读取的 DataFrame 时会。 因此，读写和写读 XML 文件具有同一结构，但写入从其他源读取的 DataFrame 可能具有另一结构。

如果 DataFrame 具有以下架构：

```
 |-- a: array (nullable = true)
 |    |-- element: array (containsNull = true)
 |    |    |-- element: string (containsNull = true)
```

和以下数据：

```
+------------------------------------+
|                                   a|
+------------------------------------+
|[WrappedArray(aa), WrappedArray(bb)]|
+------------------------------------+
```

则将生成以下 XML 文件：

```xml
<a>
  <item>aa</item>
</a>
<a>
  <item>bb</item>
</a>
```