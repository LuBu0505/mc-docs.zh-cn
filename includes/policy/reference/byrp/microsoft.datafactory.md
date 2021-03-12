---
author: WenJason
ms.service: azure-policy
ms.topic: include
origin.date: 02/09/2021
ms.date: 03/01/2021
ms.author: v-jay
ms.custom: generated
ms.openlocfilehash: 24dbfdcbe1020908d987d3602cefa9d37c89675b
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751825"
---
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[应使用客户管理的密钥对 Azure 数据工厂进行加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4ec52d6d-beb7-40c4-9a9e-fe753254690e) |使用客户管理的密钥来管理 Azure 数据工厂的静态加密。 默认情况下，使用服务管理的密钥对客户数据进行加密，但为了满足法规合规性标准，通常需要使用客户管理的密钥。 客户管理的密钥允许使用由你创建并拥有的 Azure Key Vault 密钥对数据进行加密。 你可以完全控制并负责关键生命周期，包括轮换和管理。 更多信息请访问 [https://docs.azure.cn/data-factory/enable-customer-managed-key](https://docs.azure.cn/data-factory/enable-customer-managed-key)。 |Audit、Deny、Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/DataFactory_CustomerManagedKey_Audit.json) |
|[Azure 数据工厂集成运行时应对核心数有限制](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F85bb39b5-2f66-49f8-9306-77da3ac5130f) |若要管理资源和成本，请限制集成运行时的核心数。 |Audit、Deny、Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/IR_Core_Count_Exceeds_Audit.json) |
|[Azure 数据工厂链接服务资源类型应在允许列表中](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6809a3d0-d354-42fb-b955-783d207c62a8) |定义 Azure 数据工厂链接服务类型的允许列表。 限制允许的资源类型可以控制数据移动的边界。 例如，将范围限制为只允许使用 Data Lake Storage Gen2 进行分析的 blob 存储，或只允许 SQL 和 Kusto 访问实时查询。 |Audit、Deny、Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/LinkedService_ResourceType_Audit.json) |
|[Azure 数据工厂链接服务应使用 Key Vault 来存储机密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F127ef6d7-242f-43b3-9eef-947faf1725d0) |为了确保机密（如连接字符串）得到安全管理，需要用户使用 Azure Key Vault 提供机密，而不是在链接服务中内联指定它们。 |Audit、Deny、Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/LinkedService_InlineSecrets_Audit.json) |
|[Azure 数据工厂链接服务应使用系统分配的托管标识身份验证（如果受支持）](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff78ccdb4-7bf4-4106-8647-270491d2978a) |在通过链接服务与数据存储进行通信时，使用系统分配的托管标识可以避免使用密码或连接字符串等安全性较低的凭据。 |Audit、Deny、Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/LinkedService_All_Auth_Audit_except_MSI.json) |
