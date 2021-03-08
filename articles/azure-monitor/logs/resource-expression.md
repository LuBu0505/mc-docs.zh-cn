---
title: Azure Monitor 日志查询中的 resource() 表达式 | Microsoft Docs
description: resource 表达式在以资源为中心的 Azure Monitor 日志查询中使用，可以从多个资源检索数据。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 02/20/2021
ms.openlocfilehash: 34eb0a0012333a1b8e85c1886303c2430e28c883
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102205658"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure Monitor 日志查询中的 resource() 表达式

`resource` 表达式在[范围限定为资源](scope.md#query-scope)的 Azure Monitor 查询中使用，可以从其他资源检索数据。 


## <a name="syntax"></a>语法

`resource(`标识符`)`

## <a name="arguments"></a>参数

- 标识符：资源的资源 ID。

| 标识符 | 说明 | 示例
|:---|:---|:---|
| 资源 | 包括资源的数据。 | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm") |
| 资源组或订阅 | 包括资源及其包含的所有资源的数据。  | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>注释

* 你必须具有该资源的读取访问权限。


## <a name="examples"></a>示例

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>后续步骤

- 有关查询范围的详细信息，请参阅 [Azure Monitor Log Analytics 中的日志查询范围和时间范围](scope.md)。
- 访问有关 [Kusto 查询语言](https://docs.microsoft.com/azure/kusto/query/)的完整文档。

