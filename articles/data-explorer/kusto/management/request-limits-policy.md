---
title: 请求限制策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的请求限制策略。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 02/07/2021
ms.openlocfilehash: dfd0fb0b179b99b6b1f5ff7f7b05970448aba34a
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101090109"
---
# <a name="request-limits-policy-preview"></a>请求限制策略（预览）

工作负荷组的请求限制策略允许限制请求在其执行过程中使用的资源。

## <a name="the-policy-object"></a>策略对象

每个限制包括：

* 键入的 `Value` - 限制的值。
* `IsRelaxable` - 一个布尔值，作为请求的[客户端请求属性](../api/netfx/request-properties.md)的一部分，它定义了调用方是否可以放宽限制。

以下限制是可配置的：

| 名称   | 类型    | 说明      | 支持的值  | 匹配客户端请求属性       |
|-----------|------------|-----------|---------------------|--------------|
| DataScope     | `QueryDataScope` | 查询的数据范围 - 查询是应用于所有数据还是仅应用于数据的“热”部分。   | `All`、`HotCache` 或 `null`     | `query_datascope`      |
| MaxMemoryPerQueryPerNode   | `long`  | 查询可分配的最大内存量。    | [`1`，单节点总 RAM 的 50%] | `max_memory_consumption_per_query_per_node` |
| MaxMemoryPerIterator       | `long`    | 查询运算符可分配的最大内存量。  | [`1`，单节点总 RAM 的 50%] | `maxmemoryconsumptionperiterator`   |
| MaxFanoutThreadsPercentage | `int`   | 每个节点上要扇出查询执行的线程的百分比。 如果设置为 100%，则群集将分配每个节点上的所有 CPU。 例如，Azure D14_v2 节点上部署的某个群集上的 16 个 CPU。 | [`1`, `100`]   | `query_fanout_threads_percent` |
| MaxFanoutNodesPercentage   | `int`     | 群集上要扇出查询执行的节点的百分比。 函数的使用方式类似于 `MaxFanoutThreadsPercentage`。    | [`1`, `100`]                              |  `query_fanout_nodes_percent`               |
| MaxResultRecords           | `long`     | 允许请求返回到调用方的最大记录数，超过该数目的结果将被截断。    | [`1`, `9223372036854775807`]   | `truncationmaxrecords`  |
| MaxResultBytes     | `long`           | 允许请求返回到调用方的最大数据大小（以字节为单位），超过该数目的结果将被截断。  | [`1`, `9223372036854775807`]    | `truncationmaxsize`    |
| MaxExecutionTime     | `timespan`   | 请求可以运行的最长时间。  | (`00:00:00`, `01:00:00`]   | `servertimeout`    |

### <a name="notes"></a>注释

* 未定义或定义为 `null` 的限制，取自 `default` 工作负荷组的请求限制策略。
* 更改 `default` 工作负荷组的策略时，必须定义一个限制，并将其设置为非 `null` 值。
* 为了实现后向兼容性：对于分类到 `default` 工作负荷组的导出命令或从查询引入的命令（例如 `.set-or-append` 和 `.set-or-replace`），将禁用请求限制，并且不应用策略中设置的限制。
  * 但是，如果这些命令分类到非默认的工作负荷组，则将应用策略中的限制。

### <a name="example"></a>示例

```json
{
  "DataScope": {
    "IsRelaxable": true,
    "Value": "HotCache"
  },
  "MaxMemoryPerQueryPerNode": {
    "IsRelaxable": true,
    "Value": 2684354560
  },
  "MaxMemoryPerIterator": {
    "IsRelaxable": true,
    "Value": 2684354560
  },
  "MaxFanoutThreadsPercentage": {
    "IsRelaxable": true,
    "Value": 50
  },
  "MaxFanoutNodesPercentage": {
    "IsRelaxable": true,
    "Value": 50
  },
  "MaxResultRecords": {
    "IsRelaxable": true,
    "Value": 1000
  },
  "MaxResultBytes": {
    "IsRelaxable": true,
    "Value": 33554432
  },
  "MaxExecutiontime": {
    "IsRelaxable": true,
    "Value": "00:01:00"
  }
}
```

### <a name="the-default-workload-group"></a>`default` 工作负荷组

默认情况下，`default` 工作负荷组定义了以下策略。 此策略可修改。

```json
{
  "DataScope": {
    "IsRelaxable": true,
    "Value": "All"
  },
  "MaxMemoryPerQueryPerNode": {
    "IsRelaxable": true,
    "Value": < 50% of a single node's total RAM >
  },
  "MaxMemoryPerIterator": {
    "IsRelaxable": true,
    "Value": 5368709120
  },
  "MaxFanoutThreadsPercentage": {
    "IsRelaxable": true,
    "Value": 100
  },
  "MaxFanoutNodesPercentage": {
    "IsRelaxable": true,
    "Value": 100
  },
  "MaxResultRecords": {
    "IsRelaxable": true,
    "Value": 500000
  },
  "MaxResultBytes": {
    "IsRelaxable": true,
    "Value": 67108864
  },
  "MaxExecutiontime": {
    "IsRelaxable": true,
    "Value": "00:04:00"
  }
}
```

## <a name="control-commands"></a>控制命令

通过[工作负荷组控制命令](workload-groups-commands.md)管理工作负荷组的请求限制策略。
