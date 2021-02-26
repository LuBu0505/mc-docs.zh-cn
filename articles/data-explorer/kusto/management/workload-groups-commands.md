---
title: 工作负荷组管理 - Azure 数据资源管理器
description: 本文概述了 Azure 数据资源管理器中工作负荷组的管理命令。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 02/07/2021
ms.openlocfilehash: a901fdd7050b8dc3185e221f91e1d43b8ac9aed7
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101090104"
---
# <a name="workload-groups-preview---control-commands"></a>工作负荷组（预览）- 控制命令

这些命令需要 [AllDatabasesAdmin](access-control/role-based-authorization.md) 权限。

## <a name="create-or-alter-workload_group"></a>.create-or-alter workload_group

创建新的工作负荷组，或更改现有工作负荷组。

### <a name="syntax"></a>语法

`.create-or-alter` `workload_group` *WorkloadGroupName* `"`*序列化工作负荷组和策略*`"`

### <a name="examples"></a>示例

#### <a name="full-definition-of-request-limits-policy"></a>请求限制策略的完整定义

创建具有请求限制策略的完整定义的工作负荷组：

```kusto
.create-or-alter workload_group MyWorkloadGroup '{'
'  "RequestLimitsPolicy": {'
'    "DataScope": {'
'      "IsRelaxable": true,'
'      "Value": "HotCache"'
'    },'
'    "MaxMemoryPerQueryPerNode": {'
'      "IsRelaxable": false,'
'      "Value": 6442450944'
'    },'
'    "MaxMemoryPerIterator": {'
'      "IsRelaxable": false,'
'      "Value": 5368709120'
'    },'
'    "MaxFanoutThreadsPercentage": {'
'      "IsRelaxable": true,'
'      "Value": 100'
'    },'
'    "MaxFanoutNodesPercentage": {'
'      "IsRelaxable": true,'
'      "Value": 100'
'    },'
'    "MaxResultRecords": {'
'      "IsRelaxable": true,'
'      "Value": 500000'
'    },'
'    "MaxResultBytes": {'
'      "IsRelaxable": true,'
'      "Value": 67108864'
'    },'
'    "MaxExecutionTime": {'
'      "IsRelaxable": true,'
'      "Value": "00:04:00"'
'    }'
'  }'
'}'
```

#### <a name="full-definition-of-request-limits-policy-and-request-rate-limits-policies"></a>请求限制策略和请求速率限制策略的完整定义

创建具有请求限制策略和请求速率限制策略的完整定义的工作负荷组：

```kusto
.create-or-alter workload_group ['My Workload Group'] '{'
'  "RequestLimitsPolicy": {'
'    "DataScope": {'
'      "IsRelaxable": true,'
'      "Value": "All"'
'    },'
'    "MaxMemoryPerQueryPerNode": {'
'      "IsRelaxable": true,'
'      "Value": 6442450944'
'    },'
'    "MaxMemoryPerIterator": {'
'      "IsRelaxable": true,'
'      "Value": 5368709120'
'    },'
'    "MaxFanoutThreadsPercentage": {'
'      "IsRelaxable": true,'
'      "Value": 100'
'    },'
'    "MaxFanoutNodesPercentage": {'
'      "IsRelaxable": true,'
'      "Value": 100'
'    },'
'    "MaxResultRecords": {'
'      "IsRelaxable": true,'
'      "Value": 500000'
'    },'
'    "MaxResultBytes": {'
'      "IsRelaxable": true,'
'      "Value": 67108864'
'    },'
'    "MaxExecutionTime": {'
'      "IsRelaxable": true,'
'      "Value": "00:04:00"'
'    }'
'  },'
'  "RequestRateLimitPolicies": ['
'  {'
'      "IsEnabled": true,'
'      "Scope": "WorkloadGroup",'
'      "LimitKind": "ConcurrentRequests",'
'      "Properties": {'
'        "MaxConcurrentRequests": 100'
'      }'
'    },'
'    {'
'      "IsEnabled": true,'
'      "Scope": "Principal",'
'      "LimitKind": "ConcurrentRequests",'
'      "Properties": {'
'        "MaxConcurrentRequests": 25'
'      }'
'    }'
'  ]'
'}'
```

## <a name="alter-merge-workload_group"></a>.alter-merge workload_group

### <a name="syntax"></a>语法

`.alter-merge` `workload_group` *WorkloadGroupName* `"`*序列化部分工作负荷组和策略*`"`

### <a name="examples"></a>示例

#### <a name="add-title-for-this-example"></a>为此示例添加标题

更改 `default` 工作负荷组的请求限制策略中的特定限制，同时使以前定义的限制保持原样：

