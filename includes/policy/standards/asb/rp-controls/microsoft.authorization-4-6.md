---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 03/05/2021
ms.date: 03/22/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 9bf6a8e945e295af5f9dc3f70b0ffcf55a21ce3d
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766523"
---
<!--Verified successfully-->

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[审核自定义 RBAC 规则的使用情况](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa451c1ef-c6ca-483d-87ed-f49761e3ffb5) |审核“所有者、参与者、读者”等内置角色而不是容易出错的自定义 RBAC 角色。 使用自定义角色被视为例外，需要进行严格的审查和威胁建模 |Audit、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |