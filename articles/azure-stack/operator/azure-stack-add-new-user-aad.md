---
title: 在 Azure Active Directory 中添加新的 Azure Stack Hub 用户帐户
description: 了解如何在 Azure Active Directory 中创建用户帐户，以便浏览用户门户。
author: WenJason
ms.topic: article
origin.date: 05/20/2019
ms.date: 03/01/2021
ms.author: v-jay
ms.reviewer: thoroet
ms.lastreviewed: 09/17/2019
ms.openlocfilehash: ac3e2d251b8946711d62cffd9f7c6f718f4873d0
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101696811"
---
# <a name="add-a-new-azure-stack-hub-user-account-in-azure-active-directory-azure-ad"></a>在 Azure Active Directory (Azure AD) 中添加新的 Azure Stack Hub 用户帐户

需要有用户帐户才能测试套餐和计划并创建资源。 使用 Azure 门户或 PowerShell 在 Azure AD 租户中创建用户帐户。

## <a name="create-user-account-using-the-azure-portal"></a>使用 Azure 门户创建用户帐户

必须具有 Azure 订阅才能使用 Azure 门户。

1. 登录到 [Azure](https://portal.azure.cn)。
2. 在左侧导航栏中，选择“Active Directory”并切换到要用于 Azure Stack Hub 的目录（或创建一个新目录）。
3. 选择“Azure Active Directory” > “用户” > “新建用户”。  

    ![用户 - 突出显示新用户的所有用户页面](media/azure-stack-add-new-user-aad/new-user-all-users.png)

4. 在“用户”页上，填写所需的信息。

    ![添加新用户，具有用户信息的用户页](media/azure-stack-add-new-user-aad/new-user-user.png)

   - **姓名(必填)** ：新用户的名字和姓氏。 例如，Mary Parker。
   - **用户名(必填)** ：新用户的用户名。 例如 mary@contoso.com。
       用户名的域名部分必须是初始默认域名 <_yourdomainname_>.partner.onmschina.cn，或者是一个自定义域名，例如 contoso.com。 若要详细了解如何创建自定义域名，请参阅[如何向 Azure AD 添加自定义域名](/active-directory/fundamentals/add-custom-domain)。
   - **个人资料**：（选填）可以添加关于用户的详细信息。 也可以在以后添加用户信息。 有关添加用户信息的详细信息，请参阅[如何添加或更改用户个人资料信息](/active-directory/fundamentals/active-directory-users-profile-azure-portal)。
   - **目录角色**：选择“用户”。

5. 选中“显示密码”并复制“密码”框中提供的自动生成的密码。 在初始登录过程中需要此密码。

6. 选择“创建”。

    用户已创建并添加到 Azure AD 租户。

7. 使用新帐户登录到 Azure 门户。 出现提示时，更改密码。
8. 使用新帐户登录到 `https://portal.local.azurestack.external`，以查看用户门户。

## <a name="create-a-user-account-using-powershell"></a>使用 PowerShell 创建用户帐户

如果没有 Azure 订阅，则无法使用 Azure 门户添加租户用户帐户。 在这种情况下，可以改用适用于 Windows PowerShell 的 Azure AD 模块。

> [!NOTE]
> 如果使用 Microsoft 帐户部署 ASDK，则不能使用 Azure AD PowerShell 创建租户帐户。

1. 安装 **64 位** 版本的 [适用于 IT 专业人员的 Microsoft Online Services 登录助手 RTW](https://www.microsoft.com/download/details.aspx?id=28177)。

2. 按照以下步骤安装适用于 Windows PowerShell 的 Azure AD 模块：

    - 打开提升的 Windows PowerShell 命令提示符（以管理员身份运行 Windows PowerShell）。
    - 运行 **Install-Module MSOnline** 命令。
    - 如果提示安装 NuGet 提供程序，请选择 **Y** 和 **Enter**。
    - 如果提示从 PSGallery 安装模块，请选择 **Y** 和 **Enter**。

3. 运行以下 cmdlet：

    ```powershell
    # Provide the Azure AD credential you use to deploy the ASDK.

            $msolcred = get-credential

    # Add a user account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. 使用新帐户登录到 Azure。 出现提示时，更改密码。
2. 使用新帐户登录到 `https://portal.local.azurestack.external`，以查看用户门户。

## <a name="next-steps"></a>后续步骤

[在 AD FS 中添加 Azure Stack Hub 用户](azure-stack-add-users-adfs.md)