---
title: 请求分类策略管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中请求分类策略的管理命令。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 02/07/2021
ms.openlocfilehash: d1bb66f1886c4aac61959e5cf022e3c123851c13
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101090111"
---
# <a name="request-classification-policy-preview---control-commands"></a>请求分类策略（预览）- 控制命令

这些命令需要 [AllDatabasesAdmin](access-control/role-based-authorization.md) 权限。

## <a name="alter-cluster-policy-request_classification"></a>.alter cluster policy request_classification

更改群集的请求分类策略。

### <a name="syntax"></a>语法

`.alter` `cluster` `policy` `request_classification` `"`序列化部分策略`"` `<|` 分类函数主体 

### <a name="examples"></a>示例

#### <a name="set-a-policy-with-multiple-workload-groups"></a>设置具有多个工作负荷组的策略

```kusto
.alter cluster policy request_classification '{"IsEnabled":true}' <|
    case(current_principal_is_member_of('aadgroup=somesecuritygroup@contoso.com'), "First workload group",
         request_properties.current_database == "MyDatabase" and request_properties.current_principal has 'aadapp=', "Second workload group",
         request_properties.current_application == "Kusto.Explorer" and request_properties.request_type == "Query", "Third workload group",
         request_properties.current_application == "KustoQueryRunner", "Fourth workload group",
         request_properties.request_description == "this is a test", "Fifth workload group",
         hourofday(now()) between (17 .. 23), "Sixth workload group",
         "default")
```

#### <a name="set-a-policy-with-a-single-workload-group"></a>设置只有一个工作负荷组的策略

```kusto
.alter cluster policy request_classification '{"IsEnabled":false}' <|
    iff(request_properties.current_application == "Kusto.Explorer" and request_properties.request_type == "Query",
        "Ad-hoc queries",
        "default")
```

## <a name="alter-merge-cluster-policy-request_classification"></a>.alter-merge cluster policy request_classification

启用或禁用群集的请求分类策略。

### <a name="syntax"></a>语法

`.alter-merge` `cluster` `policy` `request_classification` `"`序列化部分策略`"`

### <a name="examples"></a>示例

#### <a name="enable-the-policy"></a>启用策略

```kusto
.alter-merge cluster policy request_classification '{"IsEnabled":true}'
```

#### <a name="disable-the-policy"></a>禁用策略

```kusto
.alter-merge cluster policy request_classification '{"IsEnabled":false}'
```

## <a name="delete-cluster-policy-request_classification"></a>.delete cluster policy request_classification

删除群集的请求分类策略。

### <a name="syntax"></a>语法

`.delete` `cluster` `policy` `request_classification`

## <a name="show-cluster-policy-request_classification"></a>.show cluster policy request_classification

显示群集的请求分类策略。

### <a name="syntax"></a>语法

`.show` `cluster` `policy` `request_classification`
