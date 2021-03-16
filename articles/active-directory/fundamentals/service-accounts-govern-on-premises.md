---
title: 治理本地服务帐户 | Azure Active Directory
description: 服务帐户的帐户生命周期过程的创建和运行指南
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 02/20/2021
ms.author: v-junlch
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a71ba863ccec93dd3e15579703f81d1c1641eb7
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751784"
---
# <a name="governing-on-premises-service-accounts"></a>治理本地服务帐户

Windows Active Directory 中有四种类型的本地服务帐户：

* [组托管服务帐户](service-accounts-group-managed.md) (gMSA)

* [独立的托管服务帐户](service-accounts-standalone-managed.md) (sMSA)

* [计算机帐户](service-accounts-computer.md)

* [用作服务帐户的用户帐户](service-accounts-user-on-premises.md)


严格治理服务帐户至关重要，其目的是： 

* 根据服务帐户的用例要求和用途保护它们。

* 管理服务帐户及其凭据的生命周期。

* 根据服务帐户面临的风险和其具有的权限，对其进行评估。 

* 确保 Active Directory 和 Azure Active Directory 没有任何过时的、权限范围可能很大的服务帐户。

## <a name="principles-for-creating-a-new-service-account"></a>用于创建新服务帐户的原则

创建新的服务帐户时，请遵循以下准则。

| 原则| 注意事项 | 
| - |- | 
| 服务帐户映射| 将服务帐户关联到单个服务、应用程序或脚本。 |
| 所有权| 确保有主动请求并承担帐户责任的所有者。 |
| 范围| 明确定义范围，并预测服务帐户的使用期限。 |
| 用途| 为单一特定用途创建服务帐户。 |
| Privilege| 按照以下方式应用最低特权原则： <br>不要将其分配给内置组（如管理员）。<br> 删除本地计算机特权（如果适用）。<br>定制访问权限并使用 Active Directory 委托进行目录访问。<br>使用细化的访问权限。<br>在基于用户的服务帐户上设置帐户到期时间和基于位置的限制 |
| 监视和审核使用情况| 监视登录数据，确保它符合预期的使用情况。 针对异常使用情况设置警报。 |

### <a name="enforce-least-privilege-for-user-accounts-and-limit-account-overuse"></a>为用户帐户强制实施最低特权，限制帐户滥用

将以下设置用于那些用作服务帐户的用户帐户：

