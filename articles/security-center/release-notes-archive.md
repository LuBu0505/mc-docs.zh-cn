---
title: Azure 安全中心的新增功能存档
description: 介绍 Azure 安全中心在六个月以前的新增功能和已更改的功能。
services: security-center
documentationcenter: na
author: Johnnytechn
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2021
ms.author: v-johya
ms.openlocfilehash: 570c89ae71258bf3d2b88020dab1968ce11d10ed
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197637"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Azure 安全中心的新增功能存档

[Azure 安全中心的新增功能](release-notes.md)发行说明主页包含过去六个月的更新，而本页包含更早以前的项目。

本页提供以下方面的信息：

- 新增功能
- Bug 修复
- 已弃用的功能


## <a name="august-2020"></a>2020 年 8 月

8 月的更新包括：

- [资产清单 - 功能强大的资产安全状况新视图](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [添加了对 Azure Active Directory 安全默认值的支持（用于多重身份验证）](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
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

## <a name="march-2020"></a>2020 年 3 月

3 月的更新包括：

- [工作流自动化现已正式发布](#workflow-automation-is-now-generally-available)
- [Azure Kubernetes 服务保护](#protection-for-azure-kubernetes-service)
- [改进了实时体验](#improved-just-in-time-experience)
- [弃用了两项针对 Web 应用的安全建议](#two-security-recommendations-for-web-applications-deprecated)


### <a name="workflow-automation-is-now-generally-available"></a>工作流自动化现已正式发布

Azure 安全中心现已正式发布工作流自动化功能。 它可用于在安全警报和建议上自动触发逻辑应用。 此外，也可对提供了快速修复选项的警报和所有建议执行手动触发。

每个安全计划都包含事件响应的多个工作流。 这些流程可能包含通知相关利益干系人、启动更改管理进程，以及应用特定的修正步骤。 安全专家建议你尽可能多地将这些流程自动化。 自动化可减少开销，还可确保根据你预定义的要求快速、一致地执行处理步骤，从而增强安全性。

若要详细了解用于运行工作流的自动和手动安全中心功能，请参阅[工作流自动化](workflow-automation.md)。

详细了解如何[创建逻辑应用](../logic-apps/logic-apps-overview.md)。


### <a name="protection-for-azure-kubernetes-service"></a>Azure Kubernetes 服务保护

Azure 安全中心正在扩展其容器安全功能，现可保护 Azure Kubernetes 服务 (AKS)。

常见的开源平台 Kubernetes 已被广泛采用，现在已成为容器业务流程方面的行业标准。 尽管得到了广泛实施，但在如何保护 Kubernetes 环境方面，人们仍然缺少了解。 要抵御容器化应用程序的攻击面，需要具备专业知识来确保基础结构已安全配置且受到持续监视，防范潜在威胁。

安全中心的防范包括：

- **发现和可见性** - 在安全中心内注册的订阅中持续发现托管的 AKS 实例。
- **安全建议** - 帮助你遵守 AKS 安全最佳做法的可操作建议。 这些建议包含在安全功能分数中，确保被视为组织的安全状态的一部分。 你可能会看到的一个与 AKS 相关的建议示例是，“应使用基于角色的访问控制来限制对 Kubernetes 服务群集的访问”。
- **威胁防护** - 通过对 AKS 部署的持续分析，Azure 安全中心会提醒你注意在主机和 AKS 群集级别检测到的威胁和恶意活动。

详细了解 [Azure Kubernetes 服务与安全中心的集成](defender-for-kubernetes-introduction.md)。

详细了解[安全中心内的容器安全功能](container-security.md)。


### <a name="improved-just-in-time-experience"></a>改进了实时体验

增强了 Azure 安全中心内保护管理端口的实时工具的功能、操作和 UI，如下所示： 

- **“理由”字段** - 通过 Azure 门户的实时页面请求对虚拟机 (VM) 的访问时，现有一个新的可选字段可用来输入请求原因。 可在活动日志中跟踪在此字段中输入的信息。 
- **自动清除冗余实时 (JIT) 规则** - 无论何时更新 JIT 策略，都会自动运行清理工具来检查整个规则集的有效性。 该工具还将查看你策略中的规则与 NSG 中的规则之间是否存在不匹配的情况。 如果清理工具发现不匹配的情况，它将确定原因，并在确定可安全操作的情况下，删除不再需要的内置规则。 清理工具绝不会删除你创建的规则。 

详细了解 [JIT 访问功能](security-center-just-in-time.md)。


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>弃用了两项针对 Web 应用的安全建议

即将弃用下面两项与 Web 应用相关的安全建议： 

- 应加强 IaaS NSG 上 Web 应用的规则。
    （相关策略：应该强化 IaaS 上 Web 应用程序的 NSG 规则）

- 应限制对应用服务的访问。
    （相关策略：应限制对应用服务的访问 [预览版]）

这些建议将不再在安全中心的建议列表中显示。 相关策略将不再包含在名为“安全中心默认设置”的计划中。

详细了解[安全建议](recommendations-reference.md)。




## <a name="february-2020"></a>2020 年 2 月

### <a name="fileless-attack-detection-for-linux-preview"></a>面向 Linux 的无文件攻击检测（预览版）

随着攻击者越来越多地采用更隐蔽的方法来避免被发现，除了 Windows 之外，Azure 安全中心还扩大了无文件攻击检测范围，现涵盖 Linux。 无文件攻击利用软件漏洞、将恶意有效负载注入良性系统进程，并隐藏在内存中。 这些技术：

- 最大程度地减少或消除了磁盘上恶意软件的痕迹
- 大大降低了基于磁盘的恶意软件扫描解决方案的检测机会

为了应对这种威胁，Azure 安全中心于 2018 年 10 月发布了面向 Windows 的无文件攻击检测，现在还将该检测扩展到了 Linux 上。 



## <a name="january-2020"></a>2020 年 1 月

### <a name="enhanced-secure-score-preview"></a>安全功能分数增强版（预览版）

Azure 安全中心的安全功能分数增强版现提供预览版。 在此版本中，多个建议被组合到安全控件中，可更好地反映出你易受攻击的攻击面（例如限制对管理端口的访问）。

请在预览阶段熟悉安全功能分数的更改之处，确定可帮助你进一步保护环境的其他修正措施。

详细了解[安全功能分数增强版（预览版）](secure-score-security-controls.md)。



## <a name="november-2019"></a>2019 年 11 月

11 月的更新包括：
 - [针对 Azure 存储的威胁防护包括恶意软件信誉屏蔽](#threat-protection-for-azure-storage-includes-malware-reputation-screening)
 - [使用逻辑应用实现工作流自动化（预览版）](#workflow-automation-with-logic-apps-preview)
 - [批量资源快速修复功能已推出正式版](#quick-fix-for-bulk-resources-generally-available)
 - [扫描容器映像的漏洞（预览版）](#scan-container-images-for-vulnerabilities-preview)
 - [其他监管合规标准（预览版）](#additional-regulatory-compliance-standards-preview)
 - [针对 Azure Kubernetes 服务的威胁防护（预览版）](#threat-protection-for-azure-kubernetes-service-preview)
 - [支持自定义策略（预览版）](#support-for-custom-policies-preview)
 - [在面向社区和合作伙伴的平台中扩大 Azure 安全中心的涵盖范围](#extending-azure-security-center-coverage-with-platform-for-community-and-partners)
 - [支持导出建议和警报的高级集成（预览版）](#advanced-integrations-with-export-of-recommendations-and-alerts-preview)
 - [从 Windows 管理中心将本地服务器加入安全中心（预览版）](#onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview)

### <a name="threat-protection-for-azure-key-vault-in-north-america-regions-preview"></a>针对北美区域 Azure Key Vault 的威胁防护（预览版）

Azure Key Vault 是一个基本服务，它通过提供集中管理云中密钥、机密、加密密钥和策略的功能，来保护数据和提高云应用程序的性能。 由于 Azure Key Vault 存储敏感数据和业务关键数据，因此必须保证密钥保管库及其存储的数据的最高安全性。

Azure 安全中心对 Azure Key Vault 的威胁防护的支持提供额外的安全情报层，用于检测以非寻常和可能有害的方式访问或恶意利用密钥保管库的企图。 此新保护层使得客户无需成为安全专家或管理安全监视系统，就能应对其密钥保管库受到的威胁。 此功能在北美区域推出了公共预览版。


### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>针对 Azure 存储的威胁防护包括恶意软件信誉屏蔽

针对 Azure 存储的威胁防护提供由 Microsoft 威胁情报支持的新检测，可以使用哈希信誉分析来检测将恶意软件上传到 Azure 存储的行为，并可以检测从活动的 Tor 出口节点（一个匿名代理）进行的可疑访问。 现在，你可以使用 Azure 安全中心查看在各个存储帐户中检测到的恶意软件。


### <a name="workflow-automation-with-logic-apps-preview"></a>使用逻辑应用实现工作流自动化（预览版）

在集中管理安全性和 IT/运营的组织的环境中发现差异时，这些组织可以实施内部工作流程来驱动所需的操作。 在许多情况下，这些工作流是可重复的流程，而自动化可以在组织内部大幅简化流程。

目前我们正在安全中心内引入一项新功能，它可以让客户创建利用 Azure 逻辑应用的自动化配置，并创建可根据具体 ASC 发现结果自动触发这些配置的策略（例如“建议”或“警报”）。 可将 Azure 逻辑应用配置为执行逻辑应用连接器大型社区所支持的任何自定义操作，或使用安全中心提供的某个模板，例如，发送电子邮件或开具 ServiceNow™ 票证。

若要详细了解用于运行工作流的自动和手动安全中心功能，请参阅[工作流自动化](workflow-automation.md)。

若要了解如何创建逻辑应用，请参阅 [Azure 逻辑应用](../logic-apps/logic-apps-overview.md)。


### <a name="quick-fix-for-bulk-resources-generally-available"></a>批量资源快速修复功能已推出正式版

由于用户在安全评分中要执行许多任务，有效修正大型机群中的问题可能会变得困难。

为了简化对安全错误配置的修正，快速修正针对批量资源的建议并提高安全评分，请使用“快速修复”修正措施。

此操作可让你选择要对其应用修正措施的资源，并启动一个修正措施来让系统代你配置设置。

现在，安全中心建议页上已向客户推出了快速修复正式版。

在[安全建议参考指南](recommendations-reference.md)中了解哪些建议启用了快速修复。


### <a name="scan-container-images-for-vulnerabilities-preview"></a>扫描容器映像的漏洞（预览版）

Azure 安全中心现在可以扫描 Azure 容器注册表中容器映像的漏洞。

映像扫描的工作原理是分析容器映像文件，然后查看是否存在任何已知漏洞（由 Qualys 提供支持）。

将新容器映像推送到 Azure 容器注册表时，会自动触发扫描本身。 发现的漏洞将以安全中心建议的形式显示，其中包括 Azure 安全评分，以及有关如何修补这些漏洞以减小允许的受攻击面的信息。


### <a name="additional-regulatory-compliance-standards-preview"></a>其他监管合规标准（预览版）

“监管合规”仪表板基于安全中心评估结果提供合规态势的见解。 该仪表板会显示你的环境是否符合特定法规标准和行业基准指定的控制措施与要求，并提供有关如何符合这些要求的规范性建议。

目前，监管合规仪表板支持四项内置标准：Azure CIS 1.1.0、PCI-DSS、ISO 27001 和 SOC-TSP。 我们现在宣布推出其他受支持标准的公共预览版：NIST SP 800-53 R4、SWIFT CSP CSCF v2020、加拿大联邦 PBMM、UK Official 和 UK NHS。 此外，我们正在发布 Azure CIS 1.1.0 的更新版本，其中涵盖了标准中的更多控制措施，并增强了扩展性。


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>针对 Azure Kubernetes 服务的威胁防护（预览版）

Kubernetes 很快就成了在云中部署和管理软件的新标准。 只有少量的用户对 Kubernetes 拥有丰富的经验；很多用户只是注重一般的工程和管理，而忽略了安全方面。 需要精心配置 Kubernetes 环境以使其保持安全，确保不会公开任何以容器为中心的受攻击面，避免为攻击者打开后门。 安全中心正在将其在容器领域的支持扩展到 Azure 中发展速度最快的服务之一 - Azure Kubernetes 服务 (AKS)。

此公共预览版中的新功能包括：

- **发现和可见性** - 在安全中心的已注册订阅中持续发现 AKS 托管实例。
- **安全功能分数建议** - 可操作的项目，用于帮助客户遵守 AKS 的安全最佳做法，并提高其安全功能分数。 建议包括诸如“应使用基于角色的访问控制来限制对 Kubernetes 服务群集的访问”之类的项目。
- **威胁检测** - 基于主机和群集的分析，例如“检测到特权容器”。


<!---Not available in MC: ### Virtual machine vulnerability assessment (preview)-->
<!--Not avaiable in MC: ### Advanced data security for SQL servers on Azure Virtual Machines (preview)-->
### <a name="support-for-custom-policies-preview"></a>支持自定义策略（预览版）

Azure 安全中心现在支持自定义策略（预览版）。

我们的客户一直希望在安全中心内扩大其当前安全评估的涵盖范围，以便能够基于他们在 Azure Policy 中创建的策略执行自己的安全评估。 由于支持自定义策略，这种愿望已得到实现。

安全中心建议体验、安全评分和监管合规标准仪表板中将会包含这些新策略。 由于支持自定义策略，现在可以在 Azure Policy 中创建一个自定义计划，然后将其作为策略添加到安全中心，并将其作为建议可视化。


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>在面向社区和合作伙伴的平台中扩大 Azure 安全中心的涵盖范围

使用安全中心不仅可以从 Microsoft 接收建议，而且还能从 Check Point、Tenable 和 CyberArk 等合作伙伴提供的现有解决方案以及不断推出的其他许多集成中接收建议。  安全中心的简单加入流程可将你的现有解决方案连接到安全中心，使你可以在一个位置查看安全态势建议、运行统一报告，以及针对内置建议和合作伙伴建议利用安全中心的所有功能。 还可以将安全中心建议导出到合作伙伴产品。

[详细了解 Microsoft 智能安全关联](https://www.microsoft.com/security/partnerships/intelligent-security-association)。



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>支持导出建议和警报的高级集成（预览版）

若要在安全中心的顶层实现企业级方案，现在可以在除 Azure 门户或 API 以外的其他位置使用安全中心警报和建议。 可直接将这些警报和建议导出到事件中心与 Log Analytics 工作区。 下面是可以围绕这些新功能创建的一些工作流：

- 借助导出到 Log Analytics 工作区的功能，可以使用 Power BI 创建自定义仪表板。
- 借助导出到事件中心的功能，可将安全中心警报和建议导出到第三方 SIEM、实时导出到第三方解决方案，或导出到 Azure 数据资源管理器。


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>从 Windows 管理中心将本地服务器加入安全中心（预览版）

Windows 管理中心是一个管理门户，适用于未在 Azure 中部署的 Windows Server，为它们提供多项 Azure 管理功能（例如备份和系统更新）。 我们最近添加了一项功能，允许直接从 Windows 管理中心体验加入这些非 Azure 服务器，使其受 ASC 保护。

使用此新体验，用户可以直接在 Windows 管理中心体验中将 WAC 服务器加入 Azure 安全中心，并查看其安全警报和建议。


## <a name="september-2019"></a>2019 年 9 月

9 月的更新包括：

 - [使用自适应应用程序控制管理规则的功能已改进](#managing-rules-with-adaptive-application-controls-improvements)
 - [使用 Azure Policy 控制容器安全建议](#control-container-security-recommendation-using-azure-policy)

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>使用自适应应用程序控制管理规则的功能已改进

使用自适应应用程序控制管理虚拟机规则的体验已得到改进。 Azure 安全中心的自适应应用程序控制可帮助你控制哪些应用程序能够在你的虚拟机上运行。 除了对规则管理做出了一般性的改进外，在添加新规则时，你还可以通过一个新功能来控制要保护哪些文件类型。

[详细了解自适应应用程序控制](security-center-adaptive-application.md)。


### <a name="control-container-security-recommendation-using-azure-policy"></a>使用 Azure Policy 控制容器安全建议

现在，可以通过 Azure Policy 启用或禁用 Azure 安全中心提供的有关修正容器安全性中的漏洞的建议。

若要查看已启用的安全策略，请在安全中心内打开“安全策略”页。


## <a name="august-2019"></a>2019 年 8 月

8 月的更新包括：

 - [Azure 防火墙的实时 (JIT) VM 访问](#just-in-time-jit-vm-access-for-azure-firewall)
 - [提升安全态势的一键式修正（预览版）](#single-click-remediation-to-boost-your-security-posture-preview)

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>Azure 防火墙的实时 (JIT) VM 访问 

Azure 防火墙的实时 (JIT) VM 访问现已推出正式版。 使用此功能可以确保 Azure 防火墙保护的环境以及 NSG 保护的环境的安全。

JIT VM 访问使用 NSG 和 Azure 防火墙规则，仅在需要时才提供对 VM 的受控访问，以此减少受到网络容量耗尽攻击的风险。

为 VM 启用 JIT 时，可创建一个策略来确定要保护的端口、端口保持打开状态的时间，以及可从哪些已批准的 IP 地址访问这些端口。 此策略可帮助你控制用户在请求访问权限时可执行哪些操作。

请求将记录在 Azure 活动日志中，因此你可以轻松监视和审核访问。 此实时页面还可帮助你快速识别已启用 JIT 的现有 VM，以及建议启用 JIT 的 VM。

[详细了解 Azure 防火墙](../firewall/overview.md)。


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>提升安全态势的一键式修正（预览版）

安全评分是一个可帮助你评估工作负荷安全状况的工具。 它会评审你的安全建议并确定其优先级，以便你知道要首先执行哪些建议。 这可帮助你找到最严重的安全漏洞，以确定调查优先级。

为了简化安全配置错误的修正并帮助你快速提高安全评分，我们添加了一项新功能，让你一键式修正针对批量资源的建议。

此操作可让你选择要对其应用修正措施的资源，并启动一个修正措施来让系统代你配置设置。

请在[安全建议参考指南](recommendations-reference.md)中查看哪些建议启用了快速修复。


<!--Not available in MC: ### Cross-tenant management-->
## <a name="july-2019"></a>2019 年 7 月

### <a name="updates-to-network-recommendations"></a>网络建议的更新

Azure 安全中心 (ASC) 已推出新的网络建议，并改进了一些现有的建议。 现在，使用安全中心可以确保进一步为资源提供更好的网络保护。 

[详细了解网络建议](recommendations-reference.md#recs-networking)。


## <a name="june-2019"></a>2019 年 6 月

### <a name="adaptive-network-hardening---generally-available"></a>自适应网络强化 - 正式版

公有云中运行的工作负荷面对的最大受攻击面之一是与公共 Internet 之间的连接。 我们的客户发现，他们很难知道要部署哪些网络安全组 (NSG) 规则来确保仅在所需的源范围内提供 Azure 工作负荷。 使用此功能，安全中心可以了解 Azure 工作负荷的网络流量和连接模式，并为面向 Internet 的虚拟机提供 NSG 规则建议。 这有助于我们的客户更好地配置其网络访问策略，并限制受到攻击的风险。 

