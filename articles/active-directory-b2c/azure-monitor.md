---
title: 使用 Azure Monitor 监视 Azure AD B2C
titleSuffix: Azure AD B2C
description: 了解如何使用委托资源管理通过 Azure Monitor 记录 Azure AD B2C 事件。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.author: v-junlch
ms.subservice: B2C
ms.date: 07/27/2020
ms.openlocfilehash: 377e649ad96ff2215d9518bbf9ef0c32cea92f2a
ms.sourcegitcommit: dd2bc914f6fc2309f122b1c7109e258ceaa7c868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87297656"
---
# <a name="monitor-azure-ad-b2c-with-azure-monitor"></a>使用 Azure Monitor 监视 Azure AD B2C

使用 Azure Monitor 将 Azure Active Directory B2C (Azure AD B2C) 登录日志路由到不同的监视解决方案。 然后，可以保留日志供长期使用，或者将其与第三方安全信息和事件管理 (SIEM) 工具集成，以获取有关环境的见解。

可将日志事件路由到：

* Azure [存储帐户](../storage/blobs/storage-blobs-introduction.md)。
* [Log Analytics 工作区](../azure-monitor/platform/resource-logs-collect-workspace.md)（以分析数据、创建仪表板以及针对特定事件发出警报）。
* Azure [事件中心](../event-hubs/event-hubs-about.md)（并与 Splunk 和 Sumo Logic 实例集成）。

![Azure Monitor](./media/azure-monitor/azure-monitor-flow.png)

## <a name="prerequisites"></a>先决条件

若要完成本文中的步骤，请使用 Azure PowerShell 模块部署 Azure 资源管理器模板。

* [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps) 6.13.1 或更高版本。

## <a name="delegated-resource-management"></a>委托的资源管理

Azure AD B2C 使用 [Azure Active Directory 监视](../active-directory/reports-monitoring/overview-monitoring.md)。 

授权 Azure AD B2C 目录（**服务提供商**）中的某个用户或组在包含你的 Azure 订阅（**客户**）的租户中配置 Azure Monitor 实例。 若要创建授权，请将 [Azure 资源管理器](../azure-resource-manager/index.yml)模板部署到包含该订阅的 Azure AD 租户。 以下部分将引导你完成该过程。

## <a name="create-or-choose-resource-group"></a>创建或选择资源组

此资源组包含要从 Azure Monitor 接收数据的目标 Azure 存储帐户、事件中心或 Log Analytics 工作区。 部署 Azure 资源管理器模板时请指定资源组名称。

[创建资源组](../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups)，或者在包含你的 Azure 订阅的 Azure Active Directory (Azure AD) 租户（不是包含你的 Azure AD B2C 租户的目录）中选择现有的资源组。

此示例使用“中国东部”区域中名为 *azure-ad-b2c-monitor* 的资源组。

## <a name="delegate-resource-management"></a>委托资源管理

接下来收集以下信息：

Azure AD B2C 目录的“目录 ID”（也称为租户 ID）。

