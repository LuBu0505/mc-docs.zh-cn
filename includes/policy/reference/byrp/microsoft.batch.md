---
ms.service: azure-policy
ms.topic: include
origin.date: 01/21/2021
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: yes
ms.testdate: 09/21/2020
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 3ef17dac74e8ac7a080e889531d7be301e0acba6
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059888"
---
<!--Verified successfully-->

<!--MOONCAKE: LINES 2, 3 MEET THE SCORECARD ON this diagnostic setting-->
<!--MOONCAKE: LINES 2, 3 MEET THE SCORECARD ON (which) that -->

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Batch 帐户应使用客户管理的密钥来加密数据](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F99e9ccd8-3db9-4592-b0d1-14b1715a4d8a) |使用客户管理的密钥 (CMK) 来管理 Batch 帐户数据的静态加密。 默认情况下，使用服务管理的密钥对客户数据进行加密，但为了满足监管合规标准，通常需要使用 CMK。 CMK 允许使用由你创建并拥有的 Azure Key Vault 密钥对数据进行加密。 你可以完全控制并负责关键生命周期，包括轮换和管理。 访问 [https://aka.ms/Batch-CMK](https://aka.ms/Batch-CMK)，了解有关 CMK 加密的详细信息。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Batch/Batch_CustomerManagedKey_Audit.json) |
|[将 Batch 帐户的诊断设置部署到事件中心](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdb51110f-0865-4a6e-b274-e2e07a5b2cd7) |在创建或更新任何缺少诊断设置的 Batch 帐户时，请将该诊断设置流式部署到区域事件中心。 |DeployIfNotExists、Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Batch_DeployDiagnosticLog_Deploy_EventHub.json) |
|[将 Batch 帐户的诊断设置部署到 Log Analytics 工作区](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc84e5349-db6d-4769-805e-e14037dab9b5) |在创建或更新任何缺少诊断设置的 Batch 帐户时，请将该诊断设置流式部署到区域 Log Analytics 工作区。 |DeployIfNotExists、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Batch_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[应启用 Batch 帐户的诊断日志](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F428256e6-1fac-4f48-a757-df34c2b3336d) |审核是否已启用诊断日志。 这样便可以在发生安全事件或网络受到威胁时重新创建活动线索以用于调查目的 |AuditIfNotExists、Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditDiagnosticLog_Audit.json) |
|[应针对 Batch 帐户配置指标警报规则](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F26ee67a2-f81a-4ba8-b9ce-8550bd5ee1a7) |审核是否已针对 Batch 帐户配置指标警报规则，以启用所需指标 |AuditIfNotExists、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditMetricAlerts_Audit.json) |

<!-- Update_Description: new article about microsoft.batch -->
<!--NEW.date: 09/21/2020-->