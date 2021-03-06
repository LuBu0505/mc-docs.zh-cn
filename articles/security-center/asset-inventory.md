---
title: Azure 安全中心的资产库存
description: 了解 Azure 安全中心的资产管理体验，该体验提供对所有受安全中心监视的资源的完全可见性。
author: Johnnytechn
manager: rkarlin
services: security-center
ms.author: v-johya
ms.date: 01/06/2021
ms.service: security-center
ms.topic: how-to
ms.openlocfilehash: e244a80c514f881bb88e5426fa0bc270ccc1377d
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023062"
---
# <a name="explore-and-manage-your-resources-with-asset-inventory"></a>使用资产清单浏览和管理资源

Azure 安全中心的资产清单页提供了一个页面，用于查看已连接到安全中心的资源的安全状况。 

安全中心会定期分析 Azure 资源的安全状态，以识别潜在的安全漏洞。 然后会提供有关如何消除这些安全漏洞的建议。

当任何资源具有未完成的建议时，它们将显示在清单中。

使用此视图及其筛选器可以解决以下这类问题：

- 我的哪些启用了 Azure Defender 的订阅具有重要建议？
- 我的哪些标记为“生产”的计算机缺少 Log Analytics 代理？
- 我的多少台标记有特定标记的计算机具有重要建议？
- 特定资源组中有多少资源具有漏洞评估服务提供的安全结果？

此工具的资产管理可能性很大，并且还在不断增长。 

> [!TIP]
> 资产库存页面上的安全建议与“建议”页面上的安全建议相同，但资产库存页面会根据受影响的资源进行显示。 有关如何解决建议的详细信息，请参阅[在 Azure 安全中心实施安全建议](security-center-recommendations.md)。


## <a name="availability"></a>可用性

|方面|详细信息|
|----|:----|
|发布状态：|正式发布 (GA)|
|定价：|免费|
|所需角色和权限：|所有用户|
|云：|![是](./media/icons/yes-icon.png) 中国云|
|||


## <a name="what-are-the-key-features-of-asset-inventory"></a>资产库存的主要功能是什么？

库存页提供以下工具：

- 摘要 - 在定义任何筛选器之前，会显示库存视图顶部突出的值条带：

    - **资源总数**：连接到安全中心的资源总数。
    - **不正常的资源**：具有有效安全建议的资源。 [详细了解安全建议](security-center-recommendations.md)。
    - **未受监视的资源**：有代理监视问题的资源 - 已部署 Log Analytics 代理，但代理没有发送数据或有其他运行状况问题。

- 筛选器 - 页面顶部的多个筛选器提供一种根据你尝试回答的问题快速优化资源列表的方法。
<!--Not available in MC: combine the **Agent monitoring** filter with the **Tags** filter-->
    应用筛选器后，摘要值就会更新为与查询结果相关的值。 

- 导出选项 - 库存提供了将所选筛选器选项的结果导出到 CSV 文件的选项。 此外，还可以将查询本身导出到 Azure Resource Graph 资源管理器，以进一步优化、保存或修改 Kusto 查询语言 (KQL) 查询。

    :::image type="content" source="./media/asset-inventory/inventory-export-options.png" alt-text="库存的导出选项":::

    > [!TIP]
    > KQL 文档为数据库提供一些示例数据以及一些简单的查询，以获取相应语言的体验。 [通过此 KQL 教程了解详细信息](/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer)。

- 资产管理选项 - 通过库存可以执行复杂的发现查询。 找到与查询匹配的资源后，库存将提供诸如以下操作的快捷方式：

    - 将标签分配给经过筛选的资源 - 选中要标记的资源旁边的复选框。
    - 在安全中心中加入新服务器 - 使用“添加非 Azure 服务器”工具栏按钮。
    - 使用 Azure 逻辑应用自动执行工作负载 - 使用“触发逻辑应用”按钮可在一个或多个资源上运行逻辑应用。 逻辑应用必须提前准备好，并接受相关的触发器类型（HTTP 请求）。 [详细了解逻辑应用](../logic-apps/logic-apps-overview.md)。


## <a name="how-does-asset-inventory-work"></a>资产库存的工作方式？

资产库存利用 [Azure Resource Graph (ARG)](../governance/resource-graph/index.yml)，这种 Azure 服务提供跨多个订阅查询安全中心的安全状况数据的功能。

ARG 用于提供高效资源探索，并具有大规模查询的功能。

资产库存可以使用 [Kusto 查询语言 (KQL)](/data-explorer/kusto/query/)，通过将 ASC 数据与其他资源属性进行交叉引用来快速生成深度见解。