```kusto
.alter-merge workload_group default '{'
'  "RequestLimitsPolicy": {'
'    "DataScope": {'
'      "IsRelaxable": false,'
'      "Value": "HotCache"'
'    },'
'    "MaxExecutionTime": {'
'      "IsRelaxable": false,'
'      "Value": "00:01:00"'
'    }'
'  }'
'}'
```
#### <a name="add-title-for-this-example"></a>为此示例添加标题

更改 `default` 工作负荷组的请求速率限制策略，同时使其请求限制策略保持原样：

```kusto
.alter-merge workload_group default '{'
'  "RequestRateLimitPolicies": ['
'    {\n'
'      "IsEnabled": true,'
'      "Scope": "WorkloadGroup",'
'      "LimitKind": "ConcurrentRequests",'
'      "Properties": {'
'        "MaxConcurrentRequests": 100'
'      }'
'    }'
'  ]'
'}'
```

## <a name="drop-workload_group"></a>.drop workload_group

删除工作负荷组。

不能删除 `internal` 和 `default` 工作负荷组。

### <a name="syntax"></a>语法

`.drop` `workload_group` WorkloadGroupName

### <a name="examples"></a>示例

```kusto
.drop workload_group MyWorkloadGroup
```

```kusto
.drop workload_group ['MyWorkloadGroup']
```

## <a name="show-workload_group"></a>.show workload_group

显示特定或所有工作负荷组定义。

### <a name="syntax"></a>语法

`.show` `workload_group` WorkloadGroupName

`.show` `workload_groups`

### <a name="output"></a>输出

| ColumnName        | 数据类型 | 说明                                          |
|-------------------|----------|------------------------------------------------------|
| WorkloadGroupName | 字符串   | 工作负荷组的名称。                      |
| WorkloadGroup     | 字符串   | 工作负荷组策略的 JSON 序列化。 |

### <a name="example"></a>示例

```kusto
.show workload_group MyWorkloadGroup
```

| WorkloadGroupName  | WorkloadGroup                                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MyWorkloadGroup    | {"RequestRateLimitPolicies": [{"IsEnabled": true, "Scope": "WorkloadGroup", "LimitKind": "ConcurrentRequests", "Properties": {"MaxConcurrentRequests": 30}}]} |

## <a name="show-workload-groups-resources-utilization"></a>.show workload groups resources utilization

如果已定义请求速率限制，则显示每个工作负荷组和/或每个主题的当前资源使用率。

如果消耗的资源量等于零，则不会在结果集中包含记录。
例如，在工作负荷组中没有并发请求时。

### <a name="syntax"></a>语法

`.show` `workload_group` WorkloadGroupName `resources` `utilization`

`.show` `workload_groups` `resources` `utilization`

### <a name="output"></a>输出

| ColumnName        | ColumnType | 说明                                                                                                   |
|-------------------|------------|---------------------------------------------------------------------------------------------------------------|
| WorkloadGroupName | 字符串     | 工作负荷组的名称。                                                                               |
| 主体         | string     | 主体标识的完全限定名称（如果范围是工作负荷组，则为 `null`）。           |
| ResourceKind      | 字符串     | 跟踪的资源的类型（`ConcurrentRequests`、`TotalCpuSeconds` 或 `RequestCount`）。                |
| 容量          | long       | 跟踪的资源的容量，如请求速率限制策略中所定义。                            |
| 已耗用          | long       | 当前正在被使用的跟踪的资源量。                                          |
| TimeWindow        | timespan   | 跟踪资源的时间范围（如果 `ResourceKind` 是 `ConcurrentRequests`，则为 `null`）。 |
| MeasuredOn        | datetime   | 上次测量资源利用率时的 UTC 日期和时间。                                    |

### <a name="example"></a>示例

```kusto
.show workload_group MyWorkloadGroup resources utilization
```

