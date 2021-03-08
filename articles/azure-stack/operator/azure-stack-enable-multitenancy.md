---
title: 在 Azure Stack Hub 中配置多租户
description: 了解如何在 Azure Stack Hub 中为来宾 Azure Active Directory 租户配置多租户。
author: WenJason
ms.topic: how-to
oigin.date: 10/26/2021
ms.date: 03/01/2021
ms.author: v-jay
ms.reviewer: bryanr
ms.lastreviewed: 01/26/2021
ms.openlocfilehash: f20f0e004656ed5d613fc67ffa3696fe4ebe0bec
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697593"
---
# <a name="configure-multi-tenancy-in-azure-stack-hub"></a>在 Azure Stack Hub 中配置多租户

可以配置 Azure Stack Hub，以支持来自其他 Azure Active Directory (Azure AD) 目录中的用户登录，允许他们使用 Azure Stack Hub 中的服务。 这些目录与你的 Azure Stack Hub 具有“来宾”关系，因此被视为来宾 Azure AD 租户。 例如，考虑以下方案：

- 你是 Azure AD 主租户 contoso.partner.onmschina.cn 的服务管理员，该租户为你的 Azure Stack Hub 提供标识和访问管理服务。
- Mary 是来宾用户所在的来宾 Azure AD 租户 fabrikam.partner.onmschina.cn 的目录管理员。
- Mary 的公司 (Fabrikam) 使用你公司提供的 IaaS 和 PaaS 服务。 Fabrikam 想要让来宾目录 (fabrikam.partner.onmschina.cn) 中的所有用户都能够登录并使用受 contoso.partner.onmschina.cn 保护的 Azure Stack Hub 资源。

本指南提供了在这种情况下有关为来宾目录租户在 Azure Stack Hub 中启用或禁用多租户的相关步骤。 你和 Mary 通过注册/注销来宾目录租户来完成此过程，这将启用/禁用 Fabrikam 用户的 Azure Stack Hub 登录和服务使用。 

如果你是云解决方案提供商 (CSP)，则可以通过其他方式[配置和管理多租户 Azure Stack Hub](azure-stack-add-manage-billing-as-a-csp.md)。 

## <a name="prerequisites"></a>先决条件

注册或注销来宾目录之前，你和 Mary 必须完成各自 Azure 租户的管理步骤：Azure Stack Hub 主目录 (Contoso) 和来宾目录 (Fabrikam)：

 - [安装](powershell-install-az-module.md)和[配置](azure-stack-powershell-configure-admin.md)适用于 Azure Stack Hub 的 PowerShell。
 - [下载 Azure Stack Hub 工具](azure-stack-powershell-download.md)，并导入“连接和标识”模块：

    ```powershell
    Import-Module .\Identity\AzureStack.Identity.psm1
    ```

## <a name="register-a-guest-directory"></a>注册来宾目录

若要注册用于多租户的来宾目录，将需要同时配置 Azure Stack Hub 主目录和来宾目录。

#### <a name="configure-azure-stack-hub-directory"></a>配置 Azure Stack Hub 目录

作为 contoso.partner.onmschina.cn 的服务管理员，你必须首先将 Fabrikam 的来宾目录租户加入 Azure Stack Hub。 以下脚本将配置 Azure 资源管理器以接受来自 fabrikam.partner.onmschina.cn 租户中的用户和服务主体的登录：

```powershell  
## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint, formatted as adminmanagement.<region>.<FQDN>.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack Hub directory
$azureStackDirectoryTenant = "contoso.partner.onmschina.cn"

## Replace the value below with the guest directory tenant. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.partner.onmschina.cn"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group.
$location = "local"

# Subscription Name
$SubscriptionName = "Default Provider Subscription"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName `
 -SubscriptionName $SubscriptionName
```

#### <a name="configure-guest-directory"></a>配置来宾目录

接下来，Mary（Fabrikam 的目录管理员）必须通过运行以下脚本在 fabrikam.partner.onmschina.cn 来宾目录中注册 Azure Stack Hub：

```powershell
## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint, formatted as management.<region>.<FQDN>.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest directory tenant.
$guestDirectoryTenantName = "fabrikam.partner.onmschina.cn"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose
```

> [!IMPORTANT]
> 如果你的 Azure Stack Hub 管理员将来安装新服务或更新，则你可能需要再次运行此脚本。
>
> 随时可以再次运行此脚本来检查目录中的 Azure Stack Hub 应用的状态。
>
> 如果已注意到在托管磁盘中创建 VM 时存在的问题（在 1808 更新中引入），则已添加新的 **磁盘资源提供程序**，从而需要再次运行此脚本。

### <a name="direct-users-to-sign-in"></a>指导用户登录

最后，Mary 可以通过访问 [Azure Stack Hub 用户门户](../user/azure-stack-use-portal.md)来指导具有 @fabrikam.partner.onmschina.cn 帐户的 Fabrikam 用户登录。 对于多节点系统，用户门户 URL 的格式为 `https://management.<region>.<FQDN>`。 对于 ASDK 部署，URL 为 `https://portal.local.azurestack.external`。

Mary 还必须指导任何外部主体（Fabrikam 目录中没有 fabrikam.partner.onmschina.cn 后缀的用户）使用 `https://<user-portal-url>/fabrikam.partner.onmschina.cn` 登录。 如果他们未在此 URL 中指定 `/fabrikam.partner.onmschina.cn` 目录租户，则会被发送到其默认目录，并收到一个错误，指出其管理员未同意。

## <a name="unregister-a-guest-directory"></a>注销来宾目录

如果你不再需要允许从来宾目录租户登录到 Azure Stack Hub 服务，则可以注销该目录。 同样，此行为也需要配置 Azure Stack Hub 主目录和来宾目录：

1. 以来宾目录的管理员身份（在此场景中为 Mary）运行 `Unregister-AzsWithMyDirectoryTenant`。 该 cmdlet 从新目录中卸载所有 Azure Stack Hub 应用。

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint, formatted as management.<region>.<FQDN>.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest directory tenant.
    $guestDirectoryTenantName = "fabrikam.partner.onmschina.cn"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. 作为 Azure Stack Hub 的服务管理员（在此场景中是指你），请运行 `Unregister-AzSGuestDirectoryTenant` cmdlet：

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint, formatted as adminmanagement.<region>.<FQDN>.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack Hub directory
    $azureStackDirectoryTenant = "contoso.partner.onmschina.cn"
    
    ## Replace the value below with the guest directory tenant. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.partner.onmschina.cn"
    
    ## Replace the value below with the name of the resource group in which the directory tenant resource was created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > 禁用多租户步骤必须按顺序执行。 如果首先完成步骤 1，则步骤 2 将失败。

## <a name="retrieve-azure-stack-hub-identity-health-report"></a>检索 Azure Stack Hub 标识运行状况报告 

替换 `<region>`、`<domain>` 和 `<homeDirectoryTenant>` 占位符，然后以 Azure Stack Hub 管理员的身份执行以下 cmdlet。

```powershell

$AdminResourceManagerEndpoint = "https://adminmanagement.<region>.<domain>"
$DirectoryName = "<homeDirectoryTenant>.partner.onmschina.cn"
$healthReport = Get-AzsHealthReport -AdminResourceManagerEndpoint $AdminResourceManagerEndpoint -DirectoryTenantName $DirectoryName
Write-Host "Healthy directories: "
$healthReport.directoryTenants | Where status -EQ 'Healthy' | Select -Property tenantName,tenantId,status | ft


Write-Host "Unhealthy directories: "
$healthReport.directoryTenants | Where status -NE 'Healthy' | Select -Property tenantName,tenantId,status | ft
```

## <a name="update-azure-ad-tenant-permissions"></a>更新 Azure AD 租户权限

此操作将清除 Azure Stack Hub 中的警报，指示目录需要更新。 从 Azurestack-tools-master/identity 文件夹运行以下命令：

```powershell
Import-Module ..\Identity\AzureStack.Identity.psm1

$adminResourceManagerEndpoint = "https://adminmanagement.<region>.<domain>"

# This is the primary tenant Azure Stack Hub is registered to:
$homeDirectoryTenantName = "<homeDirectoryTenant>.partner.onmschina.cn"

Update-AzsHomeDirectoryTenant -AdminResourceManagerEndpoint $adminResourceManagerEndpoint `
   -DirectoryTenantName $homeDirectoryTenantName -Verbose
```

该脚本会提示你提供 Azure AD 租户的管理凭据，并且需要几分钟才能运行。 运行 cmdlet 后，应该会清除警报。

## <a name="next-steps"></a>后续步骤

- [管理委派提供程序](azure-stack-delegated-provider.md)
- [Azure Stack Hub 的重要概念](azure-stack-overview.md)
- [管理充当云解决方案提供商的 Azure Stack Hub 的用量和计费](azure-stack-add-manage-billing-as-a-csp.md)
- [将租户添加到 Azure Stack Hub 以获取用量和计费信息](azure-stack-csp-howto-register-tenants.md)
