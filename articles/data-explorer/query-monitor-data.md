---
title: 使用 Azure 数据资源管理器（预览版）查询 Azure Monitor 中的数据
description: 在本主题中，通过跨产品查询创建 Azure 数据资源管理器，在 Azure Monitor（Application Insights 和 Log Analytics）中查询数据。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: how-to
ms.date: 03/19/2021
ms.localizationpriority: high
ms.openlocfilehash: 19768c304c41dbc5e7f461d289b8c60736c19cfd
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767662"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>使用 Azure 数据资源管理器（预览版）查询 Azure Monitor 中的数据

Azure 数据资源管理器支持在 Azure 数据资源管理器、[Application Insights (AI)](/azure-monitor/app/app-insights-overview) 和 [Log Analytics (LA)](/azure-monitor/platform/data-platform-logs)之间跨服务查询。 你可使用 Azure 数据资源管理器查询工具和在跨服务查询中查询 Log Analytics 或 Application Insights 工作区。 本文介绍如何创建跨服务查询，以及如何将 Log Analytics 或 Application Insights 工作区添加到 Azure 数据资源管理器 Web UI。

Azure 数据资源管理器跨服务查询流：

![Azure 数据资源管理器代理流](./media/query-monitor-data/query-monitor-workflow.png)

> [!NOTE]
> * 现可通过直接使用 Azure 数据资源管理器客户端工具，或者通过在 Azure 数据资源管理器群集上运行查询以间接方式从 Azure 数据资源管理器中查询 Azure Monitor 数据 - 此查询功能现处于预览版模式。
>* 若要获取帮助，请联系[跨服务查询团队](mailto:adxproxy@microsoft.com)。


## <a name="add-a-log-analyticsapplication-insights-workspace-to-azure-data-explorer-client-tools"></a>将 Log Analytics/Application Insights 工作区添加到 Azure 数据资源管理器客户端工具

将 Log Analytics 或 Application Insights 工作区添加到 Azure 数据资源管理器客户端工具，来实现对群集的跨服务查询。

1. 在连接到 Log Analytics 或 Application Insights 群集之前，请先确认 Azure 数据资源管理器本机群集（例如 *help* 群集）是否显示在左侧菜单中。

    ![Azure 数据资源管理器本机群集](./media/query-monitor-data/web-ui-help-cluster.png)

