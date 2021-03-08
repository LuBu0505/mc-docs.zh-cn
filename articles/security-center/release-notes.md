---
title: Azure 安全中心的发行说明
description: 介绍 Azure 安全中心的新增功能和已更改的功能
services: security-center
documentationcenter: na
author: Johnnytechn
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2021
ms.author: v-johya
ms.openlocfilehash: bfd5440b06dbe26ae80319d57441536f493872c9
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197636"
---
# <a name="whats-new-in-azure-security-center"></a>Azure 安全中心的新增功能

安全中心正在积极开发中，并不断得到改进。 为及时了解最新开发成果，此页提供了有关新功能、bug 修复和已弃用功能的信息。

## <a name="september-2020"></a>2020 年 9 月

9 月的更新包括：
- [安全中心获得新的外观！](#security-center-gets-a-new-look)
- [Azure Defender 已发布](#azure-defender-released)
- [资产清单工具现已正式发布](#asset-inventory-tools-are-now-generally-available)
- [Kubernetes 工作负载保护建议捆绑](#kubernetes-workload-protection-recommendation-bundle)
- [漏洞评估发现结果现已可以连续导出](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [优化了网络安全组建议](#network-security-group-recommendations-improved)
- [弃用了预览 AKS 建议“应在 Kubernetes 服务上定义 Pod 安全策略”](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [优化了 Azure 安全中心内的电子邮件通知](#email-notifications-from-azure-security-center-improved)
- [安全功能分数不包括预览建议](#secure-score-doesnt-include-preview-recommendations)
- [建议现在包含严重性指标和新鲜度间隔](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)


### <a name="security-center-gets-a-new-look"></a>安全中心获得新的外观！

我们已发布安全中心门户页面的更新 UI。 新页面包括新的概述页面以及用于安全分数、资产清单和 Azure Defender 的面板。

重新设计的概述页面现在包含一个磁贴，用于访问安全分数、资产清单和 Azure Defender 的面板。 它还包含一个可以链接到合规性面板的磁贴。

详细了解[概述页面](overview-page.md)。


### <a name="azure-defender-released"></a>Azure Defender 已发布

Azure Defender 是集成到安全中心内部的云工作负载保护平台 (CWPP)，用于为 Azure 和混合工作负载提供高级智能的保护。 它取代了安全中心的标准定价层选项。 

从 Azure 安全中心的“定价和设置”区域启用 Azure Defender 时，将同时启用以下 Defender 计划，并为环境的计算、数据和服务层提供全面防护：

- [适用于服务器的 Azure Defender](defender-for-servers-introduction.md)
- [Azure Defender for SQL](defender-for-sql-introduction.md)
- [适用于 Kubernetes 的 Azure Defender](defender-for-kubernetes-introduction.md)
- [适用于容器注册表的 Azure Defender](defender-for-container-registries-introduction.md)

安全中心的文档对其中每个计划单独进行了介绍。

借助其专用面板，Azure Defender 为虚拟机、SQL 数据库、容器、Web 应用程序、网络等提供安全警报和高级威胁防护。

[详细了解 Azure Defender](azure-defender.md)

### <a name="asset-inventory-tools-are-now-generally-available"></a>资产清单工具现已正式发布

Azure 安全中心的资产清单页提供了一个页面，用于查看已连接到安全中心的资源的安全状况。

安全中心会定期分析 Azure 资源的安全状态，以识别潜在的安全漏洞。 然后会提供有关如何消除这些安全漏洞的建议。

当任何资源具有未完成的建议时，它们将显示在清单中。

有关详细信息，请参阅[利用资产清单浏览和管理资源](asset-inventory.md)。



### <a name="kubernetes-workload-protection-recommendation-bundle"></a>Kubernetes 工作负载保护建议捆绑

为了确保 Kubernetes 工作负载在默认情况下是安全的，安全中心将添加 Kubernetes 级别强化建议，其中包括具有 Kubernetes 准入控制的执行选项。

在 AKS 群集上安装了适用于 Kubernetes 的 Azure Policy 加载项后，将按照预先定义的一组最佳做法监视对 Kubernetes API 服务器的每个请求，然后再将其保存到群集。 然后，可以配置为强制实施最佳做法，并规定将其用于未来的工作负载。

例如，可以规定不应创建特权容器，并且阻止以后的任何请求。

有关详细信息，请参阅[使用 Kubernetes 准入控制实现工作负载保护最佳做法](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)。


### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>漏洞评估发现结果现已可以连续导出

使用连续导出将警报和建议实时流式传输到 Azure 事件中心、Log Analytics 工作区或 Azure Monitor。 在那里，你可以将此数据与 SIEM（例如 Power BI、Azure 数据资源管理器等）集成。

安全中心的集成漏洞评估工具返回资源结果，并将其作为“父”建议中的可操作建议（例如“应修正虚拟机中的漏洞”）。 

现在选择建议并启用“包括安全性结果”选项时，可以通过连续导出来导出安全性结果。

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="在连续导出配置中包括安全结果开关" :::

相关页面：

- [用于 Azure 容器注册表映像的安全中心集成漏洞评估解决方案](defender-for-container-registries-usage.md)
- [连续导出](continuous-export.md)

<!--Not available in MC: ### Prevent security misconfigurations by enforcing recommendations when creating new resources-->
###  <a name="network-security-group-recommendations-improved"></a>优化了网络安全组建议

以下与网络安全组相关的安全建议已得到优化，可减少误报。

- 应在与 VM 关联的 NSG 上限制所有网络端口
- 应关闭虚拟机上的管理端口
- 面向 Internet 的虚拟机应使用网络安全组进行保护
- 子网应与网络安全组关联


### <a name="deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services"></a>弃用了预览 AKS 建议“应在 Kubernetes 服务上定义 Pod 安全策略”

如 [Azure Kubernetes 服务](../aks/use-pod-security-policies.md)文档中所述，弃用了预览建议“应在 Kubernetes Services 上定义 Pod 安全策略”。

Pod 安全策略（预览版）功能已设置为弃用，并且在 2020 年 10 月 15 日之后将不再提供，以支持 AKS 的 Azure Policy。

弃用 Pod 安全策略（预览版）之后，必须在使用已弃用功能的任何现有群集上禁用该功能，以执行将来的群集升级并保留在 Azure 支持范围内。


### <a name="email-notifications-from-azure-security-center-improved"></a>优化了 Azure 安全中心内的电子邮件通知

电子邮件中与安全警报相关的以下部分已得到优化： 

- 添加了发送针对所有严重性级别的电子邮件警报通知的功能
- 添加了在订阅上通知具有不同 Azure 角色的用户的功能
- 默认情况下，我们会主动向订阅所有者通知高严重性警报（这些警报很可能表示真正的漏洞）
- 我们已从电子邮件通知配置页面中删除了电话号码字段

有关详细信息，请参阅[设置安全警报的电子邮件通知](security-center-provide-security-contact-details.md)。


### <a name="secure-score-doesnt-include-preview-recommendations"></a>安全功能分数不包括预览建议 

安全中心会持续评估资源、订阅和组织的安全问题。 然后，它将所有调查结果汇总成一个分数，让你可以一目了然地了解当前的安全状况：分数越高，识别出的风险级别就越低。

发现新的威胁后，安全中心会通过提出新的建议来提供新的安全建议。 为避免安全功能分数出现意外变化，以及为了提供一个宽限期（可以在新建议影响分数之前在此宽限期内了解新建议），安全功能分数的计算中将不再包括标记为“预览”的建议。 但仍应尽可能按这些建议进行修正，这样在预览期结束时，它们会有助于提升分数。

此外，“预览”建议不会使资源“运行不正常”。

预览建议示例如下：

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="带有预览标志的建议":::

[详细了解安全功能分数](secure-score-security-controls.md)。


### <a name="recommendations-now-include-a-severity-indicator-and-the-freshness-interval"></a>建议现包含严重性指示器和刷新时间间隔

现在，建议的详细信息页面包括一个刷新时间间隔指示器（如相关），并且清楚显示了建议的严重性。

:::image type="content" source="./media/release-notes/recommendations-severity-freshness-indicators.png" alt-text="显示刷新频率和严重性的建议页面":::

