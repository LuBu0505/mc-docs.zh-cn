---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/18/2021
ms.author: v-junlch
ms.custom: generated
ms.openlocfilehash: 6ee67b832d3b86c9dd457fa44101809cfd89e47a
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751042"
---
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Cache for Redis 应驻留在虚拟网络中](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7d092e0a-7acd-40d2-a975-dca21cae48c4) |Azure 虚拟网络部署为 Azure Cache for Redis 提供了增强的安全性和隔离，以及子网、访问控制策略和其他功能，以进一步限制访问。配置了虚拟网络的 Azure Cache for Redis 实例是不可公开寻址的，只能从虚拟网络中的虚拟机和应用程序访问。 |Audit、Deny、Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_CacheInVnet_Audit.json) |
|[只能与 Azure Cache for Redis 建立安全连接](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |审核是否仅启用通过 SSL 来与 Azure Redis 缓存建立连接。 使用安全连接可确保服务器和服务之间的身份验证并保护传输中的数据免受中间人攻击、窃听攻击和会话劫持等网络层攻击 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
