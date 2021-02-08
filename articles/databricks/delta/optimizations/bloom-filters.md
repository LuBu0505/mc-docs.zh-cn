---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 08/11/2020
title: Bloom 筛选器索引 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Bloom 筛选器索引。
ms.openlocfilehash: 72c6e8881d717bcbc1c41a4c66db131d658bdfac
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060082"
---
# <a name="bloom-filter-indexes"></a>Bloom 筛选器索引

[Bloom 筛选器](https://en.wikipedia.org/wiki/Bloom_filter)索引是一种节省空间的数据结构，可跳过选定列中的数据（特别对于包含任意文本的字段）。 Bloom 筛选器的工作方式是：声明数据肯定不在文件中或可能在文件中，并定义误报率 (FPP) 。

Azure Databricks 支持文件级 Bloom 筛选器；每个数据文件都可关联一个 Bloom 筛选器索引文件。 读取文件前，Azure Databricks 会检查索引文件，并且仅在索引指示该文件可能与数据筛选器匹配时才会读取该文件。 如果索引不存在或未为查询的列定义 Bloom 筛选器，那么 Azure Databricks 会一直读取数据文件。

Bloom 筛选器的大小由两者决定：为其创建了 Bloom 筛选器的集中的数字元素，以及必需的 FPP。 FPP 越低，每个元素使用的位数越多，它的准确度就越高，但代价是磁盘空间占用更多、下载速度更慢。 例如，FPP 为 10% 时，每个元素需要 5 位。

Bloom 筛选器索引是包含单个行的未压缩的 Parquet 文件。 索引存储在与数据文件相关的 `_delta_index` 子目录中，且使用与后缀为 `index.v1.parquet` 的数据文件相同的名称。 例如，数据文件 `dbfs:/db1/data.0001.parquet.snappy` 的索引将命名为 `dbfs:/db1/_delta_index/data.0001.parquet.snappy.index.v1.parquet`。

Bloom 筛选器支持具有以下（输入）数据类型的列：`byte`、`short`、`int`、`long`、`float`、`double`、`date`、`timestamp` 和 `string`。 不会向 Bloom 筛选器添加 NULL 值，因此所有与 NULL 相关的筛选器都需要读取数据文件。 Azure Databricks 支持以下数据源筛选器：`and`、`or`、`in`、`equals` 和 `equalsnullsafe`。 嵌套列不支持 Bloom 筛选器。

## <a name="configuration"></a>配置

默认启用 Bloom 筛选器。 若要禁用 Bloom 筛选器，请将会话级别 `spark.databricks.io.skipping.bloomFilter.enabled` 配置设置为 `false`。

## <a name="create-a-bloom-filter-index"></a>创建 Bloom 筛选器索引

若要在表格中为新数据或重写数据的所有列或部分列创建 Bloom 筛选器索引，请使用 `CREATE BLOOMFILTER INDEX` DDL 语句。 例如，以下语句在列 `sha` 中创建 Bloom 筛选器索引，并在列中将 FPP `0.1` 和 `50,000,000` 作为不同的项。

```sql
CREATE BLOOMFILTER INDEX
ON TABLE bloom_test
FOR COLUMNS(sha OPTIONS (fpp=0.1, numItems=50000000))
```

* Databricks Runtime 7.x：[CREATE BLOOM FILTER INDEX（Azure Databricks 上的 Delta Lake）](../../spark/latest/spark-sql/language-manual/delta-create-bloomfilter-index.md)
* Databricks Runtime 5.5 LTS 和 6.x：[创建布隆筛选器索引（Azure Databricks 上的 Delta Lake）](../../spark/2.x/spark-sql/language-manual/create-bloomfilter-index.md)

## <a name="drop-a-bloom-filter-index"></a>删除 Bloom 筛选器索引

若要从表或表的一组列中删除所有 Bloom 筛选器，可使用 `DROP BLOOMFILTER INDEX` DDL 语句。 例如： 。

```sql
DROP BLOOMFILTER INDEX ON TABLE bloom_test FOR COLUMNS(sha);
```

* Databricks Runtime 7.x：[DROP BLOOM FILTER INDEX（Azure Databricks 上的 Delta Lake）](../../spark/latest/spark-sql/language-manual/delta-drop-bloomfilter-index.md)
* Databricks Runtime 5.5 LTS 和 6.x：[删除 Bloom 筛选器索引（Azure Databricks 上的 Delta Lake）](../../spark/2.x/spark-sql/language-manual/drop-bloomfilter-index.md)

## <a name="notebook"></a>笔记本

以下笔记本演示了定义 Bloom 筛选器索引如何加速“大海捞针式”查询。

### <a name="bloom-filter-demo-notebook"></a>Bloom 筛选器演示笔记本

[获取笔记本](../../_static/notebooks/delta/bloom-filter-index-demo.html)