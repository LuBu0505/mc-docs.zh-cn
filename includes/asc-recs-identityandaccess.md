---
author: memildin
ms.service: security-center
ms.topic: include
ms.date: 03/01/2021
ms.author: v-johya
ms.custom: generated
ms.openlocfilehash: c9b30672879c0d8d2e9af7ee1165788c39ee7744
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197638"
---
这一类别有 14 条相关建议。

|建议 |说明 |严重性 |
|---|---|---|
|只多只为订阅指定 3 个所有者 |为了降低所有者帐户遭受泄露的可能性，建议将所有者帐户的数量限制为最多 3 个<br />（相关策略：[最多只能为订阅指定 3 个所有者](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4f11b553-d42e-4e3a-89be-32ca364cad4c)） |高 |
|应从订阅中删除弃用的帐户 |应从订阅中删除已被阻止登录的用户帐户。<br>这些帐户可能会成为攻击者的目标，攻击者会设法在不被发现的情况下访问你的数据。<br />（相关策略：[应从订阅中删除弃用的帐户](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6b1cbf55-e8b6-442f-ba4c-7246b6381474)） |高 |
|应从订阅中删除拥有所有者权限的已弃用帐户 |应从订阅中删除已被阻止登录的用户帐户。<br>这些帐户可能会成为攻击者的目标，攻击者会设法在不被发现的情况下访问你的数据。<br />（相关策略：[应从订阅中删除拥有所有者权限的已弃用帐户](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2febb62a0c-3560-49e1-89ed-27e074e9f8ad)） |高 |
|应启用 Key Vault 中的诊断日志 |启用日志并将其保留长达一年。 这样便可以在发生安全事件或网络遭泄露时，重新创建活动线索用于调查目的。<br />（相关策略：[应启用 Key Vault 中的诊断日志](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcf820ca0-f99e-4f3e-84fb-66e913812d21)） |低 |
|应从订阅中删除拥有所有者权限的外部帐户 |应从订阅中删除拥有所有者权限的具有不同域名的帐户（外部帐户）。 这可防止不受监视的访问。 这些帐户可能会成为攻击者的目标，攻击者会设法在不被发现的情况下访问你的数据。<br />（相关策略：[应从订阅中删除拥有所有者权限的外部帐户](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff8456c1c-aa66-4dfb-861a-25d127b775c9)） |高 |
|应从订阅中删除拥有读取权限的外部帐户 |应从订阅中删除拥有读取权限的具有不同域名的帐户（外部账户）。 这可防止不受监视的访问。 这些帐户可能会成为攻击者的目标，攻击者会设法在不被发现的情况下访问你的数据。<br />（相关策略：[应从订阅中删除拥有读取权限的外部帐户](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f5f76cf89-fbf2-47fd-a3f4-b891fa780b60)） |高 |
|应从订阅中删除具有写入权限的外部帐户 |应从订阅中删除拥有写入权限的具有不同域名的帐户（外部账户）。 这可防止不受监视的访问。 这些帐户可能会成为攻击者的目标，攻击者会设法在不被发现的情况下访问你的数据。<br />（相关策略：[应从订阅中删除具有写入权限的外部帐户](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f5c607a2e-c700-4744-8254-d77e7c9eb5e4)） |高 |
|密钥保管库应启用清除保护 |恶意删除密钥保管库可能会导致永久丢失数据。 你组织中的恶意内部人员可能会删除和清除密钥保管库。 清除保护通过强制实施软删除密钥保管库的强制保留期来保护你免受内部攻击。 你的组织内的任何人都无法在软删除保留期内清除你的密钥保管库。<br />（相关策略：[密钥保管库应启用清除保护](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0b60c0b2-2dc2-4e1c-b5c9-abbed971de53)） |中型 |
|密钥保管库应启用软删除 |在未启用软删除的情况下删除密钥保管库，将永久删除密钥保管库中存储的所有机密、密钥和证书。 意外删除密钥保管库可能会导致永久丢失数据。 软删除允许在可配置的保持期内恢复意外删除的密钥保管库。<br />（相关策略：[密钥保管库应启用软删除](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f1e66c121-a66a-4b1f-9b83-0fd99bf0fc2d)） |高 |
|应在对订阅拥有所有者权限的帐户上启用 MFA |为了防止帐户或资源出现违规问题，应为所有拥有所有者权限的订阅帐户启用多重身份验证 (MFA)。<br />（相关策略：[应在对订阅拥有所有者权限的帐户上启用 MFA](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2faa633080-8b72-40c4-a2d7-d00c03e80bed)） |高 |
|应在对订阅拥有读取权限的帐户上启用 MFA |为了防止帐户或资源出现违规问题，应为所有拥有读取特权的订阅帐户启用多重身份验证 (MFA)。<br />（相关策略：[应在对订阅拥有读取权限的帐户上启用 MFA](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fe3576e28-8b17-4677-84c3-db2990658d64)） |高 |
|应在对订阅拥有写入权限的帐户上启用 MFA |为了防止帐户或资源出现违规问题，应为所有拥有写入特权的订阅帐户启用多重身份验证 (MFA)。<br />（相关策略：[应在对订阅拥有写入权限的帐户上启用 MFA](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f9297c21d-2ed6-4474-b48f-163f75654ce3)） |高 |
|应使用服务主体（而不是管理证书）来保护你的订阅 |通过管理证书，任何使用它们进行身份验证的人员都可管理与它们关联的订阅。 为了更安全地管理订阅，建议将服务主体和资源管理器结合使用来限制证书泄露所造成的影响范围。 这也可以使资源管理自动进行。 <br />（相关策略：[应使用服务主体（而不是管理证书）来保护你的订阅](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6646a0bd-e110-40ca-bb97-84fcee63c414)） |中型 |
|应该为你的订阅分配了多个所有者 |指定多个订阅所有者，以实现管理员访问权限冗余。<br />（相关策略：[应向订阅分配多个所有者](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f09024ccc-0c5f-475e-9457-b7c0d9ed487b)） |高 |
|||

