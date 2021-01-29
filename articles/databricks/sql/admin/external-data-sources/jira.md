---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: Jira - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 数据源及其管理方式。
ms.openlocfilehash: a65fefd4e199d3fb9ca06d5412d3276d13a209c1
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692209"
---
# <a name="jira"></a>Jira

需要“用户名”（用于登录的电子邮件地址）和 [API 令牌](https://confluence.atlassian.com/cloud/api-tokens-938839638.html)才能连接到 Jira 。 令牌的行为类似于密码。

> [!div class="mx-imgBorder"]
> ![Jira 数据源](../../../_static/images/sql/jira-setup.png)

## <a name="query"></a>查询

对于简单查询，返回问题而不筛选：

```json
{}
```

仅返回特定字段：

```json
{
  "fields": "summary,priority"
}
```

仅返回特定字段并按优先级筛选：

```json
{
  "fields": "summary,priority",
  "jql": "priority=medium"
}
```

计算 ``priority=medium`` 的问题数：

```json
{
  "queryType": "count",
  "jql": "priority=medium"
}
```

还可使用字段映射为结果字段重命名，这在使用自定义字段时非常有用：

```json
{
  "fields": "summary,priority,customfield_10672",
  "jql": "priority=medium",
  "fieldMapping": {
    "customfield_10672": "my_custom_field_name"
  }
}
```

JIRA 返回的某些字段是具有多个属性的 JSON 对象。 可定义一个字段映射，以选择要返回的特定成员属性（在此示例中为“priority”字段的“id”成员）：

```json
{
  "fields": "summary,priority",
  "jql": "priority=medium",
  "fieldMapping": {
    "priority.id": "priority"
  }
}
```

下面是一个合并了不同筛选器选项的更复杂的示例：

```json
{
  "fields": "summary,priority,customfield_10672,resolutiondate,fixVersions,watches,labels",
  "jql": "project = MYPROJ AND resolution = unresolved ORDER BY priority DESC, key ASC",
  "maxResults": 30,
  "fieldMapping": {
    "customfield_10672": "my_custom_field_name",
    "priority.id": "priority",
    "fixVersions.name": "my_fix_version",
    "fixVersions.id": "my_fix_version_id"
  }
}
```

如果字段包含值列表，则返回的所有值都用“,”串联。