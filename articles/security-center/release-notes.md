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
ms.openlocfilehash: 2628a31ba590d56f8c5c20793bee1016eb3dc5a0
ms.sourcegitcommit: dc0d10e365c7598d25e7939b2c5bb7e09ae2835c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99579454"
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

Pod 安全策略（预览）功能已设置为弃用，在 2020 年 10 月 15 日之后将不再提供，以支持适用于 AKS 的 Azure Policy。

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



## <a name="august-2020"></a>2020 年 8 月

8 月的更新包括：

- [资产清单 - 功能强大的资产安全状况新视图](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [添加了对 Azure Active Directory 安全默认值的支持（用于多重身份验证）](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
- [添加了服务主体建议](#service-principals-recommendation-added)
- [VM 上的漏洞评估 - 已合并建议和策略](#vulnerability-assessment-on-vms---recommendations-and-policies-consolidated)
- [新的 AKS 安全策略已添加到 ASC_default 计划 - 仅供个人预览版客户使用](#new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only)


### <a name="asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets"></a>资产清单 - 功能强大的资产安全状况新视图

安全中心的资产清单页（当前为预览版）提供了一种方法，用于查看已连接到安全中心的资源的安全状况。

安全中心会定期分析 Azure 资源的安全状态，以识别潜在的安全漏洞。 然后会提供有关如何消除这些安全漏洞的建议。 当任何资源具有未完成的建议时，它们将显示在清单中。

你可以使用该视图及其筛选器来浏览安全状况数据，并根据发现结果采取更多操作。

详细了解[资产清单](asset-inventory.md)。


### <a name="added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication"></a>添加了对 Azure Active Directory 安全默认值的支持（用于多重身份验证）

安全中心已添加对[安全默认值](../active-directory/fundamentals/concept-fundamentals-security-defaults.md)（Microsoft 的免费标识安全保护）的全部支持。

安全默认值提供了预配置的标识安全设置，以保护组织免受与标识相关的常见攻击。 安全默认值总计已保护了逾 500 万名租户；50,000 名租户也受安全中心的保护。

现在，安全中心在识别出未启用安全默认值的 Azure 订阅时将提供安全建议。 到目前为止，安全中心建议使用条件访问启用多重身份验证，这是 Azure Active Directory (AD) 高级许可证的一部分。 对于使用 Azure AD Free 的客户，我们现在建议启用安全默认值。 

我们旨在鼓励更多客户使用 MFA 保护其云环境，并缓解对[安全功能分数](secure-score-security-controls.md)影响最大的最高风险。

详细了解[安全默认值](../active-directory/fundamentals/concept-fundamentals-security-defaults.md)。


### <a name="service-principals-recommendation-added"></a>添加了服务主体建议

添加了一条新建议，该建议推荐使用管理证书来管理订阅的安全中心客户改用服务主体。

“应使用服务主体而不是管理证书来保护订阅”这一建议推荐使用服务主体或 Azure 资源管理器，以更安全地管理订阅。 

详细了解 [Azure Active Directory 中的应用程序对象和服务主体对象](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object)。


### <a name="vulnerability-assessment-on-vms---recommendations-and-policies-consolidated"></a>VM 漏洞评估 - 合并了建议和策略

安全中心检查 VM，以检测其是否正在运行漏洞评估解决方案。 如果未找到漏洞评估解决方案，安全中心将建议简化部署。

如果发现漏洞，安全中心将建议总结结果，以便必要时进行调查和修正。

无论用户使用哪些类型的扫描仪，为确保所有用户享受一致的体验，我们已将四条建议统一为以下两条：

|统一的建议|更改描述|
|----|:----|
|**应在虚拟机上启用漏洞评估解决方案**|替换以下两条建议：<br> • 在虚拟机上启用内置漏洞评估解决方案（由 Qualys 提供技术支持）（现已弃用）（仅标准层显示此建议）<br> • 漏洞评估解决方案应安装在虚拟机上（现已弃用）（标准和免费层显示此建议）|
|**应修正虚拟机中的漏洞**|替换以下两条建议：<br>• 修正虚拟机上发现的漏洞（由 Qualys 提供支持）（现已弃用）<br>• 应通过漏洞评估解决方案修正漏洞（现已弃用）|
|||

现在可根据同一建议从 Qualys 或 Rapid7 等合作伙伴部署安全中心的漏洞评估扩展或专用许可解决方案（“BYOL”）。

此外，发现漏洞并报告到安全中心时，无论哪个漏洞评估解决方案标识了结果，都会有一条建议提醒你注意这些结果。

#### <a name="updating-dependencies"></a>更新依赖项

如果脚本、查询或自动化引用了先前的建议或策略密钥/名称，请使用下表更新引用：

##### <a name="before-august-2020"></a>2020 年 8 月之前

|建议|范围|
|----|:----|
|**在虚拟机上启用内置漏洞评估解决方案（由 Qualys 提供支持）**<br>注册表项：550e890b-e652-4d22-8274-60b3bdb24c63|内置|
|修正虚拟机上发现的漏洞（由 Qualys 提供支持）<br>注册表项：1195afff-c881-495e-9bc5-1486211ae03f|内置|
|应在虚拟机上安装漏洞评估解决方案<br>注册表项：01b1ed4c-b733-4fee-b145-f23236e70cf3|BYOL|
|**应通过漏洞评估解决方案修复漏洞**<br>注册表项：71992a2a-d168-42e0-b10e-6b45fa2ecddb|BYOL|
||||


|策略|范围|
|----|:----|
|**应对虚拟机启用漏洞评估**<br>策略 ID：501541f7-f7e7-4cd6-868c-4190fdad3ac9|内置|
|**应通过漏洞评估解决方案修正漏洞**<br>策略 ID：760a85ff-6162-42b3-8d70-698e268f648c|BYOL|
||||


##### <a name="from-august-2020"></a>2020 年 8 月之后

|建议|范围|
|----|:----|
|**应在虚拟机上启用漏洞评估解决方案**<br>密钥：ffff0522-1e88-47fc-8382-2a80ba848f5d|内置 + BYOL|
|**应修正虚拟机中的漏洞**<br>注册表项：1195afff-c881-495e-9bc5-1486211ae03f|内置 + BYOL|
||||

|策略|范围|
|----|:----|
|[应对虚拟机启用漏洞评估](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f501541f7-f7e7-4cd6-868c-4190fdad3ac9)<br>策略 ID：501541f7-f7e7-4cd6-868c-4190fdad3ac9 |内置 + BYOL|
||||


### <a name="new-aks-security-policies-added-to-asc_default-initiative---for-use-by-private-preview-customers-only"></a>新的 AKS 安全策略已添加到 ASC_default 计划 - 仅供个人预览版客户使用

为确保默认情况下保护 Kubernetes 工作负载，安全中心正在添加 Kubernetes 级别的策略和强化建议，包括带有 Kubernetes 准入控制的强制执行选项。

早期阶段的此项目包括个人预览版，并增加了 ASC_default 计划的新策略（默认情况下禁用）。

可以放心地忽略这些策略，这些策略不会对环境造成影响。 如果要启用他们，请在 https://aka.ms/SecurityPrP 中注册预览版，然后在以下选项中进行选择：

1. **单一预览版** - 仅加入此个人预览版。 明确提及“ASC 连续扫描”作为要加入的预览版。
1. **正在进行的计划** - 将添加到此个人预览版和后续个人预览版。 你需要完成个人资料和隐私协议。

