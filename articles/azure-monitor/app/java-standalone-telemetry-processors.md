---
title: 遥测处理器（预览版）- 适用于 Java 的 Azure Monitor Application Insights
description: 如何在适用于 Java 的 Azure Monitor Application Insights 中配置遥测处理器
ms.topic: conceptual
ms.date: 01/27/2021
author: Johnnytechn
ms.custom: devx-track-java
ms.author: v-johya
ms.openlocfilehash: 57926050e5643accac797f1cc8f0054d116f6def
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059911"
---
# <a name="telemetry-processors-preview---azure-monitor-application-insights-for-java"></a>遥测处理器（预览版）- 适用于 Java 的 Azure Monitor Application Insights

> [!NOTE]
> 此功能目前仍为预览版。

适用于 Application Insights 的 Java 3.0 代理现在具有在导出遥测数据之前处理该数据的功能。

下面是遥测处理器的一些用例：
 * 屏蔽敏感数据
 * 有条件地添加自定义维度
 * 更新在 Azure 门户中用于聚合和显示的名称
 * 删除范围属性以控制引入成本

## <a name="terminology"></a>术语

在讨论遥测处理器之前，请务必了解术语“范围”所指的内容。

范围是一个表示下列三个事项中的任意一项的通用术语：

* 传入请求
* 传出依赖项（例如，对另一个服务的远程调用）
* 进程内依赖项（例如，由服务的子组件所做的工作）

对于遥测处理器而言，范围的重要组件包括：

* 名称
* 特性

范围名称是 Azure 门户中用于请求和依赖项的主显示名称。

范围属性表示给定请求或依赖项的标准属性和自定义属性。

## <a name="telemetry-processor-types"></a>遥测处理器类型

目前有两种类型的遥测处理器。

#### <a name="attribute-processor"></a>属性处理器

属性处理器能够插入、更新、删除或哈希化属性。
它还可以从现有属性提取一个或多个新属性（通过正则表达式）。

#### <a name="span-processor"></a>范围处理器

范围处理器能够更新遥测名称。
它还可以从范围名称提取一个或多个新属性（通过正则表达式）。

> [!NOTE]
> 请注意，遥测处理器当前仅处理字符串类型的属性，不处理布尔或数字类型的属性。

## <a name="getting-started"></a>入门

使用以下模板创建一个名为 `applicationinsights.json` 的配置文件，并将其置于 `applicationinsights-agent-*.jar` 所在的目录中。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      }
    ]
  }
}
```

## <a name="includeexclude-criteria"></a>包括/排除条件

属性处理器和范围处理器都支持可选的 `include` 和 `exclude` 条件。
处理器将仅应用于与其 `include` 条件（如果有）匹配且与其 `exclude` 条件（如果有）不匹配的那些范围。

若要配置此选项，则在 `include` 和/或 `exclude` 下至少必须有一个 `matchType` 以及 `spanNames` 和 `attributes` 中的一个。
允许包括/排除配置具有多个指定的条件。
所有指定条件的评估结果都必须为 true 才会被视为匹配。 

必填字段： 
* `matchType` 控制如何解释 `spanNames` 和 `attributes` 数组中的项。 可能的值包括 `regexp` 或 `strict`。 

可选字段： 
* `spanNames` 必须至少与一个项匹配。 
* `attributes` 指定要用作匹配依据的属性列表。 所有这些属性必须完全匹配才会被视为匹配。

> [!NOTE]
> 如果同时指定了 `include` 和 `exclude`，则会在 `exclude` 属性之前检查 `include` 属性。

#### <a name="sample-usage"></a>示例用法

```json

"processors": [
  {
    "type": "attribute",
    "include": {
      "matchType": "strict",
      "spanNames": [
        "spanA",
        "spanB"
      ]
    },
    "exclude": {
      "matchType": "strict",
      "attributes": [
        {
          "key": "redact_trace",
          "value": "false"
        }
      ]
    },
    "actions": [
      {
        "key": "credit_card",
        "action": "delete"
      },
      {
        "key": "duplicate_key",
        "action": "delete"
      }
    ]
  }
]
```
若要更深入地进行了解，请查看[遥测处理器示例](./java-standalone-telemetry-processors-examples.md)文档。

## <a name="attribute-processor"></a>属性处理器

属性处理器修改范围的属性。 它还可以支持用于包括/排除范围的功能。 它接受一个操作列表，这些操作按配置文件中指定的顺序执行。 支持的操作有：

### `insert`

在不存在键的范围中插入新属性   

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "value1",
        "action": "insert"
      }
    ]
  }
]
```
对于 `insert` 操作，以下项是必需的
  * `key`
  * `value` 或 `fromAttribute` 中的一个
  * `action`:`insert`

### `update`

