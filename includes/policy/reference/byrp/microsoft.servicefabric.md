---
ms.service: azure-policy
ms.topic: include
origin.date: 01/08/2021
author: rockboyfor
ms.date: 01/18/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: fc60e8bce96bfe5b225db520673a2b9581ea765e
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98230850"
---
<!--Verified successfully-->
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Service Fabric 群集应将 ClusterProtectionLevel 属性设置为 EncryptAndSign](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F617c02be-7f02-4efd-8836-3180d47b6c68) |Service Fabric 使用主要群集证书为节点之间的通信提供三个保护级别（None、Sign 和 EncryptAndSign）。 设置保护级别以确保所有节点到节点消息均已进行加密和数字签名 |Audit、Deny、Disabled |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |
|[Service Fabric 群集应仅使用 Azure Active Directory 进行客户端身份验证](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb54ed75b-3e1a-44ac-a333-05ba39b99ff0) |审核 Service Fabric 中仅通过 Azure Active Directory 进行客户端身份验证 |Audit、Deny、Disabled |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |

<!-- Update_Description: new article about microsoft.servicefabric -->
<!--NEW.date: 10/19/2020-->