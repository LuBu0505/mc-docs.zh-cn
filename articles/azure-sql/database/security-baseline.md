---
title: Azure 安全基线
description: Azure SQL 数据库和 Azure SQL 托管实例的 Azure 安全基线
author: WenJason
ms.service: security
ms.topic: conceptual
origin.date: 09/21/2020
ms.date: 01/04/2021
ms.author: v-jay
ms.custom: subject-security-benchmark
ms.openlocfilehash: 005302a87f20f1d4f4fe6e0223c0a4c206291880
ms.sourcegitcommit: b4fd26098461cb779b973c7592f951aad77351f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2021
ms.locfileid: "97856961"
---
# <a name="azure-security-baseline-for-azure-sql-database--sql-managed-instance"></a>Azure SQL Database 和 Azure SQL 托管实例的 Azure 安全基线
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

适用于 Azure SQL 数据库的 Azure 安全基线包含有助于改进部署安全状况的建议。

此服务的基线摘自 [Azure 安全基准版本 1.0](../../security/benchmarks/overview.md)，其中提供了有关如何根据我们的最佳做法指导保护 Azure 上的云解决方案的建议。

有关详细信息，请参阅 [Azure 安全基线概述](../../security/benchmarks/security-baselines-overview.md)。

## <a name="network-security"></a>网络安全

有关详细信息，请参阅[安全控制：网络安全性](../../security/benchmarks/security-control-network-security.md)。

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1：在虚拟网络中使用网络安全组或 Azure 防火墙保护资源

**指导**：可以启用 Azure 专用链接，以允许通过虚拟网络中的专用终结点访问 Azure PaaS 服务（例如，SQL 数据库）和 Azure 托管的客户服务/合作伙伴服务。 虚拟网络与服务之间的流量将通过 Microsoft 主干网络，因此不会从公共 Internet 泄露。

若要允许流量到达 Azure SQL 数据库，请使用 SQL 服务标记，以允许出站流量通过网络安全组。

虚拟网络规则使 Azure SQL 数据库仅接受从虚拟网络中的所选子网发送的通信。

如何设置 Azure SQL 数据库的专用链接：

https://docs.azure.cn/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

如何对服务器使用虚拟网络服务终结点和规则：

https://docs.azure.cn/sql-database/sql-database-vnet-service-endpoint-rule-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2：监视和记录 VNet、子网和 NIC 的配置与流量

**指导**：使用 Azure 安全中心并为 Azure SQL 数据库将部署到的子网修正网络保护建议。

对于将要连接到 Azure SQL 数据库的 Azure 虚拟机 (VM)，为保护这些 VM 的网络安全组 (NSG) 启用 NSG 流日志，然后将日志发送到 Azure 存储帐户以进行流量审核。

还可以将 NSG 流日志发送到 Log Analytics 工作区，并使用流量分析来深入了解 Azure 云中的流量流。 流量分析的优势包括能够可视化网络活动、识别热点、识别安全威胁、了解流量流模式，以及查明网络不当配置。

如何启用 NSG 流日志：

https://docs.azure.cn/network-watcher/network-watcher-nsg-flow-logging-portal

了解 Azure 安全中心提供的网络安全性：

https://docs.azure.cn/security-center/security-center-network-recommendations

如何启用和使用流量分析： 

https://docs.azure.cn/network-watcher/traffic-analytics

了解 Azure 安全中心提供的网络安全性：

https://docs.azure.cn/security-center/security-center-network-recommendations

**Azure 安全中心监视**：是

**责任**：客户

### <a name="13-protect-critical-web-applications"></a>1.3：保护关键 Web 应用程序

**指导**：不适用；此建议适用于 Azure 应用服务或托管 Web 应用程序的计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="15-record-network-packets-and-flow-logs"></a>1.5：记录网络数据包和流日志

**指导**：对于将要连接到 Azure SQL 数据库实例的 Azure 虚拟机 (VM)，为保护这些 VM 的网络安全组 (NSG) 启用 NSG 流日志，然后将日志发送到 Azure 存储帐户以进行流量审核。 启用网络观察程序数据包捕获（如果调查异常活动时有此需要）。

如何启用 NSG 流日志：

https://docs.azure.cn/network-watcher/network-watcher-nsg-flow-logging-portal

如何启用网络观察程序：

https://docs.azure.cn/network-watcher/network-watcher-create

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6：部署基于网络的入侵检测/入侵防护系统 (IDS/IPS)