1. 以具有“用户管理员”角色（或更高）的用户身份登录到 [Azure 门户](https://portal.azure.cn/)。
1. 在门户工具栏中选择“目录 + 订阅”图标，然后选择包含 Azure AD B2C 租户的目录。
1. 依次选择“Azure Active Directory”、“属性”。 
1. 记下“目录 ID”。

要向其授予对前面在包含订阅的目录中创建的资源组的“参与者”权限的 Azure AD B2C 组或用户的**对象 ID**。

为了简化管理，建议为每个角色使用 Azure AD 用户组，这使你能够向组添加或删除单个用户，而不是直接向此用户分配权限。 在本演练中，你将添加一个用户。

1. 在 Azure 门户中仍选择了“Azure Active Directory”的情况下，选择“用户”，然后选择一个用户。 
1. 请记下该用户的“对象 ID”。

### <a name="create-an-azure-resource-manager-template"></a>创建 Azure 资源管理器模板

若要加入 Azure AD 租户（**客户**），请使用以下信息为套餐创建 Azure 资源管理器模板。 在 Azure 门户的“服务提供商”页中查看套餐详细信息时，可以看到 `mspOfferName` 和 `mspOfferDescription` 值。

| 字段   | 定义 |
|---------|------------|
| `mspOfferName`                     | 描述此定义的名称。 例如“Azure AD B2C 托管服务”。 此值将作为产品/服务的标题显示给客户。 |
| `mspOfferDescription`              | 套餐的简短说明。 例如，“在 Azure AD B2C 中启用 Azure Monitor”。|
| `rgName`                           | 前面在 Azure AD 租户中创建的资源组的名称。 例如 *azure-ad-b2c-monitor*。 |
| `managedByTenantId`                | Azure AD B2C 租户的“目录 ID”（也称为租户 ID）。 |
| `authorizations.value.principalId` | 有权访问此 Azure 订阅中的资源的 B2C 组或用户的“对象 ID”。 对于本演练，请指定前面记下的用户对象 ID。 |

下载 Azure 资源管理器模板和参数文件：

- [rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)
- [rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)

接下来，请使用前面记下的值更新参数文件。 以下 JSON 代码片段演示了 Azure 资源管理器模板参数文件的示例。 对于 `authorizations.value.roleDefinitionId`，请使用“参与者”角色的[内置角色](../role-based-access-control/built-in-roles.md)值 `b24988ac-6180-42a0-ab88-20f7382dd24c`。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Azure AD B2C Managed Services"
        },
        "mspOfferDescription": {
            "value": "Enables Azure Monitor in Azure AD B2C"
        },
        "rgName": {
            "value": "azure-ad-b2c-monitor"
        },
        "managedByTenantId": {
            "value": "<Replace with DIRECTORY ID of Azure AD B2C tenant (tenant ID)>"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "<Replace with user's OBJECT ID>",
                    "principalIdDisplayName": "Azure AD B2C tenant administrator",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                }
            ]
        }
    }
}
```

### <a name="deploy-the-azure-resource-manager-templates"></a>部署 Azure 资源管理器模板

更新参数文件后，将 Azure 资源管理器模板作为订阅级部署部署到 Azure 租户中。 由于这是订阅级部署，因此无法在 Azure 门户中启动。 可以使用 Azure PowerShell 模块或 Azure CLI 进行部署。 下面演示了 Azure PowerShell 方法。

使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/azure/authenticate-azureps) 登录到包含你的订阅的目录。 使用 `-tenant` 标志来强制对正确的目录进行身份验证。

```PowerShell
Connect-AzAccount -Environment AzureChinaCloud -tenant contoso.partner.onmschina.cn
```

使用 [Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription) cmdlet 列出 Azure AD 租户下的、可由当前帐户访问的订阅。 记下要投影到 Azure AD B2C 租户的订阅的 ID。

```PowerShell
Get-AzSubscription
```

接下来，切换至要投影到 Azure AD B2C 租户的订阅：

``` PowerShell
Select-AzSubscription <subscription ID>
```

最后，部署前面下载并更新的 Azure 资源管理器模板和参数文件。 请相应地替换 `Location`、`TemplateFile` 和 `TemplateParameterFile` 值。

```PowerShell
New-AzDeployment -Name "AzureADB2C" `
                 -Location "chinaeast" `
                 -TemplateFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.json" `
                 -TemplateParameterFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.parameters.json" `
                 -Verbose
```

成功部署模板后，会生成如下所示的输出（为简洁起见，输出已被截断）：

```Console
PS /usr/csuser/clouddrive> New-AzDeployment -Name "AzureADB2C" `
>>                  -Location "chinaeast" `
>>                  -TemplateFile "rgDelegatedResourceManagement.json" `
>>                  -TemplateParameterFile "rgDelegatedResourceManagement.parameters.json" `
>>                  -Verbose
WARNING: Breaking changes in the cmdlet 'New-AzDeployment' :
WARNING:  - The cmdlet 'New-AzSubscriptionDeployment' is replacing this cmdlet.


WARNING: NOTE : Go to https://aka.ms/azps-changewarnings for steps to suppress this breaking change warning, and other information on breaking changes in Azure PowerShell.
VERBOSE: 7:25:14 PM - Template is valid.
VERBOSE: 7:25:15 PM - Create template deployment 'AzureADB2C'
VERBOSE: 7:25:15 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:42 PM - Resource Microsoft.ManagedServices/registrationDefinitions '44444444-4444-4444-4444-444444444444' provisioning status is succeeded
VERBOSE: 7:25:48 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:53 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is running
VERBOSE: 7:25:53 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:59 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is running
VERBOSE: 7:26:17 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:23 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is succeeded
VERBOSE: 7:26:23 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:29 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is succeeded

