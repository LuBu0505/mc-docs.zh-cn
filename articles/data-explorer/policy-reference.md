---
title: Azure 数据资源管理器的内置策略定义
description: 列出 Azure 数据资源管理器的 Azure Policy 内置策略定义。 这些内置的策略定义提供了管理 Azure 资源的常用方法。
ms.date: 02/08/2021
ms.topic: reference
author: orspod
ms.author: v-junlch
ms.service: data-explorer
ms.custom: subject-policy-reference
ms.openlocfilehash: e00ab98e92921f278f35a35129097603c58bfa00
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101087567"
---
# <a name="azure-policy-built-in-definitions-for-azure-data-explorer"></a>Azure 数据资源管理器的 Azure Policy 内置定义

此页是 Azure 数据资源管理器的 [Azure Policy](/governance/policy/overview) 内置策略定义的索引。 有关其他服务的其他 Azure Policy 内置定义，请参阅 [Azure Policy 内置定义](/governance/policy/samples/built-in-policies)。

每个内置策略定义链接（指向 Azure 门户中的策略定义）的名称。 使用“版本”列中的链接查看 [Azure Policy GitHub 存储库](https://github.com/Azure/azure-policy)上的源。

## <a name="azure-data-explorer"></a>Azure 数据资源管理器

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure 数据资源管理器静态加密应使用客户管理的密钥](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F81e74cea-30fd-40d5-802f-d72103c2aaaa) |对 Azure 数据资源管理器群集使用客户管理的密钥启用静态加密，可提供针对静态加密所使用密钥的额外控制。 此功能通常适用于具有特殊合规性要求并且需要使用 Key Vault 管理密钥的客户。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_CMK.json) |
|[应为 Azure 数据资源管理器启用磁盘加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4b53539-8df9-40e4-86c6-6b607703bd4e) |启用磁盘加密有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_disk_encrypted.json) |
|[应为 Azure 数据资源管理器启用双重加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fec068d99-e9c7-401f-8cef-5bdde4e6ccf1) |启用双重加密有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 启用双重加密后，将使用两种不同的加密算法和两个不同的密钥对存储帐户中的数据进行两次加密，一次在服务级别，一次在基础结构级别。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_doubleEncryption.json) |
|[应为 Azure 数据资源管理器启用虚拟网络注入](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9ad2fd1f-b25f-47a2-aa01-1a5a779e6413) |通过虚拟网络注入保护网络边界，借助该虚拟网络注入，可以强制实施网络安全组规则，在本地连接并使用服务终结点保护数据连接源。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Data%20Explorer/ADX_VNET_configured.json) |

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy GitHub 存储库](https://github.com/Azure/azure-policy)中查看这些内置项。
- 查看 [Azure Policy 定义结构](/governance/policy/concepts/definition-structure)。
- 查看[了解策略效果](/governance/policy/concepts/effects)。
