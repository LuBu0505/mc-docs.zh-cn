---
title: Azure 安全中心内的安全建议 | Azure
description: 本文档介绍 Azure 安全中心中的建议如何帮助你保护 Azure 资源并保持符合安全策略。
services: security-center
documentationcenter: na
author: Johnnytechn
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 07/29/2019
ms.date: 02/25/2021
ms.author: v-johya
ms.openlocfilehash: f0f9568f6e477bfc151c9d37c5ab3316dc0fb6fd
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197496"
---
# <a name="security-recommendations-in-azure-security-center"></a>Azure 安全中心的安全建议 

本主题说明如何查看和了解 Azure 安全中心内的建议，以帮助你保护 Azure 资源。


## <a name="what-are-security-recommendations"></a>安全建议是什么？

安全中心会定期分析 Azure 资源的安全状态，以识别潜在的安全漏洞。 然后会提供有关如何消除这些安全漏洞的建议。

建议是为了保护和强化资源而要采取的措施。 

每项建议都提供：

- 问题的简短说明
- 为实施建议而要执行的修正步骤
- 受影响的资源

## <a name="how-does-microsoft-decide-what-needs-securing-and-hardening"></a>Microsoft 如何判定需要保护和强化哪些内容？

安全中心的建议以 Azure 安全基准为基础。 几乎每条建议都有一个基础策略，该策略源自基准中的需求。

Azure 安全基准是由 Microsoft 创作的特定于 Azure 的一组准则，适用于基于常见合规框架的安全与合规最佳做法。 这一公认的基准建立在 [Internet 安全中心 (CIS)](https://www.cisecurity.org/benchmark/azure/) 和[国家标准与技术研究院 (NIST)](https://www.nist.gov/) 的控制基础上，重点关注以云为中心的安全性。 详细了解 [Azure 安全基准](../security/benchmarks/introduction.md)。

查看建议的详细信息时，能够查看基础策略通常会很有帮助。 对于策略支持的每条建议，请使用建议详细信息页中的“查看策略定义”链接直接进入相关策略的 Azure 策略条目：

:::image type="content" source="media/release-notes/view-policy-definition.png" alt-text="链接到 Azure Policy 页面，了解支持建议的特定策略":::

使用此链接可查看策略定义和计算逻辑。 

如果查看[安全建议参考指南](recommendations-reference.md)上的建议列表，你还将看到指向策略定义页面的链接：

:::image type="content" source="media/release-notes/view-policy-definition-from-documentation.png" alt-text="直接从 Azure 安全中心建议参考页访问 Azure Policy 页面来了解特定策略":::

<!--Customized in MC-->
## <a name="monitor-recommendations"></a>监视建议 <a name="monitor-recommendations"></a>

安全中心将分析资源的安全状态，以识别潜在的漏洞。 

1. 在安全中心的菜单中，打开“建议”页，查看适用于你的环境的建议。 建议会被分组到各项安全控制中。

    :::image type="content" source="./media/security-center-recommendations/view-recommendations.png" alt-text="建议会按安全控制分组" lightbox="./media/security-center-recommendations/view-recommendations.png":::

1. 若要查找特定于资源类型、严重性、环境或其他对你很重要的条件的建议，请使用建议列表上方的可选筛选器。

    :::image type="content" source="media/security-center-recommendations/recommendation-list-filters.png" alt-text="用于优化 Azure 安全中心建议列表的筛选器":::

1. 展开一项控制并选择特定的建议，以查看建议详细信息页。

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="建议详细信息页。" lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    该页面包括：

    1. 对于支持的建议，顶部工具栏将显示以下任意或所有按钮：
       - **查看策略定义**，用于直接进入基础策略的 Azure 策略条目
    1. 严重性指标
    1. 刷新间隔（如果相关） 
    1. 描述 - 问题简述
    1. 修正步骤 - 修正受影响资源的安全问题时所需的手动步骤的说明。 对于带有“快速修复”的建议，可以先选择“查看修正逻辑”，然后再为资源应用建议的修补程序。 
    1. 受影响的资源 - 资源会分组到不同的选项卡中：
        - 正常资源 - 相关的资源，这些资源要么未受影响，要么已经修正了问题。
        - 不正常的资源 - 已标识的问题仍会影响的资源。
        - 不适用的资源 - 建议无法为其提供明确答案的资源。 “不适用”选项卡还会为每个资源提供原因。 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="不适用的资源及其原因。":::
    1. 用于修正建议或触发逻辑应用的操作按钮。

## <a name="preview-recommendations"></a>预览建议

计算安全分数时不包括标记为“预览”的建议。

仍应尽可能按这些建议修正，以便在预览期结束时，它们会有助于提升评分。

预览建议示例如下：

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="带有预览标志的建议":::
 
## <a name="next-steps"></a>后续步骤

在本文档中，已向你介绍安全中心的安全建议。 如需相关信息：

- [修正建议](security-center-remediate-recommendations.md) -- 了解如何为 Azure 订阅和资源组配置安全策略。
- [自动执行对安全中心触发器的响应](workflow-automation.md) -- 自动响应建议
- [安全建议 - 参考指南](recommendations-reference.md)

