---
title: 适用于 Azure Bastion 的 Azure 安全基线
description: Azure Bastion 安全基线为实现 Azure 安全基准中指定的安全建议提供过程指南和资源。
ms.service: bastion
ms.topic: conceptual
origin.date: 11/20/2020
author: rockboyfor
ms.date: 01/11/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: subject-security-benchmark
ms.openlocfilehash: be7170d1f25ffd2ef418c7c2ec6557f3609f6cb3
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022647"
---
<!--Verified successfully-->
# <a name="azure-security-baseline-for-azure-bastion"></a>适用于 Azure Bastion 的 Azure 安全基线

此安全基线将 [Azure 安全基准版本 2.0](../security/benchmarks/overview.md) 中的指南应用于 Azure Bastion。 Azure 安全基准提供有关如何在 Azure 上保护云解决方案的建议。 内容按“安全控制”分组，这些控制根据适用于 Azure Bastion 的 Azure 安全基准和相关指南定义。 排除了不适用于 Azure Bastion 的“控制”。

若要查看 Azure Bastion 如何完全映射到 Azure 安全基准，请参阅[完整的 Azure Bastion 安全基线映射文件](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)。

## <a name="network-security"></a>网络安全

[有关详细信息，请参阅 *Azure 安全基线：* 网络安全性](../security/benchmarks/security-controls-v2-network-security.md)。

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1：实现内部流量的安全性

**指导**：部署 Azure Bastion 资源时，必须创建或使用现有虚拟网络。 确保所有 Azure 虚拟网络都遵循与业务风险相匹配的企业分段原则。 任何可能会给组织带来更高风险的系统都应隔离在其自己的虚拟网络中，并通过网络安全组 (NSG) 进行充分保护。

Azure Bastion 服务需要打开以下端口才能正常运行：

- 入口流量：
    - 来自公共 Internet 的入口流量： Azure Bastion 将创建一个公共 IP，需要在该公共 IP 上启用端口 443，用于入口流量。 不需要在 AzureBastionSubnet 上打开端口 3389/22。 

    - 来自 Azure Bastion 控制平面的入口流量： 对于控制平面连接，请启用从 GatewayManager 服务标记进行的端口 443 入站。 这使控制平面（即网关管理器）能够与 Azure Bastion 通信。

- 出口流量：

    - 流向目标虚拟机 (VM) 的出口流量：Azure Bastion 将通过专用 IP 到达目标 VM。 NSG 需要允许端口 3389 和 22 的出口流量流向其他目标 VM 子网。

    - 流向 Azure 中其他公共终结点的出口流量： Azure Bastion 需要能够连接到 Azure 中的各种公共终结点，以便执行相应操作（例如，存储诊断日志和计量日志）。 因此，Azure Bastion 需要出站到 443，再到 AzureCloud 服务标记。

与网关管理器和 Azure 服务标记的连接受 Azure 证书的保护（锁定）。 外部实体（包括这些资源的使用者）无法在这些终结点上通信。 

- [如何创建包含安全规则的网络安全组](../virtual-network/tutorial-filter-network-traffic.md)

- [可在此处详细了解 Bastion NSG 要求](bastion-nsg.md)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="ns-2-connect-private-networks-together"></a>NS-2：将专用网络连接在一起

**指导**：Azure Bastion 不公开可通过专用网络访问的任何终结点。 Azure Bastion 支持部署到对等互连网络，以集中你的 Bastion 部署并启用跨网络连接。

- [Azure Bastion 和虚拟网络对等互连](vnet-peering.md)

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="identity-management"></a>标识管理

有关详细信息，请参阅 [Azure 安全基准：标识管理](../security/benchmarks/security-controls-v2-identity-management.md)。

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1：将 Azure Active Directory 标准化为中央标识和身份验证系统

**指导**：Azure Bastion 与 Azure 的默认标识和访问管理服务 Azure Active Directory (Azure AD) 集成。 用户可以使用 Azure AD 身份验证来访问 Azure 门户，在门户中管理 Azure Bastion 服务（创建、更新和删除 Bastion 资源）。

使用 Azure Bastion 连接到虚拟机需要 SSH 密钥或用户名/密码，当前不支持使用 Azure AD 凭据。

除 SSH 密钥或用户名/密码外，使用 Azure Bastion 连接到虚拟机时，用户还需要以下角色分配：
- 目标虚拟机上的读者角色
- NIC 上的读者角色（使用目标虚拟机的专用 IP）
- Azure Bastion 资源上的读者角色

有关详细信息，请参阅以下资源：