**指导**：为 Azure SQL 数据库启用高级威胁防护 (ATP)。  出现可疑数据库活动、潜在漏洞、SQL 注入攻击和异常数据库访问和查询模式时，用户将收到警报。 高级威胁防护也将警报与 Azure 安全中心集成。

了解并使用 Azure SQL 数据库的高级威胁防护： https://docs.azure.cn/sql-database/sql-database-threat-detection-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="17-manage-traffic-to-web-applications"></a>1.7：管理发往 Web 应用程序的流量

**指导**：不适用；此建议适用于 Azure 应用服务或托管 Web 应用程序的计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：最大程度地降低网络安全规则的复杂性和管理开销

**指导**：使用虚拟网络服务标签来定义网络安全组或 Azure 防火墙上的网络访问控制。 创建安全规则时，可以使用服务标记代替特定的 IP 地址。 在规则的相应源或目标字段中指定服务标记名称（例如 ApiManagement），可以允许或拒绝相应服务的流量。 Azure 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记。

如果将服务终结点用于 Azure SQL 数据库，需要出站到 Azure SQL 数据库公共 IP 地址：必须为 Azure SQL 数据库 IP 启用网络安全组 (NSG) 才能进行连接。 可以使用 Azure SQL 数据库的 NSG 服务标记执行此操作。

通过 Azure SQL 数据库的服务终结点了解服务标记：

https://docs.azure.cn/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations

了解并使用服务标记：

https://docs.azure.cn/virtual-network/service-tags-overview

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="110-document-traffic-configuration-rules"></a>1.10：阐述流量配置规则

**指导**：将标记用于网络安全组 (NSG) 以及其他与网络安全和通信流有关的资源。 对于单个 NSG 规则，请使用“说明”字段针对允许流量传入/传出网络的任何规则指定业务需求和/或持续时间等。

使用标记相关的任何内置 Azure Policy 定义（例如“需要标记及其值”）来确保使用标记创建所有资源，并在有现有资源不带标记时发出通知。

可以使用 Azure PowerShell 或 Azure CLI 基于其标记对资源进行查找或执行操作。

如何创建和使用标记：

https://docs.azure.cn/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自动化工具来监视网络资源配置和检测更改

**指导**：使用 Azure 活动日志监视网络资源配置，并检测与服务器相关的网络资源的更改。 在 Azure Monitor 中创建当关键网络资源发生更改时触发的警报。

如何查看和检索 Azure 活动日志事件：

https://docs.azure.cn/azure-monitor/platform/activity-log-view

如何在 Azure Monitor 中创建警报： 

https://docs.azure.cn/azure-monitor/platform/alerts-activity-log

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="logging-and-monitoring"></a>日志记录和监视

有关详细信息，请参阅[安全控制：日志记录和监视](../../security/benchmarks/security-control-logging-monitoring.md)。

### <a name="21-use-approved-time-synchronization-sources"></a>2.1：使用批准的时间同步源

**指导**：由 Microsoft 维护 Azure 资源的时间源。 可以为计算部署更新时间同步。

如何为 Azure 计算资源配置时间同步：

https://docs.azure.cn/virtual-machines/windows/time-sync

**Azure 安全中心监视**：不适用

**责任**：Azure

### <a name="22-configure-central-security-log-management"></a>2.2：配置中心安全日志管理

**指导**：启用 Azure SQL 数据库审核以跟踪数据库事件，并将这些事件写入 Azure 存储帐户、Log Analytics 工作区或事件中心中的审核日志。

如何为 Azure SQL 数据库设置审核：

https://docs.azure.cn/sql-database/sql-database-auditing

如何使用 Azure Monitor 收集平台日志和指标：

https://docs.azure.cn/sql-database/sql-database-metrics-diag-logging

**Azure 安全中心监视**：是

**责任**：客户

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：为 Azure 资源启用审核日志记录

**指导**：在服务器上启用审核，然后为审核日志选择存储位置（Azure 存储、Log Analytics 或事件中心）。

如何启用 Azure SQL 数据库的审核：

https://docs.azure.cn/sql-database/sql-database-auditing

**Azure 安全中心监视**：是

**责任**：客户

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4：从操作系统收集安全日志

**指南**：不适用；此基准适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="25-configure-security-log-storage-retention"></a>2.5：配置安全日志存储保留期

**指导**：将 Azure SQL 数据库日志存储在 Log Analytics 工作区中时，请根据组织的合规性规则来设置日志保留期。

如何设置日志保留参数：

https://docs.azure.cn/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="26-monitor-and-review-logs"></a>2.6：监视和审查日志

