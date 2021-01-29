---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: JSON API - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 数据源及其管理方式。
ms.openlocfilehash: 559f579c4792e171043ebba864339799ad97a86b
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692205"
---
# <a name="json-api"></a>JSON API

有时，你需要将 RDBMS 或 NOSQL 数据存储中未包含，但可从某些 HTTP API 获取的数据进行可视化处理。 针对这些情况，SQL Analytics 提供了 JSON 数据源。

SQL Analytics 将来自 JSON 数据源的所有传入数据都视为文本，因此在呈现数据时必须使用[表格式](../../user/visualizations/tables.md)。

## <a name="json-data-source-type"></a>JSON 数据源类型

使用 JSON 数据源查询 JSON API。 设置很简单，无需进行身份验证。 任何 REST JSON API 都将通过 HTTP 标头处理身份验证。 新建一个 JSON 类型的外部数据源，并随意命名（“JSON”是一个不错的选择）。

SQL Analytics 会检测 JSON 支持的数据类型（如数字、字符串、布尔值），而其他类型（主要是日期/时间戳）将被视为字符串，除非它们以 ISO-8601 格式指定。

## <a name="usage"></a>使用情况

此数据源接受 [YAML 格式]的查询。 下面是使用 GitHub API 的一些示例。

### <a name="return-a-list-of-objects-from-an-endpoint"></a>从终结点返回对象列表

```yaml
url: https://api.github.com/repos/getsql/sql/issues
```

这将按原样返回上述 API 调用的结果。

> [!div class="mx-imgBorder"]
> ![对象的 JSON 列表](../../../_static/images/sql/json_list_of_objects.png)

### <a name="return-a-single-object"></a>返回一个对象

```yaml
url: https://api.github.com/repos/getsql/sql/issues/3495
```

上述 API 调用会返回一个对象，此对象将转换为一行。

> [!div class="mx-imgBorder"]
> ![单个 JSON 对象](../../../_static/images/sql/json_single_object.png)

### <a name="return-specific-rields"></a>返回特定字段

若要仅从生成的对象中选择特定字段，可传递 ``fields`` 选项：

```yaml
url: https://api.github.com/repos/getsql/sql/issues
fields: [number, title]
```

> [!div class="mx-imgBorder"]
> ![选择 JSON 字段](../../../_static/images/sql/json_field_select.png)

### <a name="return-an-inner-object"></a>返回内部对象

许多 JSON API 会返回嵌套对象的数组。 可使用 ``path`` 键来访问数组中的对象。

```yaml
url: https://api.github.com/repos/getsql/sql/issues/3495
path: assignees
```

此查询使用 API 结果中的 ``assignee`` 对象作为查询结果。

### <a name="pass-query-string-parameters"></a>传递查询字符串参数

你可创建自己的 URL，也可传递 ``params`` 选项：

```yaml
url: "https://api.github.com/search/issues"
params:
  q: is:open type:pr repo:getsql/sql
  sort: created
  order: desc
```

此查询相当于：

```yaml
url: "https://api.github.com/search/issues?q=+is:open+type:pr+repo:getsql/sql&sort=created&order=desc"
```

### <a name="additional-http-options"></a>其他 HTTP 选项

你可传递其他键来修改各种 HTTP 选项：

* ``method`` - 要使用的 HTTP 方法（默认为 ``get``）
* ``headers`` - 要与请求一同发送的标头的字典
* ``auth`` - 基本身份验证用户名和密码（应作为数组传递：``[username, password]``）
* ``params`` - 要添加到 URL 的查询字符串参数的字典
* ``data`` - 要用作请求正文的值的字典
* ``json`` - 与 ``data`` 相同，只不过它将转换为 JSON

## <a name="usage"></a>使用情况

查询正文应只包含返回数据的 URL，例如：

```
http://myserver/path/myquery
```

## <a name="required-data-structure"></a>必需的数据结构

返回的对象必须公开两个键：``columns`` 和 ``rows``。

* ``columns`` 键应公开一个 Javascript 对象数组，该数组描述要包含在数据集中的列。 每个对象包括 3 个键：
  * ``name``
  * ``type``
  * ``friendly_name``
* ``rows`` 应返回表示每行数据的 Javascript 对象的数组。 每个对象的键应与 ``columns`` 数组中描述的 ``name`` 键相匹配。

列支持以下数据类型：

* 文本
* integer
* float
* boolean
* string
* datetime
* date

返回的数据示例：

```json
{
  "columns": [
    {
      "name": "date",
      "type": "date",
      "friendly_name": "date"
    },
    {
      "name": "day_number",
      "type": "integer",
      "friendly_name": "day_number"
    },
    {
      "name": "value",
      "type": "integer",
      "friendly_name": "value"
    },
    {
      "name": "total",
      "type": "integer",
      "friendly_name": "total"
    }
  ],
  "rows": [
    {
      "value": 40832,
      "total": 53141,
      "day_number": 0,
      "date": "2014-01-30"
    },
    {
      "value": 27296,
      "total": 53141,
      "day_number": 1,
      "date": "2014-01-30"
    },
    {
      "value": 22982,
      "total": 53141,
      "day_number": 2,
      "date": "2014-01-30"
    }
  ]
}
```