更新存在键的范围中的属性

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "value": "newValue",
        "action": "update"
      }
    ]
  }
]
```
对于 `update` 操作，以下项是必需的
  * `key`
  * `value` 或 `fromAttribute` 中的一个
  * `action`:`update`


### `delete` 

从范围中删除属性

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "delete"
      }
    ]
  }
]
```
对于 `delete` 操作，以下项是必需的
  * `key`
  * `action`: `delete`

### `hash`

将现有属性值哈希化 (SHA1)

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "action": "hash"
      }
    ]
  }
]
```
对于 `hash` 操作，以下项是必需的
* `key`
* `action` : `hash`

### `extract`

> [!NOTE]
> 此功能仅在 3.0.2 及更高版本中提供

使用正则表达式规则将值从输入键提取到规则中指定的目标键。 如果目标键已存在，则会将其替代。 它的行为类似于以现有属性作为源的[范围处理器](#extract-attributes-from-span-name) `toAttributes` 设置。

```json
"processors": [
  {
    "type": "attribute",
    "actions": [
      {
        "key": "attribute1",
        "pattern": "<regular pattern with named matchers>",
        "action": "extract"
      }
    ]
  }
]
```
对于 `extract` 操作，以下项是必需的
* `key`
* `pattern`
* `action` : `extract`

若要更深入地进行了解，请查看[遥测处理器示例](./java-standalone-telemetry-processors-examples.md)文档。

## <a name="span-processor"></a>范围处理器

范围处理器修改范围名称或根据范围名称修改范围的属性。 它还可以支持用于包括/排除范围的功能。

### <a name="name-a-span"></a>为范围命名

作为 name 节的一部分，以下设置是必需的：

* `fromAttributes`：键的属性值用于按配置中指定的顺序创建新名称。 需要在范围中指定所有属性键，处理器才能对其进行重命名。

还可以配置以下设置：

* `separator`：一个指定的字符串，将用来拆分值
> [!NOTE]
> 如果重命名依赖于属性处理器修改的属性，请确保在管道规范中的属性处理器之后指定范围处理器。

```json
"processors": [
  {
    "type": "span",
    "name": {
      "fromAttributes": [
        "attributeKey1",
        "attributeKey2",
      ],
      "separator": "::"
    }
  }
] 
```

### <a name="extract-attributes-from-span-name"></a>从范围名称中提取属性

接受正则表达式的列表，以据其匹配范围名称，并根据子表达式从中提取属性。 必须在 `toAttributes` 节下指定。

以下设置是必需的：

`rules`：用于从范围名称中提取属性值的规则列表。 范围名称中的值将替换为提取的属性名称。 列表中的每项规则都是一个正则表达式模式字符串。 将依据正则表达式来检查范围名称。 如果正则表达式匹配，则正则表达式的所有已命名子表达式都会被提取为属性并添加到范围中。 每个子表达式名称会成为一个属性名称，子表达式匹配部分会成为属性值。 范围名称中的匹配部分会替换为提取的属性名称。 如果属性已存在于范围中，则会被覆盖。 将按指定顺序为所有规则重复此过程。 每项后续规则都应用于处理上一规则后输出的范围名称。

```json

"processors": [
  {
    "type": "span",
    "name": {
      "toAttributes": {
        "rules": [
          "rule1",
          "rule2",
          "rule3"
        ]
      }
    }
  }
]

```

## <a name="list-of-attributes"></a>属性列表

下面列出了可以在遥测处理器中使用的一些常见范围属性。

### <a name="http-spans"></a>HTTP 范围

| Attribute  | 类型 | 说明 | 
|---|---|---|
| `http.method` | string | HTTP 请求方法。|
| `http.url` | string | 完整的 HTTP 请求 URL（采用 `scheme://host[:port]/path?query[#fragment]` 格式）。 通常情况下，片段不通过 HTTP 传输，但如果它是已知的，则应包括它。|
| `http.status_code` | 数值 | [HTTP 响应状态代码](https://tools.ietf.org/html/rfc7231#section-6)。|
| `http.flavor` | string | 使用的 HTTP 协议类型 |
| `http.user_agent` | string | 客户端发送的 [HTTP User-Agent](https://tools.ietf.org/html/rfc7231#section-5.5.3) 标头的值。 |

### <a name="jdbc-spans"></a>JDBC 范围

| Attribute  | 类型 | 说明  |
|---|---|---|
| `db.system` | string | 正在使用的数据库管理系统 (DBMS) 产品的标识符。 |
| `db.connection_string` | string | 用于连接到数据库的连接字符串。 建议删除嵌入的凭据。|
| `db.user` | string | 用于访问数据库的用户名。 |
| `db.name` | string | 此属性用于报告正在访问的数据库的名称。 对于用于切换数据库的命令，应当将此项设置为目标数据库（即使该命令失败）。|
| `db.statement` | string | 正在执行的数据库语句。|