**指导**：分析和监视日志中的异常行为，并定期审查结果。 使用 Azure 安全中心的高级威胁防护来对与 Azure SQL 数据库实例相关的异常活动发出警报。 或者，基于与 Azure SQL 数据库实例相关的指标值或 Azure 活动日志条目配置警报。

了解高级威胁防护和 Azure SQL 数据库的警报：

https://docs.azure.cn/sql-database/sql-database-threat-detection-overview

如何为 Azure SQL 数据库配置自定义警报：

https://docs.azure.cn/sql-database/sql-database-insights-alerts-portal?view=azps-1.4.0

**Azure 安全中心监视**：是

**责任**：客户

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7：针对异常活动启用警报

**指导**：使用适用于 Azure SQL 数据库的 Azure 安全中心高级威胁防护来监视异常活动并发出情报。 为 SQL 数据库启用 Azure Defender for SQL。 Azure Defender for SQL 包括用于呈现和减少潜在数据库漏洞，以及检测可能表明数据库有威胁的异常活动的功能。

了解高级威胁防护和 Azure SQL 数据库的警报：

https://docs.azure.cn/sql-database/sql-database-threat-detection-overview

如何为 Azure SQL 数据库启用 Azure Defender for SQL：

https://docs.azure.cn/azure-sql/database/azure-defender-for-sql

如何在 Azure 安全中心管理警报：

https://docs.azure.cn/security-center/security-center-managing-and-responding-alerts

**Azure 安全中心监视**：是

**责任**：客户

### <a name="28-centralize-anti-malware-logging"></a>2.8：集中管理反恶意软件日志记录

**指导**：不适用；对于 Azure SQL 数据库，反恶意软件解决方案由 Azure 在基础平台上管理。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="29-enable-dns-query-logging"></a>2.9：启用 DNS 查询日志记录

**指导**：不适用；DNS 日志记录不适用于 Azure SQL 数据库。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="210-enable-command-line-audit-logging"></a>2.10：启用命令行审核日志记录

**指导**：不适用；命令行审核不适用于 Azure SQL 数据库。

**Azure 安全中心监视**：不适用

**责任**：不适用

## <a name="identity-and-access-control"></a>标识和访问控制

有关详细信息，请参阅[安全控制：标识和访问控制](../../security/benchmarks/security-control-identity-access-control.md)。

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：维护管理帐户的清单

**指导**：Azure Active Directory (Azure AD) 具有必须显式分配且可查询的内置角色。 使用 Azure AD PowerShell 模块执行即席查询，以发现属于管理组成员的帐户。

如何使用 PowerShell 获取 Azure AD 中的目录角色：

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

如何使用 PowerShell 获取 Azure AD 中目录角色的成员：

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="32-change-default-passwords-where-applicable"></a>3.2：在适用的情况下更改默认密码

**指导**：Azure Active Directory 没有默认密码的概念。 预配 Azure SQL 数据库实例时，建议选择将身份验证与 Azure Active Directory 集成。

如何使用 Azure SQL 配置和管理 Azure Active Directory 身份验证： 

https://docs.azure.cn/azure-sql/database/authentication-aad-configure

**Azure 安全中心监视**：是

**责任**：客户

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用专用管理帐户

**指导**：围绕专用管理帐户的使用创建策略和过程。 使用 Azure 安全中心标识和访问管理来监视管理帐户的数量。

了解 Azure 安全中心标识和访问：

https://docs.azure.cn/security-center/security-center-identity-access

**Azure 安全中心监视**：是

**责任**：客户

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：对所有基于 Azure Active Directory 的访问使用多重身份验证

**指导**：启用 Azure Active Directory (Azure AD) 多重身份验证 (MFA)，并遵循 Azure 安全中心标识和访问管理的建议。

如何在 Azure 中启用 MFA：

https://docs.azure.cn/active-directory/authentication/howto-mfa-getstarted

如何在 Azure 安全中心监视标识和访问：

https://docs.azure.cn/security-center/security-center-identity-access

**Azure 安全中心监视**：是

**责任**：客户

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7：记录来自管理帐户的可疑活动并对其发出警报

**指导**：使用 Azure Active Directory 安全报告在环境中发生可疑活动或不安全的活动时生成日志和警报。

使用 Azure SQL 数据库的高级威胁防护可检测异常活动，指出有人在访问或利用数据库时的异常行为和可能有害的尝试。

如何在 Azure 安全中心监视用户标识和访问活动：

https://docs.azure.cn/security-center/security-center-identity-access

查看高级威胁防护和潜在警报：

