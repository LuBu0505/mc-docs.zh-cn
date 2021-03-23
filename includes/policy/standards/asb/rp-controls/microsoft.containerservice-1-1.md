---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 03/05/2021
ms.date: 03/22/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 4e2d3d59f389ae6ebab466391a23e879a9b4dde3
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766065"
---
<!--Verified successfully on 03/07/2021-->

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[应在 Kubernetes 服务上定义经授权的 IP 范围](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e246bcf-5f6f-4f87-bc6f-775d4712c7ea) |通过仅向特定范围内的 IP 地址授予 API 访问权限，来限制对 Kubernetes 服务管理 API 的访问。 建议将访问权限限制给已获授权的 IP 范围，以确保只有受允许网络中的应用程序可以访问群集。 |Audit、Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableIpRanges_KubernetesService_Audit.json) |