- [使用 Azure Bastion 连接到 Linux 虚拟机](bastion-connect-vm-ssh.md)

- [使用 Azure Bastion 连接到 Windows 虚拟机](bastion-connect-vm-rdp.md)

- [Azure 内置角色](../role-based-access-control/built-in-roles.md)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3：使用 Azure AD 单一登录 (SSO) 进行应用程序访问

**指导**：向虚拟机资源进行身份验证时，Azure Bastion 不支持通过 SSO 进行身份验证，仅支持 SSH 或用户名/密码。 但是，Azure Bastion 使用 Azure Active Directory (Azure AD) 在整个服务中提供标识和访问管理。 用户可以通过向 Azure AD 进行身份验证来访问和管理其 Azure Bastion 资源，并通过 Azure AD Connect 使用自己的同步企业标识来体验无缝单一登录。 

<!--Not Available on [Understand Application SSO with Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)-->

- [Azure AD Connect](../active-directory/hybrid/whatis-azure-ad-connect.md)

- [使用 Azure Bastion 连接到 Linux 虚拟机](bastion-connect-vm-ssh.md)

- [使用 Azure Bastion 连接到 Windows 虚拟机](bastion-connect-vm-rdp.md)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4：对所有基于 Azure Active Directory 的访问使用强身份验证控制

**指导**：Azure Bastion 通过与 Azure Active Directory (Azure AD) 集成来实现服务的访问和管理。 为 Azure AD 租户配置 Azure 多重身份验证。 Azure AD 支持通过多重身份验证 (MFA) 和强无密码方法进行强身份验证控制。  
- 多重身份验证：启用 Azure AD MFA，并遵循 Azure 安全中心标识和访问管理建议来设置你的 MFA。 可以基于登录条件和风险因素，对所有用户、特选用户或单个用户强制执行 MFA。 

- 无密码身份验证：有三个无密码身份验证选项可用：Windows Hello for Business、Microsoft Authenticator 应用和本地身份验证方法（例如智能卡）。 
    
    <!--CORRECT ON Microsoft Authenticator app-->
    
- [如何在 Azure 中启用 MFA](../active-directory/authentication/howto-mfa-getstarted.md)

<!--Not Available on [Introduction to passwordless authentication options for Azure Active Directory](../active-directory/authentication/concept-authentication-passwordless.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

<!--Not Available on ### IM-5: Monitor and alert on account anomalies-->
<!--Not Available on [You can find more information about how to enable diagnostics logging here](diagnostic-logs.md)-->

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6：基于条件限制 Azure 资源访问

**指导**：只能通过 Azure 门户访问 Azure Bastion 服务；可以使用 Azure Active Directory (Azure AD) 条件访问来限制对 Azure 门户的访问。 基于用户定义的条件，使用 Azure AD 条件访问进行更精细的访问控制，例如，要求从特定 IP 范围登录的用户使用 MFA。

客户还可以在加入域的虚拟机级别使用其他基于角色的访问控制策略，进一步限制对虚拟机的访问。

- [可在此处详细了解 Azure AD 条件访问](../active-directory/conditional-access/overview.md)

- [Azure 条件访问概述](../active-directory/conditional-access/overview.md)

- [常见的条件访问策略](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

<!--Not Available on [Configure authentication session management with conditional access](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="privileged-access"></a>特权访问

有关详细信息，请参阅 [Azure 安全基准：特权访问](../security/benchmarks/security-controls-v2-privileged-access.md)。

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2：限制对关键业务型系统的管理访问权限

**指导**：Azure Bastion 使用 Azure 基于角色的访问控制 (Azure RBAC) 来隔离对业务关键型系统的访问，其原理是限制对某些帐户授予连接到某些虚拟机的访问权限。 请务必遵循最低权限原则，使用户仅具有完成特定任务所需的权限。

确保还限制了对管理、标识和安全系统的访问，因为这些系统对业务关键型资产具有管理访问权限，这些系统包括 Active Directory 域控制器 (DC)、安全工具以及在业务关键型系统上安装了代理的系统管理软件等。 入侵这些管理和安全系统的攻击者可以立即将它们用作损害业务关键型资产的武器。

所有类型的访问控制都应符合企业分段策略，确保访问控制保持一致。

- [使用 Azure Bastion 访问虚拟机所需的角色](bastion-faq.md#roles)

<!--Not Available on [Azure Components and Reference model](https://docs.azure.cn/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)-->

- [访问管理组](../governance/management-groups/overview.md#management-group-access)

<!--Not Available on [Azure subscription administrators](../cost-management-billing/manage/add-change-subscription-administrator.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3：定期审查和协调用户访问权限

**指导**：Azure Bastion 使用 Azure Active Directory (Azure AD) 帐户和 Azure RBAC 来管理其资源。 请定期审查用户帐户和访问权限分配，确保帐户及其访问权限有效。 可使用 Azure AD 访问评审来审查组成员身份、对企业应用程序的访问权限和角色分配。 Azure AD 报告提供日志来帮助发现过时的帐户。 你还可使用 Azure AD Privileged Identity Management 来创建访问评审报表工作流以便于执行评审。

此外，Azure Privileged Identity Management 还可配置为在创建过多管理员帐户时发出警报，并识别过时或配置不正确的管理员帐户。

- [在 Privileged Identity Management (PIM) 中创建对 Azure 资源角色的访问评审](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

<!--Not Available on [Removing access to a delegation](../lighthouse/how-to/remove-delegation.md)-->

- [如何使用 Azure AD 标识和访问评审](../active-directory/governance/access-reviews-overview.md)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4：在 Azure AD 中设置紧急访问

**指导**：Azure Bastion 通过与 Azure Active Directory 和 Azure RBAC 集成来管理其资源。 为了防止意外退出 Azure AD 组织，请设置一个紧急访问帐户，以便在正常管理帐户无法使用时进行访问。 紧急访问帐户通常拥有较高的权限，因此请不要将其分配给特定的个人。 紧急访问帐户只能用于“不受限”紧急情况，即不能使用正常管理帐户的情况。

应确保妥善保管紧急访问帐户的凭据（例如密码、证书或智能卡），仅将其告诉只能在紧急情况下有权使用它们的个人。

- [在 Azure AD 中管理紧急访问帐户](../active-directory/roles/security-emergency-access.md)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="pa-5-automate-entitlement-management"></a>PA-5：将权利管理自动化 

**指导**：Azure Bastion 通过与 Azure Active Directory (Azure AD) 和 Azure RBAC 集成来管理其资源。 使用 Azure AD 的权利管理功能可自动执行访问请求工作流，包括访问权限分配、审查和过期。 还支持两阶段或多阶段审批。

- [什么是 Azure AD 访问评审](../active-directory/governance/access-reviews-overview.md)

- [什么是 Azure AD 权利管理](../active-directory/governance/entitlement-management-overview.md)

**Azure 安全中心监视**：目前不可用

**责任**：客户

<!--Not Available on ### PA-6: Use privileged access workstations-->
<!--Not Available on [Understand privileged access workstations](../active-directory/devices/concept-azure-managed-workstation.md)-->

<!--Not Available on [Deploy a privileged access workstation](../active-directory/devices/howto-azure-managed-workstation.md)-->

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7：遵循 Just Enough Administration（最小特权原则） 

**指导**：Azure Bastion 通过与 Azure 基于角色的访问控制 (RBAC) 集成来管理其资源。 使用 Azure RBAC，可通过角色分配来管理 Azure 资源访问。 可以将这些内置角色分配给用户、组、服务主体和托管标识。 某些资源具有预定义的内置角色，可以通过工具（例如 Azure CLI、Azure PowerShell 或 Azure 门户）来清点或查询这些角色。 通过 Azure RBAC 分配给资源的特权应始终限制为角色所需的特权。 这是对 Azure AD Privileged Identity Management (PIM) 的实时 (JIT) 方法的补充，应定期进行审查。 请使用内置角色来分配权限，仅在必要时创建自定义角色。

使用 Azure Bastion 连接到虚拟机时，用户需要以下角色分配：
- 目标虚拟机上的读者角色
- NIC 上的读者角色（使用目标虚拟机的专用 IP）
- Azure Bastion 资源上的读者角色

有关详细信息，请参阅以下资源：

- [使用 Azure Bastion 连接到 Linux 虚拟机](bastion-connect-vm-ssh.md)

- [使用 Azure Bastion 连接到 Windows 虚拟机](bastion-connect-vm-rdp.md)

- [Azure 内置角色](../role-based-access-control/built-in-roles.md)

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="asset-management"></a>资产管理

有关详细信息，请参阅 [Azure 安全基准：资产管理](../security/benchmarks/security-controls-v2-asset-management.md)。

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1：确保安全团队可以了解与资产相关的风险

**指南**：确保在 Azure 租户和订阅中向安全团队授予了安全读取者权限，以便他们可以使用 Azure 安全中心监视安全风险。 

根据安全团队责任划分方式的不同，监视安全风险可能是中心安全团队或本地团队的责任。 也就是说，安全见解和风险必须始终在组织内集中聚合。 

安全读取者权限可以广泛应用于整个租户（根管理组），也可以限制到管理组或特定订阅。 

注意：若要了解工作负载和服务，可能需要更多权限。 

- [安全读取者角色概述](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure 管理组概述](../governance/management-groups/overview.md)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2：确保安全团队有权访问资产清单和元数据

**指导**：向 Azure Bastion 资源、资源组和订阅应用标记，对它们进行符合逻辑的分类。 每个标记均由名称和值对组成。 例如，可以对生产中的所有资源应用名称“Environment”和值“Production”。 确保安全团队有权访问 Azure 上持续更新的资产清单。 他们可以使用 Azure Resource Graph 来查询和发现你订阅中的所有资源，包括 Azure 服务、应用程序和网络资源。

- [如何使用 Azure Resource Graph 浏览器创建查询](../governance/resource-graph/first-query-portal.md)

- [Azure 安全中心资产库存管理](../security-center/asset-inventory.md)

<!--Not Available on [For more information about tagging assets, see the resource naming and tagging decision guide](https://docs.azure.cn/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure-resource-manager%2fmanagement%2ftoc.json)-->

**Azure 安全中心监视**：是

**责任**：客户

### <a name="am-3-use-only-approved-azure-services"></a>AM-3：仅使用已批准的 Azure 服务

**指导**：使用 Azure Policy 审核和限制用户可以在你的环境中预配的服务，这包括允许或拒绝 Azure Bastion 资源部署的权力。 使用 Azure Resource Graph 查询和发现订阅中的资源。 你也可以使用 Azure Monitor 来创建规则，以便在检测到未经批准的服务时触发警报。

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [如何使用 Azure Policy 拒绝特定的资源类型](../governance/policy/samples/built-in-policies.md#general)

- [如何使用 Azure Resource Graph 浏览器创建查询](../governance/resource-graph/first-query-portal.md)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4：确保资产生命周期管理的安全

**指导**：如果不再需要部署的 Azure Bastion 资源，请删除其访问权限，以尽量减少攻击面。 用户可以通过 Azure 门户、CLI 或 REST API 来管理其 Azure Bastion 资源。 如果不再需要正在进行的远程会话，或者将其确定为潜在威胁，可将其删除或强制断开。

- [删除或强制断开远程会话](session-monitoring.md#view)

- [Azure 网络 CLI](https://docs.microsoft.com/powershell/module/az.network/?preserve-view=true&view=azps-5.1.0#networking)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5：限制用户与 Azure 资源管理器进行交互的能力

**指导**：Azure Bastion 通过与 Azure Active Directory (Azure AD) 集成来进行标识管理和身份验证。 你可以使用 Azure 条件访问来限制用户与 Azure 资源管理器交互的能力，操作方法：为“Microsoft Azure 管理”应用配置“阻止访问”。

<!--CORRECT ON Microsoft Azure Management-->

- [如何配置条件访问来阻止对 Azure 资源管理器的访问](../role-based-access-control/conditional-access-azure-management.md)

**Azure 安全中心监视**：是

**责任**：客户

## <a name="logging-and-threat-detection"></a>日志记录和威胁检测

有关详细信息，请参阅 [Azure 安全基准：日志记录和威胁检测](https://docs.azure.cn/security/benchmarks/security-controls-v2-data-protection)。

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2：启用 Azure 标识和访问管理的威胁检测

**指导**：Azure Bastion 服务集成了 Azure Active Directory (Azure AD)，并且可通过 Azure 门户进行访问。 默认通过 Azure 活动日志捕获对服务的管理操作（如创建、更新和删除）。 用户还应启用 Azure Bastion 资源日志（例如会话 BastionAuditLogs）来跟踪 Bastion 会话。

Azure AD 提供以下用户日志，可在 Azure AD 报表中进行查看，也可将这些日志与 Azure Monitor、Azure Sentinel 或其他 SIEM/监视工具集成，以用于更复杂的监视和分析用例： 
- 登录 - 在登录报告中，可了解托管应用程序的使用情况和用户登录活动。

- 审核日志 - 通过日志为 Azure AD 中的各种功能所做的所有更改提供可跟踪性。 审核日志的示例包括对 Azure AD 中的任何资源（例如添加或删除用户、应用、组、角色和策略）所做的更改。

- 风险登录 - 风险登录是指可能由非用户帐户合法拥有者进行的登录尝试。

- 已标记为存在风险的用户 - 风险用户是指可能已泄露的用户帐户。

Azure 安全中心还可针对某些可疑活动发出警报，这些活动包括失败的身份验证尝试次数太多，以及帐户已在订阅中遭到弃用。 除了基本的安全卫生监视，Azure 安全中心的威胁防护模块还可从单个 Azure 计算资源（例如虚拟机、容器、应用服务）、数据资源（例如 SQL 数据库和存储）以及 Azure 服务层中收集信息更丰富的安全警报。 通过此功能可查看单个资源中的帐户异常情况。

<!--Not Available on [Azure Bastion resource logs](diagnostic-logs.md)-->

- [Azure AD 中的审核活动报告](../active-directory/reports-monitoring/concept-audit-logs.md)

<!--Not Available on [Enable Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)-->

- [Azure 安全中心的威胁防护](../security-center/azure-defender.md)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3：为 Azure 网络活动启用日志记录

**指导**：启用 Azure Bastion 资源日志后，你可以使用诊断来查看哪些用户在什么时间从哪里连接到哪些工作负载，以及其他此类相关的日志记录信息。 用户可以将这些日志配置为发送到存储帐户，以进行长期保留和审核。

在网络安全组上启用和收集网络安全组 (NSG) 资源日志和 NSG 流日志，这些组将应用于已部署 Azure Bastion 资源的虚拟网络。 然后，这些日志可用于分析网络安全，并支持事件调查、威胁搜寻和安全警报生成。 可将流日志发送到 Azure Monitor Log Analytics 工作区，然后使用流量分析提供见解。

<!--Not Available on [Enable and work with Azure Bastion logs](diagnostic-logs.md)-->

- [如何启用网络安全组流日志](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Azure 防火墙日志和指标](../firewall/logs-and-metrics.md)

- [如何启用和使用流量分析](../network-watcher/traffic-analytics.md)

- [使用网络观察程序进行监视](../network-watcher/network-watcher-monitoring-overview.md)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4：为 Azure 资源启用日志记录

**指导**：自动可用的活动日志包含针对 Azure Bastion 服务资源的所有写入操作（PUT、POST、DELETE），但读取操作 (GET) 除外。 活动日志可用于在进行故障排除时查找错误，或监视组织中的用户如何对资源进行修改。

- [如何使用 Azure Monitor 收集平台日志和指标](../azure-monitor/platform/diagnostic-settings.md)

- [了解 Azure 中的日志记录和不同的日志类型](../azure-monitor/platform/platform-logs-overview.md)

<!--Not Available on [Enable Azure resource logs for Azure Bastion ](diagnostic-logs.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5：集中管理和分析安全日志

**指导**：集中记录存储和分析来实现关联。 对于每个日志源，请确保已分配数据所有者、访问指南、存储位置、用于处理和访问数据的工具以及数据保留要求。

确保正在将 Azure 活动日志集成到中央日志记录。 通过 Azure Monitor 引入日志，以聚合终结点设备、网络资源和其他安全系统生成的安全数据。 在 Azure Monitor 中，使用 Log Analytics 工作区来查询和执行分析，并使用 Azure 存储帐户进行长期存档存储。

另外，请启用 Azure Sentinel 或第三方 SIEM 并将数据载入其中。

许多组织选择将 Azure Sentinel 用于“热”数据（使用频繁的数据），将 Azure 存储用于“冷”数据（使用不太频繁的数据）。

- [如何使用 Azure Monitor 收集平台日志和指标](../azure-monitor/platform/diagnostic-settings.md)

<!--Not Available on [How to onboard Azure Sentinel](../sentinel/quickstart-onboard.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="lt-6-configure-log-storage-retention"></a>LT-6：配置日志存储保留期

**指导**：确保用于存储 Azure Bastion 日志的所有存储帐户或 Log Analytics 工作区都根据组织的合规性规定设置了日志保留期。

在 Azure Monitor 中，可根据组织的合规性规则设置 Log Analytics 工作区保持期。

- [如何配置 Log Analytics 工作区保留期](../azure-monitor/platform/manage-cost-storage.md)

- [在 Azure 存储帐户中存储资源日志](../azure-monitor/platform/resource-logs.md#send-to-azure-storage)

<!--Not Available on [Enable and work with Azure Bastions logs](diagnostic-logs.md)-->

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="incident-response"></a>事件响应

[有关详细信息，请参阅 *Azure 安全基线：* 事件响应](../security/benchmarks/security-controls-v2-incident-response.md)。

### <a name="ir-1-preparation---update-incident-response-process-for-azure"></a>IR-1：准备 - 更新 Azure 的事件响应流程

**指导**：确保组织具有响应安全事件的流程，已为 Azure 更新这些流程，并定期运用这些流程来确保就绪性。

<!--Not Available on [Implement security across the enterprise environment](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)-->

- [事件响应参考指南](https://docs.microsoft.com/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="ir-2-preparation---setup-incident-notification"></a>IR-2：准备 - 设置事件通知

**指导**：在 Azure 安全中心中设置安全事件联系人信息。 如果 Microsoft 安全响应中心 (MSRC) 发现非法或未经授权的一方访问了你的数据，Azure 将使用此联系信息来与你联系。 还可以选择基于事件响应需求在不同的 Azure 服务中自定义事件警报和通知。 

- [如何设置 Azure 安全中心安全联系人](../security-center/security-center-provide-security-contact-details.md)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="ir-3-detection-and-analysis---create-incidents-based-on-high-quality-alerts"></a>IR-3：检测和分析 - 基于高质量警报创建事件

**指导**：确保具有创建高质量警报和衡量警报质量的流程。 这样，你就可以从过去的事件中吸取经验，并为分析人员确定警报的优先级，确保他们不会浪费时间来处理误报。

可以基于过去的事件经验、经验证的社区源以及旨在通过融合和关联各种信号源来生成和清理警报的工具构建高质量警报。 

Azure 安全中心可跨许多 Azure 资产提供高质量的警报。 可以使用 ASC 数据连接器将警报流式传输到 Azure Sentinel。 借助 Azure Sentinel，可创建高级警报规则来自动生成事件以进行调查。 

使用导出功能导出 Azure 安全中心警报和建议，以帮助识别 Azure 资源的风险。 手动导出或持续导出警报和建议。

- [如何配置导出](../security-center/continuous-export.md)

<!--Not Available on [How to stream alerts into Azure Sentinel](../sentinel/connect-azure-security-center.md)-->

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="ir-4-detection-and-analysis---investigate-an-incident"></a>IR-4：检测和分析 - 调查事件

**指南**：确保分析人员在调查潜在事件时可查询和使用不同的数据源，以全面了解所发生的情况。 应收集各种各样的日志，以跟踪整个终止链中潜在攻击者的活动，避免出现盲点。  还应确保收集见解和经验，以供其他分析人员使用和用作将来的历史参考资料。  

用于调查的数据源包括已从作用域内服务和正在运行的系统中收集的集中式日志记录源，但还可以包括以下内容：

- 网络数据 - 使用网络安全组的流日志、Azure 网络观察程序和 Azure Monitor 来捕获网络流日志和其他分析信息。 

- 正在运行的系统的快照： 

    - 使用 Azure 虚拟机的快照功能创建正在运行的系统磁盘的快照。 

    - 使用操作系统的本机内存转储功能来创建正在运行的系统内存的快照。

    - 使用 Azure 服务的快照功能或软件自带的功能来创建正在运行的系统的快照。

Azure Sentinel 提供几乎针对任何日志源的广泛数据分析，并提供一个事例管理门户来管理事件的整个生命周期。 调查过程中的情报信息可与事件相关联，以便进行跟踪和报告。 

- [Windows 计算机的磁盘快照](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux 计算机的磁盘快照](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Azure 支持诊断信息和内存转储收集](https://www.azure.cn/support/legal/support-diagnostic-information-collection/) 

<!--Not Available on [Investigate incidents with Azure Sentinel](../sentinel/tutorial-investigate-cases.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="ir-5-detection-and-analysis---prioritize-incidents"></a>IR-5：检测和分析 - 确定事件的优先级

**指南**：根据警报严重性和资产敏感度，为分析人员提供上下文来确定应首要关注哪些事件。 

Azure 安全中心为每条警报分配严重性，方便你根据优先级来确定应该最先调查的警报。 严重性取决于安全中心对调查结果或用于发出警报的分析的确信程度，以及对导致警报的活动背后存在恶意意图的确信程度。

此外，使用标记来标记资源，并创建命名系统来对 Azure 资源进行标识和分类，特别是处理敏感数据的资源。  你的责任是根据发生事件的 Azure 资源和环境的关键性确定修正警报的优先级。

- [Azure 安全中心中的安全警报](../security-center/security-center-alerts-overview.md)

- [使用标记整理 Azure 资源](../azure-resource-manager/management/tag-resources.md)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="ir-6-containment-eradication-and-recovery---automate-the-incident-handling"></a>IR-6：遏制、根除和恢复 - 自动执行事件处理

**指导**：自动执行手动重复性任务来加快响应时间并减轻分析人员的负担。 执行手动任务需要更长的时间，这会导致减慢每个事件的速度，并减少分析人员可以处理的事件数量。 手动任务还会使分析人员更加疲劳，这会增加可导致延迟的人为错误的风险，并降低分析人员专注于复杂任务的工作效率。 使用 Azure 安全中心和 Azure Sentinel 中的工作流自动化功能，可自动触发操作或运行 playbook，对传入的安全警报作出响应。 playbook 执行多项操作，如发送通知、禁用帐户和隔离有问题的网络。 

- [在安全中心配置工作流自动化](../security-center/workflow-automation.md)

- [在 Azure 安全中心设置自动威胁响应](../security-center/tutorial-security-incident.md#triage-security-alerts)

<!--Not Available on [Set up automated threat responses in Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md)-->

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="posture-and-vulnerability-management"></a>安全状况和漏洞管理

有关详细信息，请参阅 [Azure 安全基准：安全状况和漏洞管理](https://docs.azure.cn/security/benchmarks/security-controls-v2-posture-vulnerability-management)。

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>PV-1：为所有 Azure 服务建立安全配置 

**指导**：使用 Azure Policy 为 Azure Bastion 定义和实施标准安全配置。 在“Microsoft.Network”命名空间中使用 Azure Policy 别名创建自定义策略，以审核或强制实施 Azure Bastion 的网络配置。 客户还可以利用 Azure 蓝图或 ARM 模板来安全、一致地部署 Bastion 资源，从而建立安全配置。

- [如何查看可用的 Azure Policy 别名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [了解 ARM 模板](../azure-resource-manager/templates/overview.md)

<!--Not Available on [Overview about Azure Blueprints](../governance/blueprints/overview.md)-->

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>PV-2：为所有 Azure 服务维护安全配置

**指导**：使用 Azure Policy 为 Azure Bastion 定义和实施标准安全配置。 在“Microsoft.Network”命名空间中使用 Azure Policy 别名创建自定义策略，以审核或强制实施 Bastion 资源的网络配置。

- [如何查看可用的 Azure Policy 别名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8：执行定期攻击模拟

**指导**：根据需要，对 Azure 资源进行渗透测试或红队活动，并确保修正所有关键安全发现。

请遵循 Microsoft 云渗透测试互动规则，确保你的渗透测试不违反 Azure 政策。 使用 Azure 红队演练策略和执行，并针对 Azure 托管云基础结构、服务和应用程序执行现场渗透测试。

- [Azure 中的渗透测试](../security/fundamentals/pen-testing.md)

- [参与的渗透测试规则](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft 云红色组合](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 安全中心监视**：不适用

**责任**：共享

## <a name="governance-and-strategy"></a>治理和策略

有关详细信息，请参阅 [Azure 安全基准：治理和策略](../security/benchmarks/security-controls-v2-governance-strategy.md)。

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1：定义资产管理和数据保护策略 

**指导**：确保为系统和数据的持续监视和保护记录并传达明确的策略。 确定业务关键数据和系统的发现、评估、保护和监视优先级。 

此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 与业务风险相符的数据分类标准

- 安全组织对风险和资产清单的洞察力 

- 安全组织对 Azure 服务使用的审批 

- 资产在其生命周期中的安全性

- 与组织数据分类相符的必需访问控制策略

- 使用 Azure 原生和第三方数据保护功能

- 传输中数据用例和静态数据用例的数据加密要求

- 合适的加密标准

有关详细信息，请参阅以下资源：

<!--Not Available on [Azure Security Architecture Recommendation - Storage, data, and encryption](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)-->

- [Azure 安全基础知识 - Azure 数据安全、加密和存储](../security/fundamentals/encryption-overview.md)

- [云采用框架 - Azure 数据安全和加密最佳做法](../security/fundamentals/data-encryption-best-practices.md?bc=%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fcloud-adoption-framework%2ftoc.json)

- [Azure 安全基准 - 资产管理](https://docs.azure.cn/security/benchmarks/security-controls-v2-asset-management)

- [Azure 安全基准 - 数据保护](https://docs.azure.cn/security/benchmarks/security-controls-v2-data-protection)

**Azure 安全中心监视**：不适用

**责任**：客户

<!--Not Available on ### GS-2: Define enterprise segmentation strategy-->
<!--Not Available on [Guidance on segmentation strategy in Azure (video)](https://docs.azure.cn/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)-->
<!--Not Available on [Guidance on segmentation strategy in Azure (document)](https://docs.azure.cn/security/compass/governance#enterprise-segmentation-strategy)-->
<!--Not Available on [Align network segmentation with enterprise segmentation strategy](https://docs.azure.cn/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)-->

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3：定义安全状况管理策略

**指导**：持续衡量并缓解你的个人资产及其托管环境的风险。 确定高价值资产和暴露程度高的受攻击面（例如已发布的应用程序、网络入口和出口点、用户和管理员终结点等）的优先级。

- [Azure 安全基准 - 状况和漏洞管理](https://docs.azure.cn/security/benchmarks/security-controls-v2-posture-vulnerability-management)

**Azure 安全中心监视**：不适用

**责任**：客户

<!--Not Available on ### GS-4: Align organization roles, responsibilities, and accountabilities-->
<!--Not Available on [Azure Security Best Practice 1 - People: Educate Teams on Cloud Security Journey](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)-->
<!--Not Available on [Azure Security Best Practice 2 - People: Educate Teams on Cloud Security Technology](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)-->
<!--Not Available on [Azure Security Best Practice 3 - Process: Assign Accountability for Cloud Security Decisions](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)-->

### <a name="gs-5-define-network-security-strategy"></a>GS-5：定义网络安全策略

**指导**：制定 Azure 网络安全方法，作为组织的整体安全访问控制策略的一部分。  

此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 集中化的网络管理和安全职责

- 符合企业分段策略的虚拟网络分段模型

- 各种威胁和攻击场景中的补救策略

- Internet 边缘及入口和出口策略

- 混合云和本地互连策略

- 最新的网络安全项目（例如网络关系图、参考网络体系结构）

有关详细信息，请参阅以下资源：

<!--Not Available on [Azure Security Best Practice 11 - Architecture. Single unified security strategy](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)-->

- [Azure 安全基准 - 网络安全](https://docs.azure.cn/security/benchmarks/security-controls-v2-network-security)

- [Azure 网络安全概述](../security/fundamentals/network-overview.md)

<!--Not Available on [Enterprise network architecture strategy](https://docs.azure.cn/cloud-adoption-framework/ready/enterprise-scale/architecture)-->

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6：定义标识和特权访问策略

**指导**：制定 Azure 标识和特权访问方法，作为组织的整体安全访问控制策略的一部分。  

此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 集中化的标识和身份验证系统及其与其他内部和外部标识系统的互连

- 各种用例和条件中的强身份验证方法

- 保护权限高的用户

- 异常用户活动监视和处理  

- 用户标识和访问评审及协调流程

有关详细信息，请参阅以下资源：

- [Azure 安全基准 - 标识管理](https://docs.azure.cn/security/benchmarks/security-controls-v2-identity-management)

- [Azure 安全基准 - 特权访问](https://docs.azure.cn/security/benchmarks/security-controls-v2-privileged-access)

<!--Not Available on [Azure Security Best Practice 11 - Architecture. Single unified security strategy](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)-->

- [Azure 标识管理安全概述](../security/fundamentals/identity-management-overview.md)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7：定义日志记录和威胁响应策略

**指导**：建立日志记录和威胁响应策略，以快速检测和修正威胁，同时满足合规性要求。 优先为分析人员提供高质量警报和无缝体验，以便他们能够专注于威胁而不是集成和手动步骤。 

此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 安全运营 (SecOps) 组织的角色和职责 

- 符合 NIST 或其他行业框架要求的明确定义的事件响应流程 

- 日志捕获和保留，用于支持威胁检测、事件响应和合规性需求

- 使用 SIEM、原生 Azure 功能和其他源，集中查看和关联有关威胁的信息 

- 与客户、供应商和公开的利益相关方之间的通信和通知计划

- 使用 Azure 原生的和第三方的平台进行事件处理，例如日志记录和威胁检测、取证以及攻击补救和根除

- 处理事件和事件后活动的流程，例如经验教训和证据保留

有关详细信息，请参阅以下资源：

- [Azure 安全基准 - 日志记录和威胁检测](https://docs.azure.cn/security/benchmarks/security-controls-v2-logging-threat-detection)

- [Azure 安全基准 - 事件响应](https://docs.azure.cn/security/benchmarks/security-controls-v2-incident-response)

<!--Not Available on [Azure Security Best Practice 4 - Process. Update Incident Response Processes for Cloud](https://docs.azure.cn/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)-->

<!--Not Available on [Azure Adoption Framework, logging, and reporting decision guide](https://docs.azure.cn/cloud-adoption-framework/decision-guides/logging-and-reporting/)-->

<!--Not Available on [Azure enterprise scale, management, and monitoring](https://docs.azure.cn/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)-->

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="next-steps"></a>后续步骤

- 参阅 [Azure 安全基准 V2 概述](../security/benchmarks/overview.md)
- 详细了解 [Azure 安全基线](../security/benchmarks/security-baselines-overview.md)

<!-- Update_Description: update meta properties, wording update, update link -->