https://docs.azure.cn/sql-database/sql-database-threat-detection-overview#advanced-threat-protection-alerts

**Azure 安全中心监视**：是

**责任**：客户

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：仅从批准的位置管理 Azure 资源

**指导**：使用条件访问命名位置，仅允许从 IP 地址范围或国家/地区的特定逻辑分组进行门户和 Azure 资源管理访问。

如何在 Azure 中配置命名位置： https://docs.azure.cn/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指导**：为服务器创建 Azure Active Directory (Azure AD) 管理员。

如何使用 Azure SQL 配置和管理 Azure Active Directory 身份验证： 

https://docs.azure.cn/azure-sql/database/authentication-aad-configure

如何创建和配置 Azure AD 实例：

https://docs.azure.cn/active-directory-domain-services/tutorial-create-instance

**Azure 安全中心监视**：是

**责任**：客户

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期审查和协调用户访问

**指导**：Azure Active Directory (Azure AD) 提供日志来帮助发现过时的帐户。 此外，请使用 Azure 标识访问评审来有效管理组成员身份、对企业应用程序的访问和角色分配。 可以定期评审用户的访问权限，确保只有适当的用户才能持续拥有访问权限。

如何使用 Azure 标识访问评审：

https://docs.azure.cn/active-directory/governance/access-reviews-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11：监视尝试访问已停用帐户的行为

**指导**：使用 Azure SQL 配置 Azure Active Directory (Azure AD) 身份验证，并为 Azure Active Directory 用户帐户创建诊断设置，将审核日志和登录日志发送到 Log Analytics 工作区。 在 Log Analytics 工作区中配置所需的警报。

如何使用 Azure SQL 配置和管理 Azure Active Directory 身份验证： 

https://docs.azure.cn/azure-sql/database/authentication-aad-configure

如何将 Azure 活动日志集成到 Azure Monitor 中：

https://docs.azure.cn/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="data-protection"></a>数据保护

有关详细信息，请参阅[安全控制：数据保护](../../security/benchmarks/security-control-data-protection.md)。

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：维护敏感信息的清单

**指导**：使用标记可以帮助跟踪存储或处理敏感信息的 Azure 资源。

如何创建和使用标记：

https://docs.azure.cn/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔离存储或处理敏感信息的系统

指南：为开发、测试和生产实现单独的订阅和/或管理组。 资源应当按 VNet/子网进行分隔，相应地进行标记，并在 NSG 或 Azure 防火墙中提供保护。 应当隔离用于存储或处理敏感数据的资源。 使用专用链接；在 VNet 内部署 Azure SQL 数据库，并使用专用终结点进行专用连接。

如何创建管理组：

https://docs.azure.cn/governance/management-groups/create

如何创建和使用标记：

https://docs.azure.cn/azure-resource-manager/resource-group-using-tags

如何设置 Azure SQL 数据库的专用链接：

https://docs.azure.cn/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：监视和阻止未经授权的敏感信息传输

**指导**：对于存储或处理敏感信息的 Azure SQL 数据库中的数据库，请使用标记将该数据库和相关资源标记为敏感。 结合 Azure SQL 数据库实例上的网络安全组服务标记配置专用链接，以防止敏感信息外泄。

对于 Azure 管理的底层平台，Azure 会将所有客户内容视为敏感数据，竭尽全力防范客户数据丢失和泄露。 为了确保 Azure 中的客户数据保持安全，Azure 已实施并维护一套可靠的数据保护控制机制和功能。

如何配置专用链接和 NSG，以防止 Azure SQL 数据库实例上的数据外泄：

https://docs.azure.cn/sql-database/sql-database-private-endpoint-overview

了解 Azure 中的客户数据保护：

https://docs.azure.cn/security/fundamentals/protection-customer-data

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密传输中的所有敏感信息

**指导**：Azure SQL 数据库通过使用传输层安全性加密动态数据来保护数据。 SQL 数据库始终对所有连接强制要求加密 (SSL/TLS)。 这样可以确保在客户端与服务器之间传输的所有数据经过加密，而不管连接字符串中的 Encrypt 或 TrustServerCertificate 设置如何。

了解 Azure SQL 的传输中加密：

https://docs.azure.cn/sql-database/sql-database-security-overview#information-protection-and-encryption

**Azure 安全中心监视**：不适用

**责任**：Azure

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用有效的发现工具识别敏感数据

**指导**：使用 Azure SQL 数据库数据发现和分类功能。 数据发现和分类提供了内置于 Azure SQL 数据库的高级功能，可用于发现、分类、标记和保护数据库中的敏感数据。

