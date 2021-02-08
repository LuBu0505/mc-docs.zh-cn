---
title: 遥测处理器示例 - 适用于 Java 的 Azure Monitor Application Insights
description: 通过示例介绍了适用于 Java 的 Azure Monitor Application Insights 中的遥测处理器
ms.topic: conceptual
ms.date: 01/27/2021
author: Johnnytechn
ms.custom: devx-track-java
ms.author: v-johya
ms.openlocfilehash: 2152e58140ef55a5ae5501e2cc1c1a863887c954
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061037"
---
# <a name="telemetry-processors-examples---azure-monitor-application-insights-for-java"></a>遥测处理器示例 - 适用于 Java 的 Azure Monitor Application Insights

## <a name="includeexclude-samples"></a>包括/排除示例

### <a name="1-include-spans"></a>1.包括范围

下面演示了如何包括此属性处理器的范围。 与属性不匹配的所有其他范围均不由此处理器处理。

下面是匹配项要满足的条件：
* 范围名称必须等于“spanA”或“spanB” 

以下范围与 include 属性匹配，处理器操作得到应用。
* Span1 Name: 'spanA' Attributes: {env: dev, test_request:123, credit_card:1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env:1, test_request: dev, credit_card:1234}

以下范围与 include 属性不匹配，处理器操作得不到应用。
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
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
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="2-exclude-spans"></a>2.排除范围

下面演示了如何排除此属性处理器的范围。 与属性匹配的所有范围均不由此处理器处理。

下面是匹配项要满足的条件：
* 范围名称必须等于“spanA”或“spanB” 

以下范围与 exclude 属性匹配，处理器操作得不到应用。
* Span1 Name: 'spanA' Attributes: {env: dev, test_request:123, credit_card:1234}
* Span2 Name: 'spanB' Attributes: {env: dev, test_request: false}
* Span3 Name: 'spanA' Attributes: {env:1, test_request: dev, credit_card:1234}

以下范围与 exclude 属性不匹配，处理器操作得到应用。
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="3-excludemulti-spans"></a>3.排除多个范围

下面演示了如何排除此属性处理器的范围。 与属性匹配的所有范围均不由此处理器处理。

下面是匹配项要满足的条件：
* 范围中必须存在属性 ('env', 'dev') 才能匹配。
* 只要范围中存在键为“test_request”的属性，就会有匹配项。

以下范围与 exclude 属性匹配，处理器操作得不到应用。
* Span1 Name: 'spanB' Attributes: {env: dev, test_request:123, credit_card:1234}
* Span2 Name: 'spanA' Attributes: {env: dev, test_request: false}

以下范围与 exclude 属性不匹配，处理器操作得到应用。
* Span3 Name: 'spanB' Attributes: {env:1, test_request: dev, credit_card:1234}
* Span4 Name: 'spanC' Attributes: {env: dev, dev_request: false}


```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "spanA",
            "spanB"
          ],
          "attributes": [
            {
              "key": "env",
              "value": "dev"
            },
            {
              "key": "test_request"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="4-selective-processing"></a>4.选择性处理

下面的示例演示了如何指定范围属性组，以指示此处理器应当应用于哪些范围。 属性的 `include` 指示应该包括哪些属性，而 `exclude` 属性则进一步筛选掉不应处理的范围。

使用下面的配置时，以下范围与属性匹配，处理器操作得到应用：

* Span1 Name: 'spanB' Attributes: {env: production, test_request:123, credit_card:1234, redact_trace: "false"}
* Span2 Name: 'spanA' Attributes: {env: staging, test_request: false, redact_trace: true}

以下范围与 include 属性不匹配，处理器操作得不到应用：
* Span3 Name: 'spanB' Attributes: {env: production, test_request: true, credit_card:1234, redact_trace: false}
* Span4 Name: 'spanC' Attributes: {env: dev, test_request: false}

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
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
  }
}
```
## <a name="attribute-processor-samples"></a>属性处理器示例

### <a name="insert"></a>插入

以下代码向不存在键 "attribute1" 的范围插入新属性 {"attribute1": "attributeValue1"}。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "attribute1",
            "value": "attributeValue1",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="insert-from-another-key"></a>从另一个键插入

以下代码使用属性 "anotherkey" 中的值向不存在键 "newKey" 的范围插入新属性 {"newKey": "value from attribute 'anotherkey'"}。 如果属性 'anotherkey' 不存在，则不会向范围中插入新属性。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "newKey",
            "fromAttribute": "anotherKey",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

### <a name="update"></a>更新

以下代码将属性更新为 { "db.secret": "redacted"} 并使用属性 'foo' 的值更新属性 'boo'。 没有属性 'boo' 的范围不会进行更改。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "db.secret",
            "value": "redacted",
            "action": "update"
          },
          {
            "key": "boo",
            "fromAttribute": "foo",
            "action": "update" 
          }
        ]
      }
    ]
  }
}
```

### <a name="delete"></a>删除

下面演示了如何删除键为 'credit_card' 的属性。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

### <a name="hash"></a>哈希

下面演示了如何将现有属性值哈希化。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "user.email",
            "action": "hash"
          }
        ]
      }
    ]
  }
}
```

### <a name="extract"></a>Extract

下面的示例演示了如何使用正则表达式基于另一个属性的值创建新属性。
例如，如果 http.url = 'http://example.com/path?queryParam1=value1,queryParam2=value2'，则会插入以下属性：
* httpProtocol: http
* httpDomain: example.com
* httpPath: path
* httpQueryParams: queryParam1=value1,queryParam2=value2
* http.url 值不会更改。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "http.url",
            "pattern": "^(?<httpProtocol>.*):\\/\\/(?<httpDomain>.*)\\/(?<httpPath>.*)(\\?|\\&)(?<httpQueryParams>.*)",
            "action": "extract"
          }
        ]
      }
    ]
  }
}
```

以下示例演示了如何处理范围名称与正则表达式模式匹配的范围。
此处理器会删除“token”属性，并在范围名称与“auth.\*”匹配但与“login.\*”不匹配的范围中对“password”属性进行模糊处理。

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "auth.*"
          ]
        },
        "exclude": {
          "matchType": "regexp",
          "spanNames": [
            "login.*"
          ]
        },
        "actions": [
          {
            "key": "password",
            "value": "obfuscated",
            "action": "update"
          },
          {
            "key": "token",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```


## <a name="span-processor-samples"></a>范围处理器示例

### <a name="name-a-span"></a>为范围命名

以下示例指定属性“db.svc”、“operation”和“id”的值将按该顺序形成范围的新名称，以值“::”作为分隔符。
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "fromAttributes": [
            "db.svc",
            "operation",
            "id"
          ],
          "separator": "::"
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name"></a>从范围名称中提取属性

让我们假设输入范围名称为 /api/v1/document/12345678/update。 将以下结果应用于输出范围名称 /api/v1/document/{documentId}/update 会将新属性 "documentId"="12345678" 添加到范围。
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "toAttributes": {
            "rules": [
              "^/api/v1/document/(?<documentId>.*)/update$"
            ]
          }
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name-with-include-and-exclude"></a>在使用 include 和 exclude 的情况下从范围名称中提取属性

下面演示了如何在范围具有以下属性时将范围名称重命名为“{operation_website}”并添加属性 {Key: operation_website, Value: oldSpanName }：
- 范围名称在字符串中的任意位置包含“/”。
- 范围名称不是“donot/change”。
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "^(.*?)/(.*?)$"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "donot/change"
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [
              "(?<operation_website>.*?)$"
            ]
          }
        }
      }
    ]
  }
}
```

