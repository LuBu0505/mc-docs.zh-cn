---
title: todynamic() 和 parse_json() 函数 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 todynamic() 和 parse_json() 函数。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
ms.date: 03/18/2021
ms.localizationpriority: high
ms.openlocfilehash: fde7c15c7812956e796cbd9a5acf266415e09bb7
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766497"
---
# <a name="todynamic-parse_json"></a>todynamic(), parse_json()

将 `string` 解释为 JSON 值并以 `dynamic` 形式返回值。 

> [!NOTE]
> `todynamic()` 和 `parse_json()` 函数以等效方式进行解释。

当需要提取 JSON 复合对象的多个元素时，使用此函数比使用 [extractjson() function](./extractjsonfunction.md) 函数更好。 尽可能首选使用 [dynamic()](./scalar-data-types/dynamic.md)。

## <a name="syntax"></a>语法

`parse_json(`*json*`)`
`todynamic(`*json*`)`

<!-- deprecated aliases: `toobject()` and parsejson() -->

## <a name="arguments"></a>参数

* json：`string` 类型的表达式。 它表示 [JSON 格式的值](https://json.org/)或 [dynamic](./scalar-data-types/dynamic.md) 类型的表达式（表示实际的 `dynamic` 值）。

## <a name="returns"></a>返回

`dynamic` 类型的对象，该对象由 json 的值确定：
* 如果 json 的类型为 `dynamic`，则其值将按原样使用。
* 如果 json 的类型为 `string`，并且是[格式正确的 JSON 字符串](https://json.org/)，则系统会分析字符串并返回生成的值。
* 如果 json 的类型为 `string`，但不是[格式正确的 JSON 字符串](https://json.org/)，则返回的值是包含原始 `string` 值的类型为 `dynamic` 的对象。

## <a name="example"></a>示例

在以下示例中，如果 `context_custom_metrics` 是类似如下的 `string`：

```json
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

则以下 CSL 片段会检索对象中 `duration` 槽的值，并从中检索两个槽：`duration.value` 和 `duration.min`（分别为 `118.0` 和 `110.0`）。

```kusto
T
| extend d=parse_json(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```

**备注**

通常用一个 JSON 字符串来描述属性包，其中的一个“槽”是另一个 JSON 字符串。 

例如：

```kusto
let d='{"a":123, "b":"{\\"c\\":456}"}';
print d
```

在这种情况下，不仅需要调用 `parse_json` 两次，而且还需要确保在第二次调用中使用 `tostring`。 否则，对 `parse_json` 的第二次调用会只按原样将输入传递到输出，因为它的已声明类型为 `dynamic`。

```kusto
let d='{"a":123, "b":"{\\"c\\":456}"}';
print d_b_c=parse_json(tostring(parse_json(d).b)).c
```
