---
title: Azure 数据资源管理器的 Azure Policy 法规合规性控制措施
description: 列出可用于 Azure 数据资源管理器的 Azure Policy 法规合规性控制措施。 这些内置的策略定义提供了管理 Azure 资源符合性的常用方法。
ms.date: 03/16/2021
ms.topic: sample
author: orspod
ms.author: v-junlch
ms.service: data-explorer
ms.custom: subject-policy-compliancecontrols
ms.openlocfilehash: 73f9a79fc0f8c42c63051c7d44076f0f3c4fb02a
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767661"
---
# <a name="azure-policy-regulatory-compliance-controls-for-azure-data-explorer"></a>Azure 数据资源管理器的 Azure Policy 法规合规性控制措施

Azure Policy 中的法规符合性为与不同符合性标准相关的“符合域”和“安全控件”提供 Microsoft 创建和管理的计划定义，称为“内置” 。 本页面列出了 Azure 数据资源管理器的合规性域和安全控制措施 。 可以分别为“安全控件”分配内置项，以帮助 Azure 资源符合特定的标准。

每个内置策略定义链接（指向 Azure 门户中的策略定义）的标题。 使用“策略版本”列中的链接查看 [Azure Policy GitHub 存储库](https://github.com/Azure/azure-policy)上的源。

> [!IMPORTANT]
> 下面的每个控件都与一个或多个 [Azure Policy](/governance/policy/overview) 定义关联。 这些策略有助于[评估控制的合规性](/governance/policy/how-to/get-compliance-data)；但是，控制与一个或多个策略之间通常不是一对一或完全匹配。 因此，Azure Policy 中的符合性仅引用策略本身；这不确保你完全符合控件的所有要求。 此外，符合性标准包含目前未由任何 Azure Policy 定义处理的控件。 因此，Azure Policy 中的符合性只是整体符合性状态的部分视图。 这些符合性标准的控制措施和 Azure Policy 法规符合性定义之间的关联可能会随着时间的推移而发生变化。

## <a name="cmmc-level-3"></a>CMMC 级别 3

|域 |控制 ID |控制标题 |策略<br /><sub>（Azure 门户）</sub> |策略版本<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|系统和通信保护 |SC.3.177 |采用经 FIPS 验证的加密系统来保护 CUI 的机密性。 |[Azure 数据资源管理器静态加密应使用客户管理的密钥](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F81e74cea-30fd-40d5-802f-d72103c2aaaa) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_CMK.json) |
|系统和通信保护 |SC.3.177 |采用经 FIPS 验证的加密系统来保护 CUI 的机密性。 |[应为 Azure 数据资源管理器启用磁盘加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4b53539-8df9-40e4-86c6-6b607703bd4e) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_disk_encrypted.json) |
|系统和通信保护 |SC.3.177 |采用经 FIPS 验证的加密系统来保护 CUI 的机密性。 |[应为 Azure 数据资源管理器启用双重加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fec068d99-e9c7-401f-8cef-5bdde4e6ccf1) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_doubleEncryption.json) |
|系统和通信保护 |SC.3.191 |保护静态 CUI 的机密性。 |[应为 Azure 数据资源管理器启用磁盘加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4b53539-8df9-40e4-86c6-6b607703bd4e) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_disk_encrypted.json) |
|系统和通信保护 |SC.3.191 |保护静态 CUI 的机密性。 |[应为 Azure 数据资源管理器启用双重加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fec068d99-e9c7-401f-8cef-5bdde4e6ccf1) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_doubleEncryption.json) |

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy GitHub 存储库](https://github.com/Azure/azure-policy)中查看这些内置项。