* [**帐户到期**](https://docs.microsoft.com/powershell/module/activedirectory/set-adaccountexpiration?view=winserver2012-ps)：将服务帐户设置为在审阅期过后的设定时间自动到期，除非确定它应该继续

*  **LogonWorkstations**：针对服务帐户可登录的位置限制权限。 如果它在计算机上本地运行，且仅访问该计算机上的资源，则将它限制为不允许从任何其他位置登录。

* [**无法更改密码**](https://docs.microsoft.com/powershell/module/addsadministration/set-aduser?view=win10-ps)：通过将参数设置为 false，禁止服务帐户更改其自己的密码。

 
## <a name="build-a-lifecycle-management-process"></a>构建生命周期管理过程

若要维护服务帐户的安全性，必须从确定需求时就管理它们，直到停用它们。 

通过以下过程对服务帐户进行生命周期管理：

1. 收集帐户的使用情况信息
1. 将服务帐户和应用加入配置管理数据库 (CMDB)
1. 执行风险评估或正式评审
1. 创建服务帐户并应用限制。
1. 计划和执行定期评审。 根据需要调整权限和范围。
1. 根据情况取消预配帐户。

### <a name="collect-usage-information-for-the-service-account"></a>收集服务帐户的使用情况信息

收集每个服务帐户的相关业务信息。 下表显示了要收集的最低限度的信息，但你应该收集为了帐户的存在而制定业务案例时所需的一切。

| 数据| 详细信息 |
| - | - |
| “所有者”| 负责服务帐户的用户或组 |
| 用途| 服务帐户的用途 |
| 权限（范围）| 预期权限集 |
| 配置管理数据库 (CMDB) 链接| 具有目标脚本/应用程序和所有者的跨链接服务帐户 |
| 风险| 基于安全风险评估的风险和业务影响评分 |
| 生存期| 对帐户过期或重新认证进行计划所需的预期最大生存期 |


 

理想情况下，应请求帐户自助服务，并要求提供相关信息。 所有者可以是应用程序或企业所有者、IT 成员或基础结构所有者。 如果将 Microsoft Forms 之类的工具用于此请求和相关的信息，则在帐户获得批准的情况下，可以轻松地将其移植到 CMDB 清单工具。

### <a name="onboard-service-account-to-cmdb"></a>将服务帐户加入 CMDB

将收集的信息存储在 CMDB 类型的应用程序中。 除了业务信息外，还包括其他基础结构、应用和进程的所有依赖项。  有了这个中央存储库，就可以更轻松地执行以下操作：

* 评估风险。

* 为服务帐户配置所需的限制。

* 了解相关的功能依赖项和安全依赖项。

* 针对安全性和持续的需求进行定期评审。

* 联系所有者以便评审、停用和更改服务帐户。

请考虑使用一个可以用来运行网站并有权连接到一个或多个 SQL 数据库的服务帐户。 为此服务帐户存储到 CMDB 中的信息可以是：

|数据 | 详细信息|
| - | - |
| 所有者、代理人| John Bloom、Anna Mayers |
| 用途| 运行 HR 网页并连接到 HR 数据库。 可以在访问数据库时模拟最终用户。 |
| 权限、范围| HR-WEBServer：本地登录、运行网页<br>HR-SQL1：本地登录、在所有 HR* 数据库中进行读取<br>HR-SQL2：本地登录、在 SALARY* 数据库中进行读取 |
| Cost Center| 883944 |
| 评估的风险| 中等；业务影响：中等；私人信息：中等 |
| 帐户限制| 登录到：仅此前提到的服务器；无法更改密码；MBI 密码策略； |
| 生存期| 无限制 |
| 评审周期| 每半年一次（按所有者、按安全团队、按隐私） |

### <a name="perform-risk-assessment-or-formal-review-of-service-account-usage"></a>执行对服务帐户使用情况的风险评估或正式评审

考虑到其权限和用途，请评估该帐户在遭到入侵的情况下可能会带给其关联应用程序或服务以及你的基础结构的风险。 请既考虑直接风险，又考虑间接风险。 

* 对手可以直接访问哪些内容？

* 服务帐户可以访问的其他信息或系统有哪些？

* 帐户是否可以用于授予其他权限？

* 如何知道权限更改的时间？

在执行完毕并进行了记录以后，风险评估可能会对以下方面产生影响：

* 帐户限制

* 帐户生存期

* 帐户评审要求（频率和审阅者）

### <a name="create-a-service-account-and-apply-account-restrictions"></a>创建服务帐户并应用帐户限制

仅当在 CMDB 中记录相关信息并执行风险评估后，才创建服务帐户。 帐户限制应该与风险评估结果相符。 请考虑以下限制（当与评估相关时）：

* [帐户到期](https://docs.microsoft.com/powershell/module/activedirectory/set-adaccountexpiration?view=winserver2012-ps)

   * 对于用作服务帐户的所有用户帐户，请定义一个实际且明确的结束使用日期。 使用“帐户到期”标志设置此项。 有关更多详细信息，请参阅 [Set-ADAccountExpiration](https://docs.microsoft.com/powershell/module/addsadministration/set-adaccountexpiration?view=win10-ps)。 

* 登录到 ([LogonWorkstation](https://docs.microsoft.com/powershell/module/addsadministration/set-aduser?view=win10-ps))

* [密码策略](/active-directory-domain-services/password-policy)要求

* 在 [OU 位置](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/delegating-administration-of-account-ous-and-resource-ous)创建，可确保仅针对特权用户进行管理

* 设置用于[检测对服务帐户的更改](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)和[服务帐户使用情况](https://www.manageengine.com/products/active-directory-audit/how-to/audit-kerberos-authentication-events.html)的审核，并收集其相关信息。

准备好投入生产时，请通过安全方式授予对服务帐户的访问权限。 

### <a name="schedule-regular-reviews-of-service-accounts"></a>计划对服务帐户的定期评审

设置对归类为中等风险和高风险的服务帐户的定期评审。 评审应包括： 

* 所有者持续需要此帐户的证明，以及需要相关特权和范围的理由。

* 按隐私和安全团队进行评审，包括对上游和下游连接进行评估。

* 来自审核的数据，确保它仅用于预期用途

### <a name="deprovision-service-accounts"></a>取消预配服务帐户

在取消预配过程中，首先删除权限和监视，然后删除帐户（如果需要）。

在以下情况下，请取消预配服务帐户：

* 为其创建服务帐户的脚本或应用程序已停用。

* 脚本或应用程序中该服务帐户所用于（例如，用于为其访问特定资源）的函数已停用。

* 已将此服务帐户替换为另一服务帐户。

删除所有权限后，请使用此过程来删除帐户。

1. 取消预配关联的应用程序或脚本后，请监视关联的服务帐户的登录和资源访问，以确保没有在其他过程中使用它。 如果你确定不再需要它，请继续下一步。

2. 禁止服务帐户登录，确保不再需要它。 针对帐户应保持禁用状态的时间创建一项业务策略。

3. 在实施“保持禁用状态”策略后删除服务帐户。 

   * 对于 MSA，你可以使用 PowerShell [卸载它](https://docs.microsoft.com/powershell/module/activedirectory/uninstall-adserviceaccount?view=winserver2012-ps)，也可以从托管服务帐户容器中手动删除它。

   * 对于计算机或用户帐户，你可以在 Active Directory 中手动删除该帐户。

## <a name="next-steps"></a>后续步骤
参阅以下文章，了解如何保护服务帐户

* [本地服务帐户简介](service-accounts-on-premises.md)

* [保护组托管服务帐户](service-accounts-group-managed.md)

* [保护独立的托管服务帐户](service-accounts-standalone-managed.md)

* [保护计算机帐户](service-accounts-computer.md)

* [保护用户帐户](service-accounts-user-on-premises.md)

* [管理本地服务帐户](service-accounts-govern-on-premises.md)
