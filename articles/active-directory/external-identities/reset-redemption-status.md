---
title: 重置来宾用户的兑换状态 - Azure AD
description: 了解如何在 Azure AD 外部标识中重置 Azure Active Directory B2B 来宾用户的邀请兑换状态。
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 02/20/2021
ms.author: v-junlch
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef29067381c0efed3bb2f090ba500f4fd2a40074
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751813"
---
# <a name="reset-redemption-status-for-a-guest-user"></a>重置来宾用户的邀请兑换状态

在来宾用户兑换你的 B2B 协作邀请后，有时候，你可能需要更新其登录信息，例如在以下情况下：

- 用户需要使用其他电子邮件和标识提供者进行登录
- 用户在主租户中的帐户已被删除或重新创建
- 用户已转移到其他公司，但他们仍然需要对你的资源具有相同的访问权限
- 用户的职责已转移给其他用户

以前，若要管理这些方案，你必须从你的目录中手动删除来宾用户的帐户，然后重新邀请用户。 现在，你可以使用 PowerShell 或 Microsoft Graph 邀请 API 来重置用户的兑换状态并重新邀请用户，并保留用户的对象 ID、组成员身份和应用分配。 当用户兑换新邀请时，用户的 UPN 不会更改，但用户的登录名将更改为新电子邮件地址。 然后，用户可以使用新电子邮件地址或你添加到用户对象的 `otherMails` 属性的电子邮件地址进行登录。

## <a name="use-powershell-to-reset-redemption-status"></a>使用 PowerShell 重置兑换状态

安装最新的 AzureADPreview PowerShell 模块，创建一个新邀请并将 `InvitedUserEMailAddress` 设置为新的电子邮件地址，将 `ResetRedemption` 设置为 `true`。

```powershell  
Uninstall-Module AzureADPreview 
Install-Module AzureADPreview 
Connect-AzureAD -AzureEnvironmentName AzureChinaCloud
$ADGraphUser = Get-AzureADUser -objectID "UPN of User to Reset"  
$msGraphUser = New-Object Microsoft.Open.MSGraph.Model.User -ArgumentList $ADGraphUser.ObjectId 
New-AzureADMSInvitation -InvitedUserEmailAddress <<external email>> -SendInvitationMessage $True -InviteRedirectUrl "https://account.activedirectory.windowsazure.cn/r#/applications" -InvitedUser $msGraphUser -ResetRedemption $True 
```

## <a name="use-microsoft-graph-api-to-reset-redemption-status"></a>使用 Microsoft Graph API 重置兑换状态

使用 [Microsoft Graph 邀请 API](https://docs.microsoft.com/graph/api/resources/invitation?view=graph-rest-1.0)，将 `resetRedemption` 属性设置为 `true` 并在 `invitedUserEmailAddress` 属性中指定新的电子邮件地址。

```json
POST https://microsoftgraph.chinacloudapi.cn/beta/invitations  
Authorization: Bearer eyJ0eX...  
ContentType: application/json  
{  
   "invitedUserEmailAddress": "<<external email>>",  
   "sendInvitationMessage": true,  
   "invitedUserMessageInfo": {  
      "messageLanguage": "en-US",  
      "ccRecipients": [  
         {  
            "emailAddress": {  
               "name": null,  
               "address": "<<optional additional notification email>>"  
            }  
         } 
      ],  
      "customizedMessageBody": "<<custom message>>"  
},  
"inviteRedirectUrl": "https://account.activedirectory.windowsazure.cn/r#/applications?tenantId=",  
"invitedUser": {  
   "id": "<<ID for the user you want to reset>>"  
}, 
"resetRedemption": true 
}
```

## <a name="next-steps"></a>后续步骤

- [使用 PowerShell 添加 Azure Active Directory B2B 协作用户](customize-invitation-api.md#powershell)
- [Azure AD B2B 来宾用户的属性](user-properties.md)