1. 在 Azure 数据资源管理器 UI (https://dataexplorer.azure.cn/clusters) 中，选择“添加群集”。 

1. 在“添加群集”窗口中，添加 LA 或 AI 群集的 URL。

    * 对于 LA：`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
    * 对于 AI：`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

1. 选择“添加”   。

    ![添加群集](./media/query-monitor-data/add-cluster.png)

    >[!TIP]
    >如果将连接添加到多个 Log Analytics 或 Application Insights 工作区，请为每个工作区指定不同的名称。 否则它们全部以相同的名称显示在左窗格。

1. 建立连接后，Log Analytics 或 Application Insights 工作区将随同 Azure 数据资源管理器本机群集显示在左侧窗格。

    ![Log Analytics 和 Azure 数据资源管理器群集](./media/query-monitor-data/la-adx-clusters.png)

> [!NOTE]
> 可映射的 Azure Monitor 工作区数限制为 100 个。

## <a name="run-queries"></a>运行查询

可使用支持 Kusto 查询的客户端工具来运行查询，这些工具包括：Kusto 资源管理器、Azure 数据资源管理器 Web UI、Jupyter Kqlmagic、Flow、PowerQuery、PowerShell、Lens、REST API。

> [!NOTE]
> 跨服务查询功能仅用于数据检索。 有关详细信息，请参阅[函数可支持性](#function-supportability)。

> [!TIP]
> * 数据库应与跨服务查询中指定的资源同名。 名称区分大小写。
> * 在跨服务查询中，请确保 Application Insights 应用和 Log Analytics 工作区命名正确。
> * 如果名称包含特殊字符，这些字符会在跨服务查询中替换为 URL 编码。
> * 如果名称包含的字符不符合 [KQL 标识符命名规则](kusto/query/schema-entities/entity-names.md)，这些字符将由短划线 **-** 字符替换。

### <a name="direct-query-on-your-log-analytics-or-application-insights-workspaces-from-azure-data-explorer-client-tools"></a>通过 Azure 数据资源管理器客户端工具直接查询 Log Analytics 或 Application Insights 工作区

可通过 Azure 数据资源管理器客户端工具对 Log Analytics 或 Application Insights 工作区运行查询。 

1. 验证是否已在左侧窗格中选择你的工作区。

1. 运行以下查询：

```kusto
Perf | take 10 // Demonstrate cross-service query on the Log Analytics workspace
```

![查询 Log Analytics 工作区](./media/query-monitor-data/query-la.png)

### <a name="cross-query-of-your-log-analytics-or-application-insights-workspace-and-the-azure-data-explorer-native-cluster"></a>交叉查询 Log Analytics/Application Insights 工作区和 Azure 数据资源管理器本机群集

运行跨群集服务查询时，请验证是否已在左侧窗格中选择 Azure 数据资源管理器本机群集。 以下示例演示了如何（使用 `union`）将 Azure 数据资源管理器群集表与 Log Analytics 工作区组合在一起。

运行以下查询：

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <ADX table>, cluster(CL1).database(<workspace-name>).<table name>
```

   [ ![从 Azure 数据资源管理器跨服务查询](./media/query-monitor-data/cross-query.png)](./media/query-monitor-data/cross-query.png#lightbox)

> [!TIP]
> 如果使用 [`join` 运算符](kusto/query/joinoperator.md)而不是 union，那么可能需要 [`hint`](kusto/query/joinoperator.md#join-hints) 才能在 Azure 数据资源管理器本机群集上运行它。

### <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>将一个租户的 Azure 数据资源管理器群集中的数据与另一个租户的 Azure Monitor 资源联接

不支持在服务之间跨服务查询。 你已登录到单个租户，以跨两个资源运行查询。

如果 Azure 数据资源管理器资源位于租户“A”中，而 Log Analytics 工作区位于租户“B”中，请使用以下方法：

1. 通过 Azure 数据资源管理器，可以为不同租户中的主体添加角色。 在 Azure 数据资源管理器群集上将用户 ID 作为授权用户添加到租户“B”中。 验证 Azure 数据资源管理器群集上的[“TrustedExternalTenant”](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)属性是否包含租户“B”。 在租户“B”中完全运行交叉查询。
 
### <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>从不同租户连接到 Azure 数据资源管理器群集

Kusto Explorer 会自动将你登录到用户帐户最初所属的租户。 若要使用同一用户帐户访问其他租户中的资源，必须在以下连接字符串中显式指定 `tenantId`：`Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=`TenantId

## <a name="function-supportability"></a>函数可支持性

Azure 数据资源管理器跨服务查询支持同时适用于 Application insights 和 Log Analytics 的函数。
此功能允许跨群集查询直接引用 Azure Monitor 表格函数。
跨服务查询支持下列命令：

* `.show functions`
* `.show function {FunctionName}`
* `.show database {DatabaseName} schema as json`

下图描述了一个从 Azure 数据资源管理器 Web UI 查询表格函数的示例。
若要使用此函数，请在“查询”窗口中运行名称。

  [ ![从 Azure 数据资源管理器 Web UI 查询表格函数](./media/query-monitor-data/function-query.png)](./media/query-monitor-data/function-query.png#lightbox)

## <a name="additional-syntax-examples"></a>其他语法示例

调用 Application Insights 或 Log Analytics 群集时，可使用以下语法选项：

|语法说明  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| 仅包含此订阅中所定义资源的群集中的数据库（**建议用于跨群集查询**） |   cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| 包含此订阅中所有应用/工作区的群集    |     cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|包含订阅中所有应用/工作区且属于此资源组的群集    |   cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|仅包含此订阅中定义的资源的群集      |    cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

## <a name="next-steps"></a>后续步骤

[编写查询](write-queries.md)
