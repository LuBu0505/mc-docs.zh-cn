---
title: 适用于 Azure 数据资源管理器的 Azure 安全基线
description: Azure 数据资源管理器安全基线为实现 Azure 安全基准中指定的安全建议提供过程指南和资源。
author: msmbaldwin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/18/2021
ms.author: v-junlch
ms.custom: subject-security-benchmark
ms.openlocfilehash: 3945d27481b3ad3fbf6aa6f50fa60736164357d5
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766094"
---
# <a name="azure-security-baseline-for-azure-data-explorer"></a>适用于 Azure 数据资源管理器的 Azure 安全基线

此安全基线将 [Azure 安全基准版本 1.0](/security/benchmarks/overview-v1) 中的指南应用于 Azure 数据资源管理器。 Azure 安全基准提供有关如何在 Azure 上保护云解决方案的建议。
内容按“安全控制”分组，这些控制根据适用于 Azure 数据资源管理器的 Azure 安全基准和相关指南定义。 排除了不适用于 Azure 数据资源管理器的“控制”。

 
若要查看 Azure 数据资源管理器如何完全映射到 Azure 安全基准，请参阅[完整的 Azure 数据资源管理器安全基线映射文件](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)。

## <a name="network-security"></a>网络安全

[有关详细信息，请参阅 *Azure 安全基线：* 网络安全性](/security/benchmarks/security-control-network-security)。

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1：保护虚拟网络中的 Azure 资源

**指导**：Azure 数据资源管理器支持将群集部署到虚拟网络中的子网中。 通过此功能，你可以在 Azure 数据资源管理器群集流量上强制实施网络安全组 (NSG) 规则，将本地网络连接到 Azure 数据资源管理器群集的子网，以及通过服务终结点保护数据连接源（事件中心和事件网格）。


**Azure 安全中心监视**：是

**责任**：客户

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2：监视并记录虚拟网络、子网和网络接口的配置与流量

**指导**：启用网络安全组 (NSG) 流日志，并将日志发送到存储帐户以进行流量审核。

- [如何启用 NSG 流日志](/network-watcher/network-watcher-nsg-flow-logging-portal)

- [了解 Azure 安全中心提供的网络安全](/security-center/security-center-network-recommendations)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4：拒绝与已知恶意的 IP 地址进行通信

**指导**：在虚拟网络上启用 Azure DDoS 防护标准，防止 Azure 数据资源管理器群集受到 DDoS 攻击。 根据 Azure 安全中心集成的威胁情报进行判断，拒绝与已知恶意的或未使用过的 Internet IP 地址通信。


- [了解 Azure 安全中心集成的威胁情报](/security-center/security-center-alerts-service-layer)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="15-record-network-packets"></a>1.5：记录网络数据包

**指导**：在用于保护 Azure 数据资源管理器群集的网络安全组 (NSG) 上启用流日志，并将日志发送到存储帐户以进行流量审核。

- [如何启用 NSG 流日志](/network-watcher/network-watcher-nsg-flow-logging-portal)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：最大程度地降低网络安全规则的复杂性和管理开销

**指导**：在与 Azure 数据资源管理器群集关联的网络安全组或 Azure 防火墙中使用虚拟网络服务标记来定义网络访问控制。 创建安全规则时，可以使用服务标记代替特定的 IP 地址。 在规则的相应源或目标字段中指定服务标记名称（例如 ApiManagement），可以允许或拒绝相应服务的流量。 Microsoft 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记。

- [了解并使用服务标记](/virtual-network/service-tags-overview)


**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9：维护网络设备的标准安全配置

**指导**：客户使用 Azure Policy 为网络资源定义并实施标准安全配置。

客户还可以使用 Azure 蓝图，通过在单个蓝图定义中打包关键环境项目（例如 Azure 资源管理器模板、Azure RBAC 控制和 Azure Policy 分配）来简化大规模的 Azure 部署。 轻松将蓝图应用到新的订阅和环境，并通过版本控制来微调控制措施和管理。

- [如何配置和管理 Azure Policy](/governance/policy/tutorials/create-and-manage)


**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="110-document-traffic-configuration-rules"></a>1.10：阐述流量配置规则

