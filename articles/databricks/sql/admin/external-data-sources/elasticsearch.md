---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: ElasticSearch - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 数据源及其管理方式。
ms.openlocfilehash: 5ec473315831c83cfda98fc3d26e5afb61649c0f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692215"
---
# <a name="elasticsearch"></a>ElasticSearch

SQL Analytics 支持两种类型的 ElasticSearch 查询，即 Lucene/字符串样式查询（例如 Kibana）和更复杂的基于 JSON 的查询。 对于第一种类型，创建类型为 ``Kibana`` 的数据源；对于第二种类型，创建类型为 ``Elasticsearch`` 的数据源。

## <a name="string-query-example"></a>字符串查询示例：

* 查询名为“twitter”的索引
* 按“user:kimchy”进行筛选
* 返回字段：“@timestamp”、“tweet”和“user”
* 最多返回 15 个结果
* 按 @timestamp 升序排序

```json
{
  "index": "twitter",
  "query": "user:kimchy",
  "fields": ["@timestamp", "tweet", "user"],
  "limit": 15,
  "sort": "@timestamp:asc"
}
```

## <a name="simple-query-on-a-logstash-elasticsearch-instance"></a>logstash ElasticSearch 实例上的简单查询：

* 查询名为“logstash-2015.04.*”的索引（在本例中为所有 2015 年 4 月）
* 按 type:events 且 eventName:UserUpgrade 且 channel:selfserve 进行筛选
* 返回字段：“@timestamp”、“userId”、“channel”、“utm_source”、“utm_medium”、“utm_campaign”、“utm_content”
* 最多返回 250 个结果
* 按 @timestamp 升序排序

```json
{
  "index": "logstash-2015.04.*",
  "query": "type:events AND eventName:UserUpgrade AND channel:selfserve",
  "fields": ["@timestamp", "userId", "channel", "utm_source", "utm_medium", "utm_campaign", "utm_content"],
  "limit": 250,
  "sort": "@timestamp:asc"
}
```

## <a name="json-document-query-on-a-elasticsearch-instance"></a>ElasticSearch 实例上的 JSON 文档查询：

* 查询名为“twitter”的索引
  * 按用户“kimchy”进行筛选
  * 返回字段：“@timestamp”、“tweet”和“user”
  * 最多返回 15 个结果
  * 按 @timestamp 升序排序

```json
{
  "index": "twitter",
  "query": {
    "match": {
      "user": "kimchy"
    }
  },
  "fields": ["@timestamp", "tweet", "user"],
  "limit": 15,
  "sort": "@timestamp:asc"
}
```