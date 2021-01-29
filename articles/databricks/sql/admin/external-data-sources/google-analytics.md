---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: Google Analytics - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 数据源及其管理方式。
ms.openlocfilehash: cc8553bbfdb53730fbac0ad87e7fb85bdadf20c6
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692214"
---
# <a name="google-analytics"></a>Google Analytics

## <a name="create-a-service-account"></a>创建服务帐户

1. 打开[“服务帐户”页面](https://console.cloud.google.com/iam-admin/serviceaccounts)。 如果出现提示，请选择一个项目。
2. 单击“创建服务帐户”。
3. 在“创建服务帐户”窗口中，键入服务帐户的名称，然后选择“提供新的私钥”。 出现提示时，选择“JSON 密钥文件类型”。 然后单击“创建”。

这会生成新的公钥/私钥对，并将其下载到计算机上；它是此密钥的唯一副本。 由你负责安全存储它。

## <a name="enable-the-api"></a>启用 API

需要为 Google Cloud 项目启用“Analytics API”。

## <a name="add-service-account-to-the-google-analytics-account"></a>向 Google Analytics 帐户添加服务帐户

新创建的服务帐户将包含如下所示的电子邮件地址：``quickstart@PROJECT-ID.iam.gserviceaccount.com``
使用此电子邮件地址[添加用户](https://support.google.com/analytics/answer/1009702)到你想要通过 API 访问的 Google Analytics 视图。 对于 SQL Analytics，仅需要[读取和分析](https://support.google.com/analytics/answer/2884495)权限。

## <a name="create-data-source-in-sql-analytics"></a>在 SQL Analytics 中创建数据源

使用在“创建服务帐户”步骤中生成的 JSON 文件创建“Google Analytics”类型的数据源。

## <a name="queries"></a>查询

Google Analytics 使用 JSON 文档样式查询。 可使用[查询资源管理器工具](https://ga-dev-tools.appspot.com/query-explorer/)了解可能的字段类型和维度。

### <a name="example-queries"></a>查询示例

```json
{
    "ids": "ga:97038718",
    "start_date": "30daysAgo",
    "end_date": "yesterday",
    "metrics": "ga:newUsers",
    "dimensions": "ga:country",
    "max_results": 10,
    "sort": "-ga:newUsers"
}
```

```json
{
    "ids": "ga:97038718",
    "start_date": "30daysAgo",
    "end_date": "yesterday",
    "metrics": "ga:newUsers",
    "dimensions": "ga:date",
    "sort": "-ga:newUsers"
}
```