**指导**：将标记用于网络安全组 (NSG) 以及与 Azure 数据资源管理器群集的网络安全和通信流相关的其他资源。 对于单个 NSG 规则，请使用“说明”字段针对允许流量传入/传出网络的任何规则指定业务需求和/或持续时间等。

- [如何创建和使用标记](/azure-resource-manager/resource-group-using-tags)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自动化工具来监视网络资源配置和检测更改

**指导**：使用 Azure Policy 来验证（和/或修正）网络资源的配置。

- [如何配置和管理 Azure Policy](/governance/policy/tutorials/create-and-manage)

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="logging-and-monitoring"></a>日志记录和监视

[有关详细信息，请参阅 *Azure 安全基线：* 日志记录和监视](/security/benchmarks/security-control-logging-monitoring)。

### <a name="22-configure-central-security-log-management"></a>2.2：配置中心安全日志管理

**指导**：Azure 数据资源管理器使用诊断日志获取有关引入成功和失败的见解。 可将操作日志导出到 Azure 存储、事件中心或 Log Analytics 以监视引入状态。

- [如何监视 Azure 数据资源管理器引入操作](./using-diagnostic-logs.md)

- [如何在 Azure 数据资源管理器中引入和查询监视数据](./ingest-data-no-code.md)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：为 Azure 资源启用审核日志记录

**指导**：启用 Azure 数据资源管理器的诊断设置以访问和记录服务专用操作和日志记录。 默认情况下，Azure Monitor 中的 Azure 活动日志（包括有关资源的概要日志记录）处于启用状态。

- [如何监视 Azure 数据资源管理器引入操作](./using-diagnostic-logs.md)

- [如何使用 Azure Monitor 收集平台日志和指标](/azure-monitor/platform/diagnostic-settings)

- [Azure 平台日志概述](/azure-monitor/platform/platform-logs-overview)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="25-configure-security-log-storage-retention"></a>2.5：配置安全日志存储保留期

**指导**：在 Azure Monitor 中，根据组织的合规性规章设置 Log Analytics 工作区保留期。 使用 Azure 存储帐户进行长期/存档存储。

