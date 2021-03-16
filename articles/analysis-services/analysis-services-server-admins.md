---
title: 管理 Azure Analysis Services 中的服务器管理员 | Azure
description: 本文介绍如何使用 Azure 门户、PowerShell 或 REST API 管理 Azure Analysis Services 服务器的服务器管理员。
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 02/04/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: c2251603f1a5402e24b451d148c74c6f79d0f48f
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054364"
---
# <a name="manage-server-administrators"></a>管理服务器管理员

服务器管理员必须是 Azure Active Directory (Azure AD) 中服务器所在租户的有效用户、服务主体或安全组。 可以使用 **Analysis Services 管理员** 来管理 Azure 门户中的服务器，在 SSMS、PowerShell 或 REST API 中使用“服务器属性”来管理服务器管理员。 

添加“安全组”时，请使用 `obj:groupid@tenantid`。 添加到服务器管理员角色的安全组不支持服务主体。

若要详细了解如何将服务主体添加到服务器管理员角色，请参阅[将服务主体添加到服务器管理员角色](analysis-services-addservprinc-admins.md)。

如果已启用服务器防火墙，则必须在防火墙规则中包括服务器管理员客户端计算机的 IP 地址。 若要了解详细信息，请参阅[配置服务器防火墙](analysis-services-qs-firewall.md)。

## <a name="to-add-server-administrators-by-using-azure-portal"></a>使用 Azure 门户添加服务器管理员

1. 在门户中，对于服务器，单击“Analysis Services 管理员”。
2. 在“\<servername> - Analysis Services 管理员”中，单击“添加”。
3. 在“添加服务器管理员”中，从 Azure AD 选择用户帐户，或使用电子邮件地址邀请外部用户。

    :::image type="content" source="./media/analysis-services-server-admins/aas-manage-users-admins.png" alt-text="Azure 门户中的服务器管理员":::

## <a name="to-add-server-administrators-by-using-ssms"></a>使用 SSMS 添加服务器管理员

1. 右键单击服务器 >“属性”。
2. 在“Analysis Server 属性”中，单击“安全性”。
3. 单击“添加”，然后输入 Azure AD 中用户或组的电子邮件地址。

    :::image type="content" source="./media/analysis-services-server-admins/aas-manage-users-ssms.png" alt-text="在 SSMS 中添加服务器管理员":::

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

创建新服务器时，使用 [New-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet 指定 Administrator 参数。 <br />
使用 [Set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet 修改现有服务器的 Administrator 参数。

## <a name="rest-api"></a>REST API

创建新服务器时，使用 [Create](https://docs.microsoft.com/rest/api/analysisservices/servers/create) 指定 asAdministrator 属性。 <br />
修改现有服务器时，使用 [Update](https://docs.microsoft.com/rest/api/analysisservices/servers/update)指定 asAdministrator 属性。 <br />

## <a name="next-steps"></a>后续步骤 

[身份验证和用户权限](analysis-services-manage-users.md)  
[管理数据库角色和用户](analysis-services-database-users.md)  
[Azure 基于角色的访问控制 (Azure RBAC)](../role-based-access-control/overview.md)

<!--Update_Description: update meta properties, wording update, update link-->