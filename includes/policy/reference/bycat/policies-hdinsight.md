---
ms.service: azure-policy
ms.topic: include
origin.date: 02/09/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes|no
ms.testdate: 03/01/2021null
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 6440d32bcf354ba9564f29287f0782f019590a98
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196743"
---
<!--Verified successfully-->

|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure HDInsight 群集应使用客户管理的密钥来加密静态数据](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F64d314f6-6062-4780-a861-c23e8951bee5) |使用客户管理的密钥来管理 Azure HDInsight 群集的静态加密。 默认情况下，使用服务管理的密钥对客户数据进行加密，但为了满足法规符合性标准，通常需要使用客户管理的密钥。 客户管理的密钥允许使用由你创建并拥有的 Azure Key Vault 密钥对数据进行加密。 你可以完全控制并负责关键生命周期，包括轮换和管理。 更多信息请访问 [https://aka.ms/hdi.cmk](https://aka.ms/hdi.cmk)。 |Audit、Deny、Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_CMK_Audit.json) |
|[Azure HDInsight 群集应使用主机加密来加密静态数据](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1fd32ebd-e4c3-4e13-a54a-d7422d4d95f6) |启用主机加密有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 启用主机加密时，存储在 VM 主机上的数据将静态加密，且已加密的数据将流向存储服务。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionAtHost_Audit.json) |
|[Azure HDInsight 群集应使用传输中加密来加密 Azure HDInsight 群集节点之间的通信](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd9da03a1-f3c3-412a-9709-947156872263) |在 Azure HDInsight 群集节点之间的传输过程中，数据可能会被篡改。 启用传输中加密可解决传输过程中的滥用和篡改问题。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionInTransit_Audit.json) |

<!--Update_Description: new article about policies hdinsight-->
<!--NEW.date: 03/01/2021-->