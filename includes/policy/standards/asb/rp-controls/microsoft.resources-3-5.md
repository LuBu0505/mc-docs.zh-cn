---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 03/05/2021
ms.date: 03/22/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 84de0e3096d75aecf9cf5a07d636f46f8f485a98
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766522"
---
<!--Verified successfully-->

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[应对订阅中拥有写入权限的帐户启用 MFA](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9297c21d-2ed6-4474-b48f-163f75654ce3) |为了防止帐户或资源出现违规问题，应为所有拥有写入特权的订阅帐户启用多重身份验证 (MFA)。 |AuditIfNotExists、Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForWritePermissions_Audit.json) |
|[应在对订阅拥有所有者权限的帐户上启用 MFA](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faa633080-8b72-40c4-a2d7-d00c03e80bed) |为了防止帐户或资源出现违规问题，应为所有拥有所有者权限的订阅帐户启用多重身份验证 (MFA)。 |AuditIfNotExists、Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForOwnerPermissions_Audit.json) |
|[应在对订阅拥有读取权限的帐户上启用 MFA](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe3576e28-8b17-4677-84c3-db2990658d64) |为了防止帐户或资源出现违规问题，应为所有拥有读取特权的订阅帐户启用多重身份验证 (MFA)。 |AuditIfNotExists、Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForReadPermissions_Audit.json) |