如何使用 Azure SQL 数据库的数据发现和分类功能：

https://docs.azure.cn/sql-database/sql-database-data-discovery-and-classification

**Azure 安全中心监视**：是

**责任**：客户

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6：使用 Azure RBAC 控制对资源的访问

**指导**：使用 Azure Active Directory (Azure AD) 来验证和控制对 Azure SQL 数据库实例的访问。

如何将 Azure SQL 数据库与 Azure Active Directory 集成来进行身份验证：

https://docs.azure.cn/sql-database/sql-database-aad-authentication

如何控制 Azure SQL 数据库中的访问：

https://docs.azure.cn/sql-database/sql-database-control-access

**Azure 安全中心监视**：是

**责任**：客户

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7：使用基于主机的数据丢失防护来强制实施访问控制

**指导**：Azure 管理 Azure SQL 数据库的底层基础结构，并实施了严格的控制措施来防止客户数据丢失或泄露。

了解 Azure 中的客户数据保护：

https://docs.azure.cn/security/fundamentals/protection-customer-data

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：静态加密敏感信息

**指导**：透明数据加密 (TDE) 通过加密静态数据，帮助保护 Azure SQL 数据库、Azure SQL 托管实例和 Azure 数据仓库免受恶意脱机活动的威胁。 它可执行静态数据库、关联备份和事务日志文件的实时加密和解密，无需更改应用程序。 默认情况下，在 SQL 数据库和 SQL 托管实例中会为所有新部署的数据库启用 TDE。 TDE 加密密钥可以由 Azure 或客户管理。

如何管理透明数据加密和使用自己的加密密钥：

https://docs.azure.cn/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#manage-transparent-data-encryption

**Azure 安全中心监视**：是

**责任**：客户

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：记录对关键 Azure 资源的更改并对此类更改发出警报

**指导**：将 Azure Monitor 与 Azure 活动日志结合使用，以创建在 Azure SQL 数据库的生产实例和其他关键资源或相关资源发生更改时发出的警报。

如何针对 Azure 活动日志事件创建警报： 

https://docs.azure.cn/azure-monitor/platform/alerts-activity-log

**Azure 安全中心监视**：是

**责任**：客户

## <a name="vulnerability-management"></a>漏洞管理

有关详细信息，请参阅[安全控制：漏洞管理。](../../security/benchmarks/security-control-vulnerability-management.md)

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1：运行自动漏洞扫描工具

**指导**：为 Azure SQL 数据库启用 Azure Defender for SQL，并遵循 Azure 安全中心关于在服务器上执行漏洞评估的建议。

如何对 Azure SQL 数据库运行漏洞评估：

https://docs.azure.cn/sql-database/sql-vulnerability-assessment

如何启用 Azure Defender for SQL：

https://docs.azure.cn/azure-sql/database/azure-defender-for-sql

如何实施 Azure 安全中心漏洞评估建议：

https://docs.azure.cn/security-center/security-center-vulnerability-assessment-recommendations

**Azure 安全中心监视**：是

**责任**：客户

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2：部署自动操作系统修补管理解决方案

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3：部署第三方自动软件修补管理解决方案

**指南**：不适用；此基准适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4：比较连续进行的漏洞扫描

**指导**：为 Azure SQL 数据库实例启用定期重复扫描；这会将漏洞评估配置为每周自动对数据库运行一次扫描。 扫描结果摘要会发送到你提供的电子邮件地址。 比较结果以验证漏洞已得到修复。

如何导出 Azure 安全中心的漏洞评估报告：

https://docs.azure.cn/sql-database/sql-vulnerability-assessment#implementing-vulnerability-assessment

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用风险评级过程来确定已发现漏洞的修正措施的优先级

**指导**：使用 Azure 安全中心提供的默认风险评级（安全功能分数）。

了解 Azure 安全中心安全功能分数：

https://docs.azure.cn/security-center/security-center-secure-score

**Azure 安全中心监视**：是

**责任**：客户

## <a name="inventory-and-asset-management"></a>清单和资产管理

有关详细信息，请参阅[安全控制：清单和资产管理](../../security/benchmarks/security-control-inventory-asset-management.md)。

### <a name="61-use-azure-asset-discovery"></a>6.1：使用 Azure 资产发现

**指导**：使用 Azure Resource Graph 查询和发现订阅中的所有资源（包括 Azure SQL 数据库）。  确保你在租户中拥有适当的（读取）权限，并且可以枚举所有 Azure 订阅，以及订阅中的资源。

尽管可以通过 Resource Graph 发现经典 Azure 资源，但我们强烈建议你今后还是创建并使用 Azure 资源管理器资源。

如何使用 Azure Resource Graph 创建查询： https://docs.azure.cn/governance/resource-graph/first-query-portal

如何查看 Azure 订阅： https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

了解 Azure RBAC： https://docs.azure.cn/role-based-access-control/overview

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="62-maintain-asset-metadata"></a>6.2：维护资产元数据

**指导**：将标记应用到 Azure资源，以便有条理地将元数据组织成某种分类。

如何创建和使用标记：

https://docs.azure.cn/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：删除未经授权的 Azure 资源

**指导**：在适用的情况下，请使用标记、管理组和单独的订阅来组织和跟踪资产。 定期核对清单，确保及时地从订阅中删除未经授权的资源。

如何创建管理组：

https://docs.azure.cn/governance/management-groups/create

如何创建和使用标记：

https://docs.azure.cn/azure-resource-manager/resource-group-using-tags

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4：维护已批准的 Azure 资源和软件标题的清单

**指导**：定义已批准的 Azure 资源以及计算资源的已批准软件的列表

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：监视未批准的 Azure 资源

**指南**：在 Azure Policy 中使用以下内置策略定义，对可以在客户订阅中创建的资源类型施加限制：

- 不允许的资源类型

- 允许的资源类型

使用 Azure Resource Graph 查询/发现订阅中的资源。 确保环境中存在的所有 Azure 资源已获得批准。

如何配置和管理 Azure Policy： https://docs.azure.cn/governance/policy/tutorials/create-and-manage

如何使用 Azure Graph 创建查询： https://docs.azure.cn/governance/resource-graph/first-query-portal

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6：监视计算资源中未批准的软件应用程序

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7：删除未批准的 Azure 资源和软件应用程序

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="68-use-only-approved-applications"></a>6.8：仅使用已批准的应用程序

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="69-use-only-approved-azure-services"></a>6.9：仅使用已批准的 Azure 服务

**指导**：使用以下内置策略定义，通过 Azure Policy 对可以在客户订阅中创建的资源类型施加限制：

- 不允许的资源类型

- 允许的资源类型

使用 Azure Resource Graph 查询/发现订阅中的资源。 确保环境中存在的所有 Azure 资源已获得批准。

如何配置和管理 Azure Policy： https://docs.azure.cn/governance/policy/tutorials/create-and-manage

如何使用 Azure Policy 拒绝特定的资源类型： https://docs.azure.cn/governance/policy/samples/not-allowed-resource-types

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="610-implement-approved-application-list"></a>6.10：实施已批准的应用程序列表

**指导**：不适用；此建议适用于在计算资源上运行的应用程序。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11：限制用户通过脚本来与 Azure 资源管理器交互的功能

**指导**：通过对“Microsoft Azure 管理”应用配置“阻止访问”，使用 Azure 条件访问来限制用户与 Azure 资源管理器交互的功能。

如何配置条件访问以阻止访问 Azure 资源管理器： https://docs.azure.cn/role-based-access-control/conditional-access-azure-management

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12：限制用户在计算资源中执行脚本的功能

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13：以物理或逻辑方式隔离高风险应用程序

**指导**：不适用；此建议适用于应用服务或托管桌面或 Web 应用程序的计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

## <a name="secure-configuration"></a>安全配置

有关详细信息，请参阅[安全控制：安全配置](../../security/benchmarks/security-control-secure-configuration.md)。

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：为所有 Azure 资源建立安全配置

**指导**：使用适用于 Azure SQL 数据库的 Azure Policy 或 Azure 安全中心建议来维护所有 Azure 资源的安全配置。

如何配置和管理 Azure Policy：

https://docs.azure.cn/governance/policy/tutorials/create-and-manage

**Azure 安全中心监视**：是

**责任**：客户

### <a name="72-establish-secure-operating-system-configurations"></a>7.2：建立安全的操作系统配置

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：维护安全的 Azure 资源配置

**指南**：使用 Azure Policy“[拒绝]”和“[不存在则部署]”对不同的 Azure 资源强制实施安全设置。

如何配置和管理 Azure Policy：

https://docs.azure.cn/governance/policy/tutorials/create-and-manage

了解 Azure Policy 效应：

https://docs.azure.cn/governance/policy/concepts/effects

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4：维护安全的操作系统配置

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全存储 Azure 资源的配置

**指导**：如果使用自定义的 Azure Policy 定义，请使用 Azure DevOps 或 Azure Repos 安全地存储和管理代码。

如何在 Azure DevOps 中存储代码：

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos 文档：

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="76-securely-store-custom-operating-system-images"></a>7.6：安全存储自定义操作系统映像

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="77-deploy-system-configuration-management-tools"></a>7.7：部署系统配置管理工具

**指导**：使用“Microsoft.SQL”命名空间中的 Azure Policy 别名创建自定义策略，以审核、强制实施系统配置并设置相关警报。 另外，开发一个用于管理策略例外的流程和管道。

如何配置和管理 Azure Policy：

https://docs.azure.cn/governance/policy/tutorials/create-and-manage

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8：为操作系统部署系统配置管理工具

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9：为 Azure 服务实施自动配置监视

**指导**：利用 Azure 安全中心对 Azure SQL 数据库执行基线扫描。

如何在 Azure 安全中心修正建议：

https://docs.azure.cn/security-center/security-center-sql-service-recommendations

**Azure 安全中心监视**：是

**责任**：客户

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10：为操作系统实施自动配置监视

**指导**：不适用；此建议适用于计算资源。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全管理 Azure 机密

**指导**：使用 Azure Key Vault 存储 Azure SQL 数据库透明数据加密 (TDE) 的加密密钥。

如何保护 Azure SQL 数据库中存储的敏感数据并将加密密钥存储在 Azure Key Vault 中：

https://docs.azure.cn/sql-database/sql-database-always-encrypted-azure-key-vault

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全自动管理标识

**指导**：使用托管标识在 Azure Active Directory (Azure AD) 中为 Azure 服务提供一个自动托管的标识。 使用托管标识可以向支持 Azure AD 身份验证的任何服务（包括 Azure Key Vault）进行身份验证，无需在代码中放入任何凭据。

教程：使用 Windows VM 系统分配托管标识访问 Azure SQL：

https://docs.azure.cn/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql

如何配置托管标识：

https://docs.azure.cn/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除意外的凭据透露

**指导**：实施凭据扫描程序来识别代码中的凭据。 凭据扫描程序还会建议将发现的凭据转移到更安全的位置，例如 Azure Key Vault。

如何设置凭据扫描器： https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure 安全中心监视**：不适用

**责任**：客户

## <a name="malware-defense"></a>恶意软件防护

有关详细信息，请参阅[安全控制：恶意软件防护](../../security/benchmarks/security-control-malware-defense.md)。

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1：使用集中管理的反恶意软件

**指导**：不适用；此建议适用于计算资源。 Microsoft 会处理基础平台的反恶意软件。

**Azure 安全中心监视**：不适用

**责任**：不适用

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：预先扫描要上传到非计算 Azure 资源的文件

**指导**：Microsoft 反恶意软件会在支持 Azure 服务（例如，Azure 应用服务）的基础主机上启用，但不会对客户内容运行。

预扫描要上传到非计算 Azure 资源（例如应用服务、Blob 存储、Azure SQL 数据库等）的任何内容。Microsoft 无法访问这些实例中的数据。

了解适用于 Azure 云服务和虚拟机的 Microsoft Antimalware： https://docs.azure.cn/security/fundamentals/antimalware

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>步骤 8.3：确保反恶意软件和签名已更新

**指南**：不适用；此建议适用于计算资源。 Microsoft 会处理基础平台的反恶意软件。

**Azure 安全中心监视**：不适用

**责任**：不适用

## <a name="data-recovery"></a>数据恢复

有关详细信息，请参阅[安全控制：数据恢复](../../security/benchmarks/security-control-data-recovery.md)。

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：确保定期执行自动备份

**指导**：为了防止企业丢失数据，Azure SQL 数据库每周自动创建完整数据库备份，每 12 小时自动创建差异数据库备份，每隔 5 - 10 分钟自动创建事务日志备份。 所有服务层级的备份在 RA-GRS 存储中存储至少 7 天。 对于时间点还原，所有服务层级（“基本”除外）都支持可配置的备份保留期，最长为 35 天。

为了满足不同的符合性要求，可为每周、每月和/或每年备份选择不同的保留期。 存储消耗量取决于所选的备份频率和保留期。

了解 Azure SQL 数据库的备份和业务连续性：

https://docs.azure.cn/sql-database/sql-database-business-continuity

**Azure 安全中心监视**：是

**责任**：共享

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：执行完整的系统备份并备份所有客户管理的密钥

**指导**：Azure SQL 数据库自动创建数据库备份（保留 7 到 35 天），并使用 Azure 读取访问异地冗余存储 (RA-GRS)，确保即使数据中心不可用也会保留这些备份。 这些备份是自动创建的。 如果需要，请为 Azure SQL 数据库启用长期异地冗余备份。

