---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 03/05/2021
ms.date: 03/17/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 5b6965ceebb88c7e7d31337d9b32700c83f6fffa
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766064"
---
<!--Verified successfully on 03/17/2021-->

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Kubernetes 服务应升级到不易受攻击的 Kubernetes 版本](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb893a29-21bb-418c-a157-e99480ec364c) |将 Kubernetes 服务群集升级到更高 Kubernetes 版本，以抵御当前 Kubernetes 版本中的已知漏洞。 Kubernetes 版本 1.11.9+、1.12.7+、1.13.5+ 和 1.14.0+ 中已修补漏洞 CVE-2019-9946 |Audit、Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UpgradeVersion_KubernetesService_Audit.json) |