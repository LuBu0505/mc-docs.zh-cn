---
title: 使用无服务器 SQL 池查询 JSON 文件
description: 此部分介绍如何使用 Azure Synapse Analytics 中的无服务器 SQL 池读取 JSON 文件。
services: synapse-analytics
author: WenJason
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
origin.date: 05/20/2020
ms.date: 03/22/2021
ms.author: v-jay
ms.reviewer: jrasnick
ms.openlocfilehash: 3f6a59bce73af6ace5cea7089496b9e4983ad651
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766179"
---
# <a name="query-json-files-using-serverless-sql-pool-in-azure-synapse-analytics"></a>使用 Azure Synapse Analytics 中的无服务器 SQL 池查询 JSON 文件

在本文中，你将了解如何在 Azure Synapse Analytics 中使用无服务器 SQL 池编写查询。 使用 [OPENROWSET](develop-openrowset.md) 进行查询的目标是读取 JSON 文件。 
- 标准 JSON 文件，其中多个 JSON 文档以 JSON 数组的形式存储。
- 行分隔的 JSON 文件，其中 JSON 文档用换行符分隔。 这些类型的文件的常见扩展为 `jsonl`、`ldjson` 和 `ndjson`。

## <a name="read-json-documents"></a>读取 JSON 文档

要查看 JSON 文件的内容，最简单的方法是提供 `OPENROWSET` 函数的文件 URL，指定 csv `FORMAT`，并为 `fieldterminator` 和 `fieldquote` 设置值 `0x0b`。 如果仅需读取行分隔的 JSON 文件，这样就可以了。 如果有经典 JSON 文件，则需要为 `rowterminator` 设置值 `0x0b`。 `OPENROWSET` 函数将分析 JSON 并采用以下格式返回每个文档：

| 文档 |
| --- |
|{"date_rep":"2020-07-24","day":24,"month":7,"year":2020,"cases":3,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-25","day":25,"month":7,"year":2020,"cases":7,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-26","day":26,"month":7,"year":2020,"cases":4,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-27","day":27,"month":7,"year":2020,"cases":8,"deaths":0,"geo_id":"AF"}|

如果文件公开可用，或者你的 Azure AD 标识可以访问该文件，则应该使用类似于以下示例的查询来查看该文件的内容。

### <a name="read-json-files"></a>读取 JSON 文件

下面的示例查询读取 JSON 和行分隔的 JSON 文件，并将每个文档作为单独的行返回。

```sql
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.chinacloudapi.cn/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.jsonl',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.chinacloudapi.cn/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

前面的示例查询中的 JSON 文档包含一个对象数组。 查询在结果集中将每个对象返回为单独的行。 请确保你可以访问此文件。 如果文件受到 SAS 密钥或自定义标识的保护，则你需要[为 SQL 登录设置服务器级别的凭据](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential)。 

### <a name="data-source-usage"></a>数据源使用情况

上面的示例使用文件的完整路径。 作为替代方法，你可以创建一个外部数据源，其中包含指向存储根文件夹的位置，并在 `OPENROWSET` 函数中使用该数据源和指向该文件的相对路径：

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.chinacloudapi.cn/public/curated/covid-19/ecdc_cases' );
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.json',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

如果数据源受到 SAS 密钥或自定义标识的保护，则你可以[使用数据库范围的凭据配置数据源](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential)。

以下部分介绍如何查询各种类型的 JSON 文件。

## <a name="parse-json-documents"></a>分析 JSON 文档

前面示例中的查询将每个 JSON 文档作为单个字符串返回到结果集中单独的行中。 可以使用函数 `JSON_VALUE` 和 `OPENJSON` 分析 JSON 文档中的值，并将它们作为关系值返回，如以下示例中所示：

| date\_rep | cases | geo\_id |
| --- | --- | --- |
| 2020-07-24 | 3 | AF |
| 2020-07-25 | 7 | AF |
| 2020-07-26 | 4 | AF |
| 2020-07-27 | 8| AF |

### <a name="sample-json-document"></a>示例 JSON 文档

查询示例读取包含具有以下结构的文档的 JSON 文件：

```json
{
    "date_rep":"2020-07-24",
    "day":24,"month":7,"year":2020,
    "cases":13,"deaths":0,
    "countries_and_territories":"Afghanistan",
    "geo_id":"AF",
    "country_territory_code":"AFG",
    "continent_exp":"Asia",
    "load_date":"2020-07-25 00:05:14",
    "iso_country":"AF"
}
```

> [!NOTE]
> 如果这些文档存储为以行分隔的 JSON，则需要将 `FIELDTERMINATOR` 和 `FIELDQUOTE` 设置为 0x0b。 如果具有标准 JSON 格式，则需要将 `ROWTERMINATOR` 设置为 0x0b。

### <a name="query-json-files-using-json_value"></a>使用 JSON_VALUE 查询 JSON 文件

下面的查询展示了如何使用 [JSON_VALUE](https://docs.microsoft.com/sql/t-sql/functions/json-value-transact-sql?view=azure-sqldw-latest&preserve-view=true) 从 JSON 文档中检索标量值（标题和发布者）：

```sql
select
    JSON_VALUE(doc, '$.date_rep') AS date_reported,
    JSON_VALUE(doc, '$.countries_and_territories') AS country,
    JSON_VALUE(doc, '$.cases') as cases,
    doc
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
order by JSON_VALUE(doc, '$.geo_id') desc
```

### <a name="query-json-files-using-openjson"></a>使用 OPENJSON 查询 JSON 文件

以下查询使用 [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql?view=azure-sqldw-latest&preserve-view=true)。 它将检索在塞尔维亚报告的新冠肺炎统计信息：

```sql
select
    *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
    cross apply openjson (doc)
        with (  date_rep datetime2,
                cases int,
                fatal int '$.deaths',
                country varchar(100) '$.countries_and_territories')
where country = 'Serbia'
order by country, date_rep desc;
```

## <a name="next-steps"></a>后续步骤

本系列文章中的下一篇文章将演示如何：

- [查询文件夹和多个文件](query-folders-multiple-csv-files.md)
- [创建和使用视图](create-use-views.md)
