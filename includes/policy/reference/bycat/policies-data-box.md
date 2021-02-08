---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 01/21/2021
ms.date: 02/08/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 6a7e4ca69e00b9f13c27beb75101fb0b7918e123
ms.sourcegitcommit: 0232a4d5c760d776371cee66b1a116f6a5c850a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99580612"
---
<!--Verified successfully-->
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Data Box 作业应为设备上静态数据启用双重加密](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc349d81b-9985-44ae-a8da-ff98d108ede8) |为设备上的静态数据启用第二层基于软件的加密。 设备已通过用于静态数据的高级加密标准 256 位加密受到保护。 此选项将添加第二层数据加密。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Box/DataBox_DoubleEncryption_Audit.json) |
|[Azure Data Box 作业应使用客户管理的密钥加密设备解锁密码](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86efb160-8de7-451d-bc08-5d475b0aadae) |使用客户管理的密钥控制 Azure Data Box 的设备解锁密码的加密。 客户管理的密钥还有助于通过 Data Box 服务管理对设备解锁密码的访问，以便以自动方式准备设备和复制数据。 设备本身的数据已使用高级加密标准 256 位加密进行静态加密，并且设备解锁密码默认使用 Microsoft 托管密钥进行加密。 |Audit、Deny、Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Box/DataBox_CMK_Audit.json) |