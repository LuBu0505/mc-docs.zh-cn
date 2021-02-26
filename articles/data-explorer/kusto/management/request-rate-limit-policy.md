---
title: 请求速率限制策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的请求速率限制策略。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 02/07/2021
ms.openlocfilehash: 4d87471f02150815591eb3abaa2faa7603afe882
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101090108"
---
# <a name="request-rate-limit-policy-preview"></a>请求速率限制策略（预览）

工作负荷组的请求速率限制策略使你能够限制分类到工作负荷组的并发请求数：
  * 每个工作负荷组
  * 每个主体

## <a name="the-policy-object"></a>策略对象

请求速率限制策略具有以下属性：

| 名称       | 支持的值                            | 说明                                |
|------------|---------------------------------------------|--------------------------------------------|
| IsEnabled  | `true`, `false`                             | 指示是否启用策略。 |
| 范围      | `WorkloadGroup`, `Principal`                | 应用限制的范围。      |
| LimitKind  | `ConcurrentRequests`, `ResourceUtilization` | 请求速率限制的类型。        |
| 属性 | 属性包                                | 请求速率限制的属性。      |

### <a name="concurrent-requests-rate-limit"></a>并发请求速率限制

类型 `ConcurrentRequests` 的请求速率限制包含以下属性：

| 名称                  | 类型 | 说明                                | 支持的值 |
|-----------------------|------|--------------------------------------------|------------------|
| MaxConcurrentRequests | int  | 最大并发请求数。 | [`1`, `10000`]   |

由于此策略而被拒绝的命令将引发 `ControlCommandThrottledException`（错误代码 = 429）。

由于此策略而被拒绝的查询将引发 `QueryThrottledException`（错误代码 = 429）。

### <a name="resource-utilization-rate-limit"></a>资源利用率速率限制

类型 `ResourceUtilization` 的请求速率限制包含以下属性：

| 名称           | 类型           | 说明     | 支持的值      |
|----------------|----------------|----------------|--------------|
| ResourceKind   | `ResourceKind` | 要限制的资源。 注意：`ResourceKind` 为 `TotalCpuSeconds` 时，将基于已完成的请求的 CPU 利用率的事后报告强制执行限制：执行将在定义的 `TimeWindow` 内（根据已完成的请求的报告）实现 `MaxUtilization` 后开始的请求将失败   。 | `RequestCount`, `TotalCpuSeconds` |
| MaxUtilization | `long`         | 可使用的资源的最大数量。    | [`1`, `9223372036854775807`]      |
| TimeWindow     | `timespan`     | 应用限制的滑动时间窗口。     | [`00:01:00`, `1.00:00:00`]        |

由于此策略而被拒绝的请求将引发 `QuotaExceededException`（错误代码 = 429）。

### <a name="example"></a>示例

以下策略最多允许：

* 500 个并发请求（适用于工作负荷组）。
* 每个主体 25 个并发请求。
* 每个主体每小时 50 个请求。

```json
[
  {
    "IsEnabled": true,
    "Scope": "WorkloadGroup",
    "LimitKind": "ConcurrentRequests",
    "Properties": {
      "MaxConcurrentRequests": 500
    }
  },
  {
    "IsEnabled": true,
    "Scope": "Principal",
    "LimitKind": "ConcurrentRequests",
    "Properties": {
      "MaxConcurrentRequests": 25
    }
  },
  {
    "IsEnabled": true,
    "Scope": "Principal",
    "LimitKind": "ResourceUtilization",
    "Properties": {
      "ResourceKind": "RequestCount",
      "MaxUtilization": 50,
      "TimeWindow": "01:00:00"
    }
  }
]
```

### <a name="the-default-workload-group"></a>`default` 工作负荷组

默认情况下，`default` 工作负荷组定义了以下策略。 此策略可修改。

```json
[
  {
    "IsEnabled": true,
    "Scope": "WorkloadGroup",
    "LimitKind": "ConcurrentRequests",
    "Properties": {
      "MaxConcurrentRequests": < Cores-Per-Node x 10 >
    }
  }
]
```

#### <a name="notes"></a>注释

* 对 `default` 工作负荷组的最大并发请求数的限制取决于群集的 SKU，计算方法如下：`Cores-Per-Node x 10`。
  * 例如，使用 Azure D14_v2 节点设置的群集（其中每个节点有 16 个 Vcore）默认限制为 `16` x `10` = `160`。
* 如果工作负荷组中对定义的最大并发请求数没有限制，则应用允许的最大值 `10000`。
* 更改 `default` 工作负荷组的策略时，必须为工作负荷组的最大并发请求数定义限制。
* 群集的[容量策略](capacitypolicy.md)还可能会限制属于特定类别的请求的请求速率，例如引入。
  * 如果超出[容量策略](capacitypolicy.md)或请求速率限制策略定义的任何一种限制，则将限制控制命令。
* 当应用类型 `ConcurrentRequests` 的请求速率限制时，[`.show capacity`](diagnostics.md#show-capacity) 的输出可能会根据这些限制而变化。
  * [`.show capacity`](diagnostics.md#show-capacity) 将根据请求的上下文、其分类到的工作负荷组以及其有效策略显示运行请求的主体的容量。
  * 运行命令时，如果它们的请求被分类到不同的工作负荷组，则不同的主体可能会看到不同的输出。
  * 运行 `.show capacity with(scope=cluster)` 时，将忽略请求上下文，并且输出仅受群集的[容量策略](capacitypolicy.md)的影响。

## <a name="control-commands"></a>控制命令

通过[工作负荷组控制命令](workload-groups-commands.md)管理工作负荷组的请求并发策略。
