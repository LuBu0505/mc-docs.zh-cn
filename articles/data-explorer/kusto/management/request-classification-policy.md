---
title: 请求分类策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的请求分类策略。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 02/07/2021
ms.openlocfilehash: 9be9f05721d1921d845cddb31a904bbd438cf6ae
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101090110"
---
# <a name="request-classification-policy-preview"></a>请求分类策略（预览）

分类过程会根据请求的特征将传入请求分配到工作负荷组。 通过编写用户定义的函数来定制分类逻辑，作为群集级别请求分类策略的一部分。

如果没有启用请求分类策略，所有请求都将分类到 `default` 工作负荷组。

## <a name="policy-object"></a>策略对象

策略具有以下属性：

* `IsEnabled`：`bool` - 指示是否启用策略。
* `ClassificationFunction`：`string` - 用于对请求进行分类的函数主体。

## <a name="classification-function"></a>分类函数

根据用户定义的函数对传入请求进行分类。 用于将请求分类到现有工作负荷组的函数的结果。

用户定义的函数具有以下特征和行为：

* 如果在策略中将 `IsEnabled` 设置为 `true`，则将为每个新请求评估用户定义函数。
* 用户定义函数为请求的整个生存期提供请求的工作负荷组上下文。
* 在以下情况中，将为请求提供 `default` 工作负荷组上下文：
    * 用户定义函数会返回空字符串、`default` 或不存在的工作负荷组的名称
    * 函数由于任何原因运行失败。
* 在任何给定时间只能指定一个用户定义函数。

### <a name="requirements-and-limitations"></a>要求和限制

分类函数：

* 必须返回 `string` 类型的单个标量值，这是要向其分配请求的工作负荷组的名称。
* 不得引用其他任何实体（数据库、表或函数）。
  * 具体而言，不得使用以下函数和运算符：
    * `cluster()`
    * `database()`
    * `table()`
    * `external_table()`
    * `externaldata`
* 有权访问特殊 `dynamic` 符号，即具有以下属性的 `request_properties` 属性包：

    | 名称                | 类型     | 说明           | 示例         |
    |---------------------|----------|-------|---------------|
    | current_database    | `string` | 请求数据库的名称。     | `"MyDatabase"`       |
    | current_application | `string` | 发送请求的应用程序的名称。   | `"Kusto.Explorer"`  |
    | current_principal   | `string` | 发送请求的主体标识的完全限定名称。       | `"aaduser=1793eb1f-4a18-418c-be4c-728e310c86d3;83af1c0e-8c6d-4f09-b249-c67a2e8fda65"` |
    | query_consistency   | `string` | 在查询中，指查询的一致性（`strongconsistency` 或 `weakconsistency`）。 调用方可将此属性设置为请求的[客户端请求属性](../api/netfx/request-properties.md)的一部分：要设置的客户端请求属性为 `queryconsistency`。   | `"strongconsistency"`      |
    | request_description | `string` | 请求的作者可以包含的自定义文本。 调用方可将此文本设置为请求的[客户端请求属性](../api/netfx/request-properties.md)的一部分：要设置的客户端请求属性为 `request_description`。     | `"Some custom description"`       |
    | request_text        | `string` | 请求的混淆文本。 查询文本中包含的混淆字符串字面量被多个星号 (`*`) 字符替换。      | `".show version"`    |
    | request_type        | `string` | 请求的类型（`Command` 或 `Query`）。            | `"Command"`               |

### <a name="examples"></a>示例

#### <a name="a-single-workload-group"></a>单个工作负荷组

```kusto
iff(request_properties.current_application == "Kusto.Explorer" and request_properties.request_type == "Query",
    "Ad-hoc queries",
    "default")
```

#### <a name="multiple-workload-groups"></a>多个工作负荷组

```kusto
case(current_principal_is_member_of('aadgroup=somesecuritygroup@contoso.com'), "First workload group",
     request_properties.current_database == "MyDatabase" and request_properties.current_principal has 'aadapp=', "Second workload group",
     request_properties.current_application == "Kusto.Explorer" and request_properties.request_type == "Query", "Third workload group",
     request_properties.current_application == "Kusto.Explorer", "Third workload group",
     request_properties.current_application == "KustoQueryRunner", "Fourth workload group",
     request_properties.request_description == "this is a test", "Fifth workload group",
     hourofday(now()) between (17 .. 23), "Sixth workload group",
     "default")
```

## <a name="control-commands"></a>控制命令

使用以下[控制命令](request-classification-policy-commands.md)管理群集的请求分类。