## <a name="how-to-use-asset-inventory"></a>如何使用资产库存

1. 从安全中心的边栏选择“库存”。

1. 使用“按名称筛选”框可显示特定资源，或以如下所述的方式使用筛选器。

1. 在筛选器中选择相关选项，以创建要执行的特定查询。

    :::image type="content" source="./media/asset-inventory/inventory-filters.png" alt-text="库存筛选选项" lightbox="./media/asset-inventory/inventory-filters.png":::

    默认情况下，资源按有效安全建议的数量排序。

    > [!IMPORTANT]
    > 每个筛选器中的选项特定于当前选择的订阅中的资源和你在其他筛选器中的选择。
    >
    > 例如，如果你仅选择了一个订阅，并且该订阅没有要修正的具有重要安全建议的资源（0 个运行不正常的资源），则“建议”筛选器将没有选项。 

1. 若要使用“安全发现包含”筛选器，请通过漏洞发现的 ID、安全检查或 CVE 名称输入自由文本以筛选受影响的资源：

    ![“安全发现包含”筛选器](./media/asset-inventory/security-findings-contain-elements.png)

    > [!TIP]
    > “安全发现包含”和“标记”筛选器仅接受一个值 。 若要使用多个筛选器，请使用“添加筛选器”。

1. 若要使用“Azure Defender”筛选器，请选择一个或多个选项（“关”、“开”或“部分”）：

    - 关 - 不受 Azure Defender 计划保护的资源。 可以右键单击其中任意一些资源并对其进行升级：

        :::image type="content" source="./media/asset-inventory/upgrade-resource-inventory.png" alt-text="通过右键单击将资源升级到 Azure Defender" lightbox="./media/asset-inventory/upgrade-resource-inventory.png":::

    - 开 - 受 Azure Defender 计划保护的资源
    - 部分 - 此选项应用于禁用了某些（但不是全部）Azure Defender 计划的订阅 。 例如，以下订阅已禁用五个 Azure Defender 计划。 

        :::image type="content" source="./media/asset-inventory/pricing-tier-partial.png" alt-text="部分开启 Azure Defender 的订阅":::

1. 若要进一步检查查询结果，请选择你感兴趣的资源。

1. 若要在 Resource Graph Explorer 中以查询的形式查看当前选定的筛选器选项，请选择“打开查询”。

    ![ARG 中的库存查询](./media/asset-inventory/inventory-query-in-resource-graph-explorer.png)

1. 运行先前定义的逻辑应用 

1. 如果已经定义了一些筛选器并使页面保持打开状态，则安全中心不会自动更新结果。 除非手动重新加载页面或选择“刷新”，否则对资源的任何更改都不会影响显示的结果。


## <a name="faq---inventory"></a>常见问题解答 - 库存

### <a name="why-arent-all-of-my-subscriptions-machines-storage-accounts-etc-shown"></a>为什么未显示我的所有订阅、计算机、存储帐户等？

库存视图从云安全状况管理 (CSPM) 角度列出了安全中心连接的资源。 筛选器不会返回你环境中的所有资源；只会返回那些具有重要（或“有效”）建议的资源。 

例如，以下屏幕截图显示了一个有权访问 38 个订阅但只有 10 个订阅现在有建议的用户。 因此，当它们按“资源类型 = 订阅”进行筛选时，库存中仅显示具有有效建议的那 10 个订阅：

:::image type="content" source="./media/asset-inventory/filtered-subscriptions-some.png" alt-text="在没有有效建议的情况下，并非所有子项都返回":::

### <a name="why-do-some-of-my-resources-show-blank-values-in-the-azure-defender-or-agent-monitoring-columns"></a>为什么我的一些资源在 Azure Defender 或代理监视列中显示空值？

<!--Customized-->
并非所有受安全中心监视的资源都有代理。 例如，Azure 存储帐户或 PaaS 资源（如磁盘）。

当定价或代理监视与资源无关时，库存的这些列中将不会显示任何内容。

:::image type="content" source="./media/asset-inventory/agent-pricing-blanks.png" alt-text="某些资源在代理监视或 Azure Defender 列中显示空白信息":::

## <a name="next-steps"></a>后续步骤

本文介绍 Azure 安全中心的资产库存页面。

有关相关工具的详细信息，请参阅以下页面：

- [Azure Resource Graph (ARG)](../governance/resource-graph/index.yml)
- [Kusto 查询语言 (KQL)](/data-explorer/kusto/query/)

