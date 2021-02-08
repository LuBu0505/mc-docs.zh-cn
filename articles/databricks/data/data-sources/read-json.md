---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/04/2020
title: JSON 文件 - Azure Databricks
description: 了解如何使用 Azure Databricks 读取 JSON 文件中的数据。
ms.openlocfilehash: 0761c8170c45fd9c83bad60aa21a26495b6d7433
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059879"
---
# <a name="json-file"></a><a id="json"> </a><a id="json-file"> </a>JSON 文件

可在[单行](#single-line-mode)或[多行](#multi-line-mode)模式下读取 JSON 文件。 在单行模式下，可将一个文件拆分成多个部分进行并行读取。 在多行模式下，文件将作为一个整体实体加载且无法拆分。

有关详细信息，请参阅 [JSON 文件](https://spark.apache.org/docs/latest/sql-data-sources-json.html)。

## <a name="options"></a>选项

有关支持的读取和写入选项，请参阅以下 Apache Spark 参考文章。

* 读取
  * [Python](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=dataframereader#pyspark.sql.DataFrameReader.json)
  * [Scala](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/sql/DataFrameReader.html#json(paths:String*):org.apache.spark.sql.DataFrame)
* 写入
  * [Python](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=dataframereader#pyspark.sql.DataFrameWriter.json)
  * [Scala](https://spark.apache.org/docs/latest/api/scala/org/apache/spark/sql/DataFrameWriter.html#json(path:String):Unit)

## <a name="examples"></a>示例

### <a name="single-line-mode"></a>单行模式

在此示例中，每行有一个 JSON 对象：

```json
{"string":"string1","int":1,"array":[1,2,3],"dict": {"key": "value1"}}
{"string":"string2","int":2,"array":[2,4,6],"dict": {"key": "value2"}}
{"string":"string3","int":3,"array":[3,6,9],"dict": {"key": "value3", "extra_key": "extra_value3"}}
```

若要读取 JSON 数据，请使用：

```scala
val df = spark.read.json("example.json")
```

Spark 会自动推断架构。

```scala
df.printSchema
```

```
root
 |-- array: array (nullable = true)
 |    |-- element: long (containsNull = true)
 |-- dict: struct (nullable = true)
 |    |-- extra_key: string (nullable = true)
 |    |-- key: string (nullable = true)
 |-- int: long (nullable = true)
 |-- string: string (nullable = true)
```

### <a name="multi-line-mode"></a>多行模式

此 JSON 对象占据多行：

```json
[
  {"string":"string1","int":1,"array":[1,2,3],"dict": {"key": "value1"}},
  {"string":"string2","int":2,"array":[2,4,6],"dict": {"key": "value2"}},
  {
    "string": "string3",
    "int": 3,
    "array": [
        3,
        6,
        9
    ],
    "dict": {
        "key": "value3",
        "extra_key": "extra_value3"
    }
  }
]
```

若要读取此对象，请启用多行模式：

#### <a name="sql"></a>SQL

```sql
CREATE TEMPORARY VIEW multiLineJsonTable
USING json
OPTIONS (path="/tmp/multi-line.json",multiline=true)
```

#### <a name="scala"></a>Scala

```scala
val mdf = spark.read.option("multiline", "true").json("/tmp/multi-line.json")
mdf.show(false)
```

### <a name="charset-auto-detection"></a><a id="charset-auto-detect"> </a><a id="charset-auto-detection"> </a>字符集自动检测

默认情况下，会自动检测输入文件的字符集。 可以使用 ``charset`` 选项显式指定字符集：

```python
spark.read.option("charset", "UTF-16BE").json("fileInUTF16.json")
```

下面是一些受支持的字符集：``UTF-8``、``UTF-16BE``、``UTF-16LE``、``UTF-16``、``UTF-32BE``、``UTF-32LE``、``UTF-32``。 若要查看 Oracle Java SE 支持的字符集的完整列表，请参阅[受支持的编码](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html)。

## <a name="notebook"></a>笔记本

以下笔记本演示单行和多行模式。

### <a name="read-json-files-notebook"></a>读取 JSON 文件笔记本

[获取笔记本](../../_static/notebooks/read-json-files.html)