| WorkloadGroupName | 主体                                                                         | ResourceKind       | 容量 | 已耗用 | TimeWindow | MeasuredOn                  |
|-------------------|-----------------------------------------------------------------------------------|--------------------|----------|----------|------------|-----------------------------|
| MyWorkloadGroup   |                                                                                   | ConcurrentRequests | 30       | 25       |            | 2020-11-04 22:38:54.7256255 |
| MyWorkloadGroup   | aadapp=7929a76b-6f2d-49ef-9aa5-facaccbbf106;94918272-e999-45a6-81f1-85f0428dad53  | ConcurrentRequests | 25       | 19       |            | 2020-11-04 22:38:54.7256255 |
| MyWorkloadGroup   | aadapp=7929a76b-6f2d-49ef-9aa5-facaccbbf106;94918272-e999-45a6-81f1-85f0428dad53  | RequestCount       | 120      | 2        | 00:01:00   | 2020-11-04 22:38:54.0000000 |
| MyWorkloadGroup   | aadapp=7929a76b-6f2d-49ef-9aa5-facaccbbf106;94918272-e999-45a6-81f1-85f0428dad53  | TotalCpuSeconds    | 32500    | 32480    | 01:00:00   | 2020-11-04 22:38:54.0000000 |
| MyWorkloadGroup   | aaduser=e2056bdc-5448-4999-8b9b-1ebf9dd1e62b;94918272-e999-45a6-81f1-85f0428dad53 | ConcurrentRequests | 25       | 6        |            | 2020-11-04 22:38:53.4456894 |
| MyWorkloadGroup   | aaduser=e2056bdc-5448-4999-8b9b-1ebf9dd1e62b;94918272-e999-45a6-81f1-85f0428dad53 | RequestCount       | 120      | 15       | 00:01:00   | 2020-11-04 22:38:54.0000000 |
| MyWorkloadGroup   | aaduser=e2056bdc-5448-4999-8b9b-1ebf9dd1e62b;94918272-e999-45a6-81f1-85f0428dad53 | TotalCpuSeconds    | 32500    | 22584    | 01:00:00   | 2020-11-04 22:38:54.0000000 |

## <a name="example"></a>示例

此示例将执行下列步骤：

1. 创建名为 `My Workload Group` 的工作负荷组。
1. 创建一个 `request_classification` 策略，通过以下特征将请求分类为 `My Workload Group`：
    * 请求为查询。
    * 当前主体是 AAD 用户，并且是 AAD 组 `MyGroup@contoso.com` 的成员。
    * 当前应用程序名为 `Kusto.Explorer`。
    * 当前数据库名为 `My Database`。
1. 将以下请求限制应用到分类为 `My Workload Group` 的请求：
    * 默认数据范围：热缓存（调用方不能放宽客户端请求属性中的限制）。
    * 结果集中的最大记录数：100,000（调用方可以放宽客户端请求属性中的限制）。
    * 结果集的最大大小：50 MB（调用方可以放宽客户端请求属性中的限制）。
    * 最大执行时间：1 分钟（调用方不能放宽客户端请求属性中的限制）。
1. 将以下请求速率限制应用到分类为 `My Workload Group` 的请求：
    * 并发请求的最大数目：10。
    * 每个主体的并发请求的最大数目：3。
    * 每个主体每分钟的请求总数：12。

任何其他请求分类到 `default` 工作负荷组。
未在 `My Workload Group` 策略中定义的请求限制从 `default` 工作负荷组的策略中获取。

```kusto
.alter cluster policy request_classification '{"IsEnabled":true}' <|
    case(current_principal_is_member_of("aadgroup=MyGroup@microsoft.com") and
         request_properties.current_database == "My Database" and
         request_properties.current_application == "Kusto.Explorer" and
         request_properties.current_principal startswith 'aaduser=' and
         request_properties.request_type == 'Query', "My Workload Group",
         "default")
```

```kusto
.create-or-alter workload_group ['My Workload Group'] '{'
'  "RequestLimitsPolicy": {'
'    "DataScope": {'
'      "IsRelaxable": false,'
'      "Value": "HotCache"'
'    },'
'    "MaxResultRecords": {'
'      "IsRelaxable": true,'
'      "Value": 100000'
'    },'
'    "MaxResultBytes": {'
'      "IsRelaxable": true,'
'      "Value": 52428800'
'    },'
'    "MaxExecutionTime": {'
'      "IsRelaxable": false,'
'      "Value": "00:01:00"'
'    }'
'  },'
'  "RequestRateLimitPolicies": ['
'    {'
'      "IsEnabled": true,'
'      "Scope": "WorkloadGroup",'
'      "LimitKind": "ConcurrentRequests",'
'      "Properties": {'
'        "MaxConcurrentRequests": 10'
'      }'
'    },'
'    {'
'      "IsEnabled": true,'
'      "Scope": "Principal",'
'      "LimitKind": "ConcurrentRequests",'
'      "Properties": {'
'        "MaxConcurrentRequests": 3'
'      }'
'    },'
'    {'
'      "IsEnabled": true,'
'      "Scope": "Principal",'
'      "LimitKind": "ResourceUtilization",'
'      "Properties": {'
'        "ResourceKind": "RequestCount",'
'        "MaxUtilization": 12,'
'        "TimeWindow": "00:01:00"'
'      }'
'    }'
'  ]'
'}'
```