DeploymentName          : AzureADB2C
Location                : chinaeast
ProvisioningState       : Succeeded
Timestamp               : 1/31/20 7:26:24 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name                   Type                       Value
                          =====================  =========================  ==========
                          mspOfferName           String                     Azure AD B2C Managed Services
                          mspOfferDescription    String                     Enables Azure Monitor in Azure AD B2C
...
```

部署模板后，可能需要花费几分钟时间来完成资源投影。 在转到下一部分选择订阅之前，可能需要等待几分钟（通常不超过 5 分钟）。

## <a name="select-your-subscription"></a>选择订阅

部署模板并等待几分钟让资源投影完成之后，请通过以下步骤将订阅关联到 Azure AD B2C 目录。

1. 如果当前已在 Azure 门户中登录，请**注销**。 此步骤以及下一步骤的目的是在门户会话中刷新你的凭据。
1. 使用 Azure AD B2C 管理帐户登录到 [Azure 门户](https://portal.azure.cn)。
1. 在门户工具栏中选择“目录 + 订阅”图标。
1. 选择包含订阅的目录。

    ![切换目录](./media/azure-monitor/azure-monitor-portal-03-select-subscription.png)
1. 确认是否选择了正确的目录和订阅。 在此示例中，已选择所有目录和订阅。

    ![在目录和订阅筛选器中选择了所有目录](./media/azure-monitor/azure-monitor-portal-04-subscriptions-selected.png)

## <a name="configure-diagnostic-settings"></a>配置诊断设置

诊断设置定义要将资源的日志和指标发送到的位置。 可能的目标为：

- [Azure 存储帐户](../azure-monitor/platform/resource-logs-collect-storage.md)
- [事件中心](../azure-monitor/platform/resource-logs-stream-event-hubs.md)解决方案。
- [Log Analytics 工作区](../azure-monitor/platform/resource-logs-collect-workspace.md)

如果尚未这样做，请在 [Azure 资源管理器模板](#create-an-azure-resource-manager-template)中指定的资源组内创建所选目标类型的实例。

### <a name="create-diagnostic-settings"></a>创建诊断设置

你已准备好在 Azure 门户中[创建诊断设置](../active-directory/reports-monitoring/overview-monitoring.md)。

为 Azure AD B2C 活动日志配置监视设置：

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
1. 在门户工具栏中选择“目录 + 订阅”图标，然后选择包含 Azure AD B2C 租户的目录。
1. 选择“Azure Active Directory”
1. 在“监视”下，选择“诊断设置” 。
1. 如果资源上有现有的设置，则会看到已配置的设置列表。 如果要添加新设置，请选择“添加诊断设置”；如果要编辑现有设置，请选择“编辑”设置 。 每个设置最多只能包含一个目标类型。

    ![Azure 门户中的诊断设置窗格](./media/azure-monitor/azure-monitor-portal-05-diagnostic-settings-pane-enabled.png)

1. 为设置指定名称（如果未指定）。
1. 选中要将日志发送到的每个目标对应的框。 选择“配置”并根据下表中所述指定其设置。

    | 设置 | 说明 |
    |:---|:---|
    | 存档到存储帐户 | 存储帐户的名称。 |
    | 流式传输到事件中心 | 要在其中创建事件中心的命名空间（如果这是首次流式传输日志）或要将日志流式传输到的命名空间（如果已有资源将该日志类别流式传输到此命名空间）。
    | 发送到 Log Analytics | 工作区的名称。 |

1. 选择“AuditLogs”和“SignInLogs” 。
1. 选择“保存” 。

## <a name="next-steps"></a>后续步骤

有关在 Azure Monitor 中添加和配置诊断设置的详细信息，请参阅[教程：从 Azure 资源收集和分析资源日志](../azure-monitor/insights/monitor-azure-resource.md)。

有关将 Azure AD 日志流式传输到事件中心的信息，请参阅[教程：将 Azure Active Directory 日志流式传输到 Azure 事件中心](../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)。