如果要将客户管理的密钥用于透明数据加密，请确保已备份你的密钥。

了解 Azure SQL 数据库中的备份：

https://docs.azure.cn/sql-database/sql-database-automated-backups?tabs=single-database

如何在 Azure 中备份密钥保管库密钥：

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure 安全中心监视**：是

**责任**：客户

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：验证所有备份，包括客户管理的密钥

**指导**：确保能够定期在 Azure 备份中执行内容数据还原。 如有必要，在隔离的 VLAN 中对还原内容进行测试。 测试对备份的客户管理的密钥进行还原。

如何在 Azure 中还原 Key Vault 密钥：

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

如何使用时间点还原来恢复 Azure SQL 数据库备份：

https://docs.azure.cn/sql-database/sql-database-recovery-using-backups#point-in-time-restore

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：确保保护备份和客户管理的密钥

**指导**：在 Azure Key Vault 中启用“软删除”，以防止意外删除或恶意删除密钥。

如何在密钥保管库中启用软删除：

https://docs.azure.cn/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure 安全中心监视**：是

**责任**：客户

## <a name="incident-response"></a>事件响应

有关详细信息，请参阅[安全控制：事件响应](../../security/benchmarks/security-control-incident-response.md)。

### <a name="101-create-an-incident-response-guide"></a>10.1：创建事件响应指导

**指导**：确保在书面的事件响应计划中定义人员职责，以及事件处理/管理的各个阶段。

如何在 Azure 安全中心配置工作流自动化：

https://docs.azure.cn/security-center/security-center-planning-and-operations-guide

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：创建事件评分和优先级设定过程

**指导**：安全中心将为警报分配严重性来帮助你确定每条警报的处理优先顺序，以便在资源泄密时可以立即采取措施。 严重性取决于安全中心在发出警报时所依据的检测结果和分析结果的置信度，以及导致发出警报的活动的恶意企图的置信度。

Azure 安全中心的安全警报： https://docs.azure.cn/security-center/security-center-alerts-overview

**Azure 安全中心监视**：是

**责任**：客户

### <a name="103-test-security-response-procedures"></a>10.3：测试安全响应过程

**指导**：定期执行演练来测试系统的事件响应功能。 识别弱点和差距，并根据需要修改计划。

可以参阅 NIST 的刊物：IT 计划和功能的测试、训练与演练计划指南：

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure 安全中心监视**：不适用

**责任**：客户

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全事件联系人详细信息，并针对安全事件配置警报通知

**指导**：如果 Microsoft 安全响应中心 (MSRC) 发现非法或未经授权的某方访问了你的数据，Azure 将使用安全事件联系人信息与你取得联系。

如何设置 Azure 安全中心安全联系人：

https://docs.azure.cn/security-center/security-center-provide-security-contact-details

**Azure 安全中心监视**：是

**责任**：客户

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：将安全警报整合到事件响应系统中

**指导**：使用连续导出功能导出 Azure 安全中心警报和建议。 使用连续导出可以手动导出或者持续导出警报和建议。 

如何配置连续导出：

https://docs.azure.cn/security-center/continuous-export

**Azure 安全中心监视**：目前不可用

**责任**：客户

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自动响应安全警报

**指导**：使用 Azure 安全中心内的工作流自动化功能可以通过“逻辑应用”针对安全警报和建议自动触发响应。

如何配置工作流自动化和逻辑应用：

https://docs.azure.cn/security-center/workflow-automation

**Azure 安全中心监视**：目前不可用

**责任**：客户

## <a name="penetration-tests-and-red-team-exercises"></a>渗透测试和红队练习

有关详细信息，请参阅[安全控制：渗透测试和红队演练](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md)。

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1：定期对 Azure 资源执行渗透测试，确保在 60 天内修正所有发现的关键安全问题

**指导**：请遵循 Azure 互动规则，确保你的渗透测试不违反 Microsoft 政策：

https://www.trustcenter.cn/zh-cn/security/penetrationtesting.html.

对于 Microsoft 红队演练策略和执行，以及针对 Microsoft 托管云基础结构、服务和应用程序的实时站点渗透测试，可在此处找到详细信息： https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure 安全中心监视**：不适用

**责任**：共享

## <a name="next-steps"></a>后续步骤

- 参阅 [Azure 安全基准](../../security/benchmarks/overview.md)
- 详细了解 [Azure 安全基线](../../security/benchmarks/security-baselines-overview.md)