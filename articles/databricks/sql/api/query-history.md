---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/24/2020
title: 查询历史记录 API - Azure Databricks
description: 了解 Databricks 查询历史记录 API。
ms.openlocfilehash: 3f11bd18549d77be748b74a3f5f8e68d31321e7d
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692314"
---
# <a name="query-history-api"></a>查询历史记录 API

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

> [!IMPORTANT]
>
> 要访问 Databricks REST API，必须[进行身份验证](authentication.md)。

## <a name="list"></a>列表

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/history/queries``                | ``GET``         |

通过 [SQL 终结点](sql-endpoints.md)列出查询的历史记录。 可以按用户 ID、终结点 ID、状态和时间范围进行筛选。

### <a name="request"></a>请求

| 字段名称                        | 类型                              | 说明                                                                |
|-----------------------------------|-----------------------------------|----------------------------------------------------------------------------|
| filter_by                         | [QueryFilter](#queryfilter)       | 用于限制查询历史记录结果的筛选器。 此字段是可选的。           |
| max_results                       | ``INT32``                         | 限制一页中返回的结果数。 默认值为 100。      |
| page_token                        | ``STRING``                        | 用于获取下一页结果的不透明标记。 此字段是可选的。 |

### <a name="response"></a>响应

| 字段名称                        | 类型                              | 说明                               |
|-----------------------------------|-----------------------------------|-------------------------------------------|
| next_page_token                   | ``STRING``                        | 用于获取下一页的不透明标记。   |
| has_next_page                     | ``BOOLEAN``                       | 是否还有一页结果。 |
| res                               | [QueryInfo](#queryinfo) 的数组  | 查询结果。                            |

### <a name="example-request"></a>示例请求

```json
{
  "filter_by": {
    "statuses": ["RUNNING"],
    "user_ids": [12345],
    "endpoint_ids": ["1234567890abcdef"],
  },
  "max_results": 100
}
```

### <a name="example-response"></a>示例响应

```json
{
  "next_page_token": "Ci0KJDU4NjEwZjY5LTgzNzUtNDdiMS04YTg1LWYxNTU5ODI5MDYyMhDdobu YuS4SABhk",
  "has_next_page": true,
  "res": [
    {
      "query_id": "26b5c452-1dff-429e-9b55-7c16131c89ee",
      "status": "FINISHED",
      "query_text": "select 1 + 1",
      "query_start_time_ms": 1595357086200,
      "execution_end_time_ms": 1595357086373,
      "query_end_time_ms": 1595357087200,
      "user_id": [12345],
      "user_name": "user@example.com",
      "spark_ui_url":"https://<databricks-instance>/sparkui/0710-201419-test887/driver-8401376710892156045/SQL/execution/?id=0",
      "endpoint_id": "1234567890abcdef",
      "rows_produced": 100,
    },
    {
      "query_id": "26b5c452-1dff-429e-9b55-7c16131c89ee",
      "status": "FAILED",
      "query_text": "select 1 + 1",
      "query_start_time_ms": 1595357196200,
      "user_id": [12345],
      "user_name": "user@example.com",
      "endpoint_id": "1234567890abcdef",
      "error_message": "Query failed because ...",
    }
  ]
}
```

## <a name="data-structures"></a>数据结构

### <a name="queryfilter"></a>QueryFilter

| 字段名称                        | 类型                                 | 描述                                |
|-----------------------------------|--------------------------------------|--------------------------------------------|
| statuses                          | [QueryStatus](#querystatus) 的数组 | 查询的状态。                       |
| user_ids                          | ``INT64`` 的数组                   | 运行查询的用户 ID。     |
| endpoint_ids                      | ``STRING`` 的数组                  | 在其上运行查询的终结点 ID。 |
| query_start_time_range            | [TimeRange](#timerange)              | 查询开始的时间范围。    |

### <a name="queryinfo"></a>QueryInfo

| 字段名称                        | 类型                              | 说明                                          |
|-----------------------------------|-----------------------------------|------------------------------------------------------|
| query_id                          | ``INT64``                         | 查询 ID。                                            |
| status                            | [QueryStatus](#querystatus)       | 查询状态。                                        |
| query_text                        | ``STRING``                        | 查询的文本。                               |
| query_start_time_ms               | ``INT64``                         | 查询的开始时间。                          |
| execution_end_time_ms             | ``INT64``                         | 查询执行结束的时间。               |
| query_end_time_ms                 | ``INT64``                         | 查询结束的时间。                            |
| user_id                           | ``INT64``                         | 运行查询的用户 ID。                   |
| user_name                         | ``STRING``                        | 运行查询的用户的电子邮件地址。        |
| spark_ui_url                      | ``STRING``                        | 查询计划的 URL。                               |
| endpoint_id                       | ``STRING``                        | 终结点 ID。                                         |
| error_message                     | ``STRING``                        | 描述查询无法完成的原因的消息。 |
| rows_produced                     | ``INT32``                         | 查询返回的结果数。         |

### <a name="querystatus"></a>QueryStatus

| 状态                                             | 说明                                        |
|----------------------------------------------------|----------------------------------------------------|
| ``QUEUED``                                         | 查询已接收并排队。                |
| ``RUNNING``                                        | 查询执行已开始。                       |
| ``CANCELED``                                       | 用户已取消查询。              |
| ``FAILED``                                         | 查询执行失败。                        |
| ``FINISHED``                                       | 查询执行已完成。                     |

### <a name="timerange"></a>TimeRange

| 字段名称                        | 类型                              | 说明                                             |
|-----------------------------------|-----------------------------------|---------------------------------------------------------|
| start_time_ms                     | ``INT64``                         | 将结果限制为在此时间后开始的查询。  |
| end_time_ms                       | ``INT64``                         | 将结果限制为在此时间之前开始的查询。 |