- [如何为 Log Analytics 工作区设置日志保留参数](/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="26-monitor-and-review-logs"></a>2.6：监视和查看日志

**指导**：分析和监视日志中的异常行为，并定期审查结果。 启用 Azure 数据资源管理器的诊断设置后，使用 Azure Monitor 的 Log Analytics 工作区查看日志并对日志数据执行查询。

- [了解 Log Analytics 工作区](/azure-monitor/log-query/get-started-portal)

- [如何在 Azure Monitor 中执行自定义查询](/azure-monitor/log-query/get-started-queries)

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="identity-and-access-control"></a>标识和访问控制

[有关详细信息，请参阅 *Azure 安全基线：* 标识和访问控制](/security/benchmarks/security-control-identity-access-control)。

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：维护管理帐户的清单

**指导**：在 Azure 数据资源管理器中，安全角色定义哪些安全主体（用户和应用程序）有权对受保护的资源（例如数据库或表）进行操作，以及允许进行哪些操作。 可以利用 Kusto 查询列出 Azure 数据资源管理器群集和数据库的具有管理员角色的主体。


**Azure 安全中心监视**：是

**责任**：客户

### <a name="32-change-default-passwords-where-applicable"></a>3.2：在适用的情况下更改默认密码

**指导**：Azure Active Directory (Azure AD) 没有默认密码的概念。 其他需要密码的 Azure 资源会强制创建具有复杂性要求和最小密码长度的密码，该长度因服务而异。 你对可能使用默认密码的第三方应用程序和市场服务负责。

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用专用管理帐户

**指导**：客户围绕专用管理帐户的使用创建标准操作程序。 使用 Azure 安全中心标识和访问管理来监视管理帐户的数量。

客户还可以通过使用 Microsoft 服务的 Azure Active Directory (Azure AD) Privileged Identity Management 特权角色和 Azure ARM 来启用实时/恰好足够的访问权限。 

- [Privileged Identity Management](/active-directory/privileged-identity-management/pim-configure)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4：使用 Azure Active Directory 单一登录 (SSO)

**指导**：客户应尽可能使用 Azure Active Directory (Azure AD) SSO，而不是为每个服务配置单个独立凭据。 请使用 Azure 安全中心标识和访问管理建议。


**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：对所有基于 Azure Active Directory 的访问使用多重身份验证

**指导**：启用 Azure Active Directory (Azure AD) 多重身份验证 (MFA)，并遵循 Azure 安全中心标识和访问管理的建议。

- [如何在 Azure 中启用 MFA](/active-directory/authentication/howto-mfa-getstarted)

- [如何在 Azure 安全中心监视标识和访问](/security-center/security-center-identity-access)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6：使用由 Azure 管理的安全工作站执行管理任务

**指导**：使用配置了多重身份验证 (MFA) 的 PAW（特权访问工作站）来登录和配置 Azure 资源。

- [了解特权访问工作站](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [如何在 Azure 中启用 MFA](/active-directory/authentication/howto-mfa-getstarted)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7：记录来自管理帐户的可疑活动并对其发出警报

**指导**：当环境中出现可疑或不安全的活动时，请使用 Azure Active Directory (Azure AD) 安全报告来生成日志和警报。 使用 Azure 安全中心监视标识和访问活动


- [如何在 Azure 安全中心监视用户的标识和访问活动](/security-center/security-center-identity-access)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：仅从批准的位置管理 Azure 资源

**指导**：用户使用条件访问命名位置，仅允许从 IP 地址范围或国家/地区的特定逻辑分组进行访问。

- [如何在 Azure 中配置命名位置](/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指导**：Azure Active Directory (Azure AD) 是向 Azure 数据资源管理器进行身份验证的首选方法。 Azure AD 支持多种身份验证方案：

- 用户身份验证（交互式登录）：用于对人类主体进行身份验证。

- 应用程序身份验证（非交互式登录）：用于对必须在没有人类用户参与的情况下运行或进行身份验证的服务和应用程序进行身份验证。


**Azure 安全中心监视**：是

**责任**：客户

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期审查和协调用户访问

**指导**：Azure Active Directory (Azure AD) 提供日志来帮助发现过时的帐户。 此外，请使用 Azure 标识访问评审来有效管理组成员身份、对企业应用程序的访问和角色分配。 可以定期评审用户的访问权限，确保只有适当的用户才持续拥有访问权限。 


- [Azure AD 报告](/active-directory/reports-monitoring/)

- [如何使用 Azure 标识访问评审](/active-directory/governance/access-reviews-overview)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11：监视尝试访问已停用凭据的行为

**指导**：可使用 Azure Active Directory (Azure AD) 登录活动、审核和风险事件日志源进行监视，这让你可以与任何安全信息和事件管理 (SIEM)/监视工具集成。

 你可以通过为 Azure Active Directory 用户帐户创建诊断设置，并将审核日志和登录日志发送到 Log Analytics 工作区，来简化此过程。 客户可以在 Log Analytics 工作区中配置所需的警报。

- [如何将 Azure 活动日志集成到 Azure Monitor](/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12：针对帐户登录行为偏差发出警报

**指导**：使用 Azure Active Directory (Azure AD) 风险检测和标识保护功能配置对检测到的与用户标识相关的可疑操作的自动响应。 此外，可将数据引入 Azure Sentinel 以做进一步调查。


**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13：在支持场合下为 Microsoft 提供对相关客户数据的访问权限

**指导**：在 Microsoft 需要访问客户数据的支持方案中，客户密码箱为客户提供接口来审核和批准/拒绝客户数据访问请求。


**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="data-protection"></a>数据保护

[有关详细信息，请参阅 *Azure 安全基线：* 数据保护](/security/benchmarks/security-control-data-protection)。

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：维护敏感信息的清单

**指导**：使用标记可以帮助跟踪存储或处理敏感信息的 Azure 资源。

- [如何创建和使用标记](/azure-resource-manager/resource-group-using-tags)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔离存储或处理敏感信息的系统

**指导**：为开发、测试和生产实现单独的订阅和/或管理组。 Azure 数据资源管理器群集应当按虚拟网络/子网从其他资源进行分隔，相应地进行标记，并在网络安全组 (NSG) 或 Azure 防火墙中进行保护。 存储或处理敏感数据的 Azure 数据资源管理器群集应当充分隔离。


- [如何创建管理组](/governance/management-groups/create)

- [如何创建和使用标记](/azure-resource-manager/resource-group-using-tags)


- [如何创建采用安全配置的 NSG](/virtual-network/tutorial-filter-network-traffic)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密传输中的所有敏感信息

**指导**：默认情况下，Azure 数据资源管理器群集协商 TLS 1.2。 确保连接到 Azure 资源的任何客户端能够协商 TLS 1.2 或更高版本。

**Azure 安全中心监视**：不适用

**责任**：共享

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用有效的发现工具识别敏感数据

**指导**：数据标识、分类和丢失防护功能尚不适用于 Azure 数据资源管理器。 如果需要出于合规性目的使用这些功能，请实施第三方解决方案。

对于 Microsoft 管理的底层平台，Microsoft 会将所有客户内容视为敏感数据，并会全方位地防范客户数据丢失和遭到透露。 为了确保 Azure 中的客户数据保持安全，Microsoft 实施并维护了一套可靠的数据保护控制措施和功能。

- [了解 Azure 中的客户数据保护](/security/fundamentals/protection-customer-data)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6：使用基于角色的访问控制来控制对资源的访问

**指导**：通过 Azure 数据资源管理器，可以使用 Azure 基于角色的访问控制 (RBAC) 模型来控制对数据库和表的访问。 在此模型下，主体（用户、组和应用）将映射到角色。 主体可以根据分配的角色访问资源。

- [角色和权限列表以及有关如何为 Azure 数据资源管理器配置 RBAC 的说明](./manage-database-permissions.md)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：静态加密敏感信息

**指导**：Azure 磁盘加密有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 它为群集虚拟机的 OS 和数据磁盘提供卷加密。 它还与 Azure 密钥保管库集成，让我们可以控制和管理磁盘加密密钥和机密，并确保 VM 磁盘上的所有数据在 Azure 存储中进行静态加密。

- [如何为 Azure 数据资源管理器群集启用静态加密](./cluster-disk-encryption.md)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：记录对关键 Azure 资源的更改并对此类更改发出警报

**指导**：将 Azure Monitor 与 Azure 活动日志配合使用，以创建在 Azure 数据资源管理器上发生资源级别更改时发出的警报。

- [如何针对 Azure 活动日志事件创建警报](/azure-monitor/platform/alerts-activity-log)

**Azure 安全中心监视**：是

**责任**：客户

## <a name="vulnerability-management"></a>漏洞管理

[有关详细信息，请参阅 *Azure 安全基线：* 漏洞管理。](/security/benchmarks/security-control-vulnerability-management)

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用风险评级过程来确定已发现漏洞的修正措施的优先级

**指导**：使用 Azure 安全中心提供的默认风险评级（安全功能分数）。

- [了解 Azure 安全中心安全功能分数](/security-center/security-center-secure-score)

**Azure 安全中心监视**：是

**责任**：客户

## <a name="inventory-and-asset-management"></a>清单和资产管理

[有关详细信息，请参阅 *Azure 安全基线：* 清单和资产管理](/security/benchmarks/security-control-inventory-asset-management)。

### <a name="61-use-automated-asset-discovery-solution"></a>6.1：使用自动化资产发现解决方案

**指导**：使用 Azure Resource Graph 查询和发现订阅中的所有资源。 确保租户中具有适当的（读取）权限，并枚举所有 Azure 订阅以及订阅中的资源。

尽管可以通过 Resource Graph 发现经典 Azure 资源，但我们强烈建议你今后还是创建并使用 Azure 资源管理器资源。

- [如何使用 Azure Resource Graph 创建查询](/governance/resource-graph/first-query-portal)

- [如何查看 Azure 订阅](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?preserve-view=true&view=azps-4.8.0)

- [了解 Azure RBAC](/role-based-access-control/overview)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="62-maintain-asset-metadata"></a>6.2：维护资产元数据

**指导**：将标记应用到 Azure资源，以便有条理地将元数据组织成某种分类。

- [如何创建和使用标记](/azure-resource-manager/resource-group-using-tags)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：删除未经授权的 Azure 资源

**指导**：在适用的情况下，你可以使用适当的命名约定、标记、管理组或单独的订阅来组织和跟踪资产。 你可以使用 Azure Resource Graph 定期核对清单，确保及时地从订阅中删除未经授权的资源。 


- [如何创建管理组](/governance/management-groups/create)

- [如何创建和使用标记](/azure-resource-manager/resource-group-using-tags)

- [如何使用 Azure Resource Graph 创建查询](/governance/resource-graph/first-query-portal)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4：定义并维护已批准的 Azure 资源的清单

**指导**：你将需要根据组织需求，创建已获批 Azure 资源以及已获批用于计算资源的软件的清单。

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：监视未批准的 Azure 资源

**指导**：可以在 Azure 策略中使用以下内置策略定义，对可以在客户订阅中创建的资源类型施加限制：

- 不允许的资源类型

- 允许的资源类型

你将能够使用活动日志（可使用 Azure Monitor 对其进行监视）监视策略生成的事件。

此外，可以使用 Azure Resource Graph 来查询/发现订阅中的资源。

- [创建和管理策略以强制实施符合性](/governance/policy/tutorials/create-and-manage)

- [快速入门：使用 Azure Resource Graph 资源管理器运行你的第一个 Resource Graph 查询](/governance/resource-graph/first-query-portal)

- [使用 Azure Monitor 创建、查看和管理活动日志警报](/azure-monitor/platform/alerts-activity-log)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="69-use-only-approved-azure-services"></a>6.9：仅使用已批准的 Azure 服务

**指导**：可以在 Azure Policy 中使用以下内置策略定义，对可以在客户订阅中创建的资源类型施加限制：

 - 不允许的资源类型

 - 允许的资源类型

有关详细信息，请参阅以下资源：

- [创建和管理策略以强制实施符合性](/governance/policy/tutorials/create-and-manage)

- [Azure Policy 示例](/governance/policy/samples/built-in-policies#general)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11：限制用户与 Azure 资源管理器进行交互的能力

**指导**：通过为“Azure 管理”应用配置“阻止访问”，使用 Azure 条件访问来限制用户与 Azure 资源管理器交互的功能。 这会阻止在 Azure 订阅中创建和更改资源。 

- [使用条件访问管理对 Azure 管理的访问权限](/role-based-access-control/conditional-access-azure-management)

**Azure 安全中心监视**：是

**责任**：客户

## <a name="secure-configuration"></a>安全配置

[有关详细信息，请参阅 *Azure 安全基线：* 安全配置](/security/benchmarks/security-control-secure-configuration)。

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：为所有 Azure 资源建立安全配置

**指导**：使用 Azure Policy 别名创建自定义策略，审核或强制实施 Azure 资源的配置。 你还可以使用内置的 Azure Policy 定义。

此外，Azure 资源管理器能够以 JavaScript 对象表示法 (JSON) 导出模板，应该对其进行检查，以确保配置满足或超过组织的安全要求。

还可以使用来自 Azure 安全中心的建议作为 Azure 资源的安全配置基线。

- [如何查看可用的 Azure Policy 别名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?preserve-view=true&view=azps-4.8.0)

- [教程：创建和管理策略以强制实施符合性](/governance/policy/tutorials/create-and-manage)

- [在 Azure 门户中将单资源和多资源导出到模板](/azure-resource-manager/templates/export-template-portal)

- [安全建议 - 参考指南](/security-center/recommendations-reference)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：维护安全的 Azure 资源配置

**指导**：使用 Azure 策略“[拒绝]”和“[不存在则部署]”对不同的 Azure 资源强制实施安全设置。  可以使用更改跟踪、策略符合性仪表板等解决方案或自定义解决方案来轻松识别环境中的安全更改。

- [了解 Azure Policy 效果](/governance/policy/concepts/effects)

- [创建和管理策略以强制实施符合性](/governance/policy/tutorials/create-and-manage)


- [获取 Azure 资源的符合性数据](/governance/policy/how-to/get-compliance-data)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全存储 Azure 资源的配置

**指导**：使用 Azure Repos 安全地存储和管理代码，如自定义 Azure 策略、Azure 资源管理器模板、Desired State Configuration 脚本等。若要访问在 Azure DevOps 中管理的资源，可以向特定用户、内置安全组或 Azure Active Directory (Azure AD)（如果与 Azure DevOps 集成）中定义的组或 Active Directory（如果与 TFS 集成）授予或拒绝授予权限。


**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7：部署 Azure 资源的配置管理工具

**指导**：使用 Azure Policy 为 Azure 资源定义和实施标准安全配置。 使用 Azure Policy 别名创建自定义策略，审核或强制实施 Azure 资源的网络配置。 还可以使用与特定资源相关的内置策略定义。  此外，你也可以使用 Azure 自动化来部署配置更改。

- [如何配置和管理 Azure Policy](/governance/policy/tutorials/create-and-manage)

- [如何使用别名](/governance/policy/concepts/definition-structure#aliases)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9：为 Azure 资源实施自动配置监视

**指导**：使用 Azure Policy 别名创建自定义策略，以审核、强制实施系统配置并为其发出警报。 使用 Azure Policy“[审核]”、“[拒绝]”和“[不存在则部署]”自动强制实施 Azure 资源的配置。

- [如何配置和管理 Azure Policy](/governance/policy/tutorials/create-and-manage)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全自动管理标识

**指导**：使用托管标识在 Azure Active Directory (Azure AD) 中为 Azure 服务提供一个自动托管的标识。 使用托管标识可以向支持 Azure AD 身份验证的任何服务（包括 Key Vault）进行身份验证，无需在代码中放入任何凭据。

- [如何配置托管标识](/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

- [配置 Azure 数据资源管理器群集的托管标识](./managed-identities.md)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除意外的凭据透露

**指南**：实施凭据扫描程序来识别代码中的凭据。 凭据扫描程序还会建议将发现的凭据转移到更安全的位置，例如 Azure Key Vault。 

- [如何设置凭据扫描程序](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="malware-defense"></a>恶意软件防护

[有关详细信息，请参阅 *Azure 安全基线：* 恶意软件防护](/security/benchmarks/security-control-malware-defense)。

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：预先扫描要上传到非计算 Azure 资源的文件

**指导**：在支持 Azure 服务（例如 Azure 数据资源管理器）的底层主机上已启用 Microsoft Antimalware，但是，该软件不会针对客户内容运行。

预扫描要上传到非计算 Azure 资源的任何内容，例如 Azure 数据资源管理器、Data Lake Storage、Blob 存储、Azure Database for PostgreSQL 等。Microsoft 无法访问这些实例中的数据。

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="data-recovery"></a>数据恢复

[有关详细信息，请参阅 *Azure 安全基线：* 数据恢复](/security/benchmarks/security-control-data-recovery)。

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：确保定期执行自动备份

**指导**：始终复制 Azure 存储帐户中 Azure 数据资源管理器群集使用的数据，以确保持续性和高可用性。 Azure 存储功能会复制数据，以防范各种计划内和计划外的事件，包括暂时性的硬件故障、网络中断或断电、大范围自然灾害等。 可以选择在同一数据中心中、跨同一区域中的局域数据中心或跨地理上隔离的区域复制数据。

- [了解 Azure 存储冗余和服务级别协议](/storage/common/storage-redundancy)


**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：执行完整系统备份，并备份客户管理的所有密钥

**指导**：Azure 数据资源管理器对静态存储帐户中的所有数据进行加密。 默认情况下，数据使用 Microsoft 管理的密钥进行加密。 为了更进一步控制加密密钥，可以提供客户管理的密钥来用于对数据进行加密。 客户管理的密钥必须存储在 Azure密钥保管库中。

- [如何备份 Azure Key Vault 证书](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultcertificate?preserve-view=true&view=azps-4.8.0)

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：验证所有备份，包括客户管理的密钥

**指南**：定期测试 Azure Key Vault 机密的数据还原。

- [如何还原 Azure Key Vault 证书](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultcertificate?preserve-view=true&view=azps-4.8.0)



**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：确保保护备份和客户管理的密钥

**指导**：客户在 Key Vault 中启用“软删除”，以防止意外删除或恶意删除密钥。  你还可以启用清除保护，这样在保留期结束之前，便无法清除处于已删除状态的保管库或对象。  

- [如何在密钥保管库中启用软删除和清除保护](/key-vault/key-vault-ovw-soft-delete)


**Azure 安全中心监视**：是

**责任**：客户

## <a name="incident-response"></a>事件响应

[有关详细信息，请参阅 *Azure 安全基线：* 事件响应](/security/benchmarks/security-control-incident-response)。

### <a name="101-create-an-incident-response-guide"></a>10.1：创建事件响应指导

**指南**：为组织制定事件响应指南。 确保在书面的事件响应计划中定义人员职责，以及事件处理/管理从检测到事件后审查的各个阶段。
    

- [关于建立自己的安全事件响应流程的指南](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

    

- [Microsoft 安全响应中心的事件剖析](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

    

- [客户还可利用 NIST 的“计算机安全事件处理指南”来帮助创建自己的事件响应计划](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：创建事件评分和优先级设定过程

**指导**：安全中心为每条警报分配严重性，以帮助你优先处理应该最先调查的警报。 严重性取决于安全中心在发出警报时所依据的检测结果和分析结果的置信度，以及导致发出警报的活动的恶意企图的置信度。

此外，请明确标记订阅（例如 生产、非生产）并创建命名系统来对 Azure 资源进行明确标识和分类，特别是处理敏感数据的资源。  你的责任是根据发生事件的 Azure 资源和环境的关键性确定修正警报的优先级。

- [Azure 安全中心中的安全警报](/security-center/security-center-alerts-overview)

- [使用标记整理 Azure 资源](/azure-resource-manager/resource-group-using-tags)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="103-test-security-response-procedures"></a>10.3：测试安全响应过程

**指南**：定期执行演练来测试系统的事件响应功能，以帮助保护 Azure 资源。 识别弱点和差距，并根据需要修改计划。
    

- [请参阅 NIST 的出版物：Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)（IT 规划和功能的测试、培训与演练计划指南）

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全事件联系人详细信息，并针对安全事件配置警报通知

**指导**：如果 Microsoft 安全响应中心 (MSRC) 发现数据被某方非法访问或未经授权访问，Microsoft 会使用安全事件联系信息联系用户。 事后审查事件，确保问题得到解决。
    

- [如何设置 Azure 安全中心安全联系人](/security-center/security-center-provide-security-contact-details)

**Azure 安全中心监视**：是

**责任**：客户

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：将安全警报整合到事件响应系统中

**指南**：使用连续导出功能导出 Azure 安全中心警报和建议，以帮助确定 Azure 资源的风险。 使用连续导出可以手动导出或者持续导出警报和建议。 可以使用 Azure 安全中心数据连接器将警报流式传输到 Azure Sentinel。
    

- [如何配置连续导出](/security-center/continuous-export)

    


**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自动响应安全警报

**指导**：使用 Azure 安全中心内的工作流自动化功能，通过“逻辑应用”针对安全警报和建议自动触发响应，以保护 Azure 资源。
    

- [如何配置工作流自动化和逻辑应用](/security-center/workflow-automation)

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="penetration-tests-and-red-team-exercises"></a>渗透测试和红队练习

[有关详细信息，请参阅 *Azure 安全基线：* 渗透测试和红队演练](/security/benchmarks/security-control-penetration-tests-red-team-exercises)。

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1：定期对 Azure 资源执行渗透测试，确保修正所有发现的关键安全问题

**指导**：请遵循 Microsoft 云渗透测试互动规则，确保你的渗透测试不违反 Microsoft 政策。 使用 Microsoft 红队演练策略和执行，以及针对 Microsoft 托管云基础结构、服务和应用程序执行现场渗透测试。 

- [参与的渗透测试规则](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft 云红色组合](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 安全中心监视**：不适用

**责任**：共享

## <a name="next-steps"></a>后续步骤

- 参阅 [Azure 安全基准 V2 概述](/security/benchmarks/overview)
- 详细了解 [Azure 安全基线](/security/benchmarks/security-baselines-overview)
