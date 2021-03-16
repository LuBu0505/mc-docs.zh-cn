---
title: 排查常见错误
description: 了解如何排查为 Kubernetes 创建策略定义、各种 SDK 和加载项时遇到的问题。
origin.date: 01/26/2021
author: rockboyfor
ms.date: 03/01/2021
ms.author: v-yeche
ms.topic: troubleshooting
ms.openlocfilehash: 056189e316d68ecc00b93140b14592b7cda7c493
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196892"
---
# <a name="troubleshoot-errors-with-using-azure-policy"></a>排查使用 Azure Policy 时出现的错误

创建策略定义、使用 SDK 时，可能会遇到错误。 本文介绍描述可能会发生的各种常见错误，并提供建议的解决方法。

<!--NOT AVAILABLE ON [Azure Policy for Kubernetes](../concepts/policy-for-kubernetes.md)-->

## <a name="find-error-details"></a>查找错误详细信息

错误详细信息的位置取决于使用的是 Azure Policy 的哪个部分。

- 如果使用的是自定义策略，请转到 Azure 门户以获取有关架构的 Lint 分析反馈，或查看生成的[符合性数据](../how-to/get-compliance-data.md)以了解资源的评估方式。
- 如果使用的是各种 SDK，则 SDK 提供有关函数失败原因的详细信息。

<!--NOT AVAILABLE ON ../concepts/policy-for-kubernetes.md-->

## <a name="general-errors"></a>常规错误

### <a name="scenario-alias-not-found"></a>方案：找不到别名

#### <a name="issue"></a>问题

策略定义中使用的别名不正确或不存在。 Azure Policy 使用[别名](../concepts/definition-structure.md#aliases)映射到 Azure 资源管理器属性。

#### <a name="cause"></a>原因

策略定义中使用的别名不正确或不存在。

#### <a name="resolution"></a>解决方法

首先，验证资源管理器属性是否有别名。 若要查找可用别名，请转到[适用于 Visual Studio Code 的 Azure Policy 扩展](../how-to/extension-for-vscode.md)或 SDK。
如果资源管理器属性没有别名，请创建支持票证。

### <a name="scenario-evaluation-details-arent-up-to-date"></a>方案：评估详细信息不是最新的

#### <a name="issue"></a>问题

资源处于“未启动”状态或符合性详细信息不是最新的。

#### <a name="cause"></a>原因

应用新策略或计划分配大约需要 30 分钟。 现有分配范围内的新资源或更新的资源大约只需 15 分钟即可使用。 标准符合性扫描每 24 小时进行一次。 有关详细信息，请参阅[评估触发器](../how-to/get-compliance-data.md#evaluation-triggers)。

#### <a name="resolution"></a>解决方法

首先，请等待一段时间来完成评估以及等待 Azure 门户或 SDK 中显示符合性结果。 若要使用 Azure PowerShell 或 REST API 开始新的评估扫描，请参阅[按需评估扫描](../how-to/get-compliance-data.md#on-demand-evaluation-scan)。

### <a name="scenario-compliance-isnt-as-expected"></a>方案：符合性与预期不符

#### <a name="issue"></a>问题

资源未处于预期有效的评估状态（符合或不符合） 。

#### <a name="cause"></a>原因

资源不在正确的策略分配范围内，或者策略定义未按预期执行。

#### <a name="resolution"></a>解决方法

若要解决策略定义问题，请执行以下操作：

1. 首先，请等待一段时间来完成评估以及等待 Azure 门户或 SDK 中显示符合性结果。 

1. 若要使用 Azure PowerShell 或 REST API 开始新的评估扫描，请参阅[按需评估扫描](../how-to/get-compliance-data.md#on-demand-evaluation-scan)。
1. 确保分配参数和分配范围已正确设置。
1. 检查[策略定义模式](../concepts/definition-structure.md#mode)：
    - 此模式应为 `all`，适用于所有资源类型。
    - 如果策略定义检查标记或位置，则此模式应为 `indexed`。
1. 确保资源的范围未被[排除](../concepts/assignment-structure.md#excluded-scopes)或[豁免](../concepts/exemption-structure.md)。
1. 如果策略分配的符合性显示 `0/0` 资源，表示没有确定在分配范围内适用的资源。 检查策略定义和分配范围。
1. 对于应合规但实际不合规的资源，请参阅[确定不合规的原因](../how-to/determine-non-compliance.md)。 通过将定义与计算的属性值进行比较，可了解资源不合规的原因。
    - 如果“目标值”错误，请修改策略定义。
    - 如果“当前值”错误，请通过 `resources.azure.com` 验证资源有效负载。
1. 有关其他常见问题和解决方案，请参阅[故障排查：强制实施与预期不符](#scenario-enforcement-not-as-expected)。

如果复制的和自定义的内置策略定义或自定义定义仍存在问题，请在“创作策略”下创建支持票证，以正确提交问题。

### <a name="scenario-enforcement-not-as-expected"></a>方案：未按预期执行

#### <a name="issue"></a>问题

预期由 Azure Policy 处理的资源未被处理，并且 [Azure 活动日志](../../../azure-monitor/platform/platform-logs-overview.md)中没有条目。

#### <a name="cause"></a>原因

已为 [enforcementMode](../concepts/assignment-structure.md#enforcement-mode)“禁用”设置配置了策略分配。
当 enforcementMode 处于禁用状态时，不会强制实施策略效果，并且活动日志中没有条目。

#### <a name="resolution"></a>解决方法

通过执行以下操作来排查策略分配的实施问题：

1. 首先，请等待一段时间来完成评估以及等待 Azure 门户或 SDK 中显示符合性结果。 

1. 若要使用 Azure PowerShell 或 REST API 开始新的评估扫描，请参阅[按需评估扫描](../how-to/get-compliance-data.md#on-demand-evaluation-scan)。
1. 确保分配参数和分配范围已正确设置，以及“enforcementMode”是否为“已启用”。
1. 检查[策略定义模式](../concepts/definition-structure.md#mode)：
    - 此模式应为 `all`，适用于所有资源类型。
    - 如果策略定义检查标记或位置，则此模式应为 `indexed`。
1. 确保资源的范围未被[排除](../concepts/assignment-structure.md#excluded-scopes)或[豁免](../concepts/exemption-structure.md)。
1. 验证资源有效负载是否与策略逻辑匹配。 可以通过[捕获 HTTP 存档 (HAR) 跟踪](../../../azure-portal/capture-browser-trace.md)或查看 Azure 资源管理器模板（ARM 模板）属性来完成此操作。
1. 有关其他常见问题和解决方案，请参阅[故障排查：符合性与预期不符](#scenario-compliance-isnt-as-expected)。

如果复制的和自定义的内置策略定义或自定义定义仍存在问题，请在“创作策略”下创建支持票证，以正确提交问题。

### <a name="scenario-denied-by-azure-policy"></a>方案：被 Azure Policy 拒绝

#### <a name="issue"></a>问题

拒绝创建或更新资源。

#### <a name="cause"></a>原因

向新资源或更新的资源的范围执行的策略分配符合设有[拒绝](../concepts/effects.md#deny)效果的策略定义的条件。 符合这些定义的资源将无法创建或更新。

#### <a name="resolution"></a>解决方法

拒绝策略分配中的错误消息包括策略定义和策略分配 ID。 如果消息中的错误信息丢失，还可在[活动日志](../../../azure-monitor/platform/activity-log.md#view-the-activity-log)中找到。 使用此信息可获取更多详细信息，以了解资源限制和调整请求中的资源属性以使其匹配允许的值。

## <a name="template-errors"></a>模板错误

### <a name="scenario-policy-supported-functions-processed-by-template"></a>方案：策略支持的、由模板处理的函数

#### <a name="issue"></a>问题

Azure Policy 支持大量 ARM 模板函数以及仅在策略定义中可用的函数。 资源管理器将这些函数作为部署的一部分而不是作为策略定义的一部分进行处理。

#### <a name="cause"></a>原因

如果使用受支持的函数（如 `parameter()` 或 `resourceGroup()`），可在部署时生成函数的处理结果，而不是允许策略定义和 Azure Policy 引擎处理函数。

#### <a name="resolution"></a>解决方法

若要将函数作为策略定义的一部分进行传递，请使用 `[` 转义整个字符串，以便使属性看起来像是 `[[resourceGroup().tags.myTag]`。 转义字符会导致资源管理器在处理模板时将值视为字符串。 然后，Azure Policy 将函数放置在策略定义中，使其能够按预期的动态方式执行。 有关详细信息，请参阅 [Azure 资源管理器模板中的语法和表达式](../../../azure-resource-manager/templates/template-expressions.md)。

## <a name="add-on-for-kubernetes-installation-errors"></a>Kubernetes 加载项安装错误

### <a name="scenario-installation-by-using-a-helm-chart-fails-because-of-a-password-error"></a>方案：由于密码错误而导致使用 Helm 图表安装失败

#### <a name="issue"></a>问题

`helm install azure-policy-addon` 命令失败，并返回以下错误之一：

- `!: event not found`
- `Error: failed parsing --set data: key "<key>" has no value (cannot end with ,)`

#### <a name="cause"></a>原因

生成的密码包含 Helm 图表要用于拆分的逗号 (`,`)。

#### <a name="resolution"></a>解决方法

运行 `helm install azure-policy-addon` 时，使用反斜杠 (`\`) 转义密码值中的逗号 (`,`)。

### <a name="scenario-installation-by-using-a-helm-chart-fails-because-the-name-already-exists"></a>方案：由于名称已存在而导致使用 Helm 图表安装失败

#### <a name="issue"></a>问题

`helm install azure-policy-addon` 命令失败，并返回以下错误：

- `Error: cannot re-use a name that is still in use`

#### <a name="cause"></a>原因

已安装或部分安装带有名称 `azure-policy-addon` 的 Helm 图表。

#### <a name="resolution"></a>解决方法

删除 Kubernetes 加载项的 Azure Policy，然后重新运行 `helm install azure-policy-addon` 命令。


### <a name="scenario-the-resource-provider-isnt-registered"></a>方案：未注册资源提供程序

#### <a name="issue"></a>问题

加载项可以访问 Azure Policy 服务终结点，但加载项日志会显示以下错误之一：

- `The resource provider 'Microsoft.PolicyInsights' is not registered in subscription '{subId}'. See
  https://aka.ms/policy-register-subscription for how to register subscriptions.`

- `policyinsightsdataplane.BaseClient#CheckDataPolicyCompliance: Failure responding to request:
  StatusCode=500 -- Original Error: autorest/azure: Service returned an error. Status=500
  Code="InternalServerError" Message="Encountered an internal server error.`

#### <a name="cause"></a>原因

未注册“Microsoft.PolicyInsights”资源提供程序。 必须为加载项注册该资源提供程序，才能获取策略定义并返回符合性数据。

#### <a name="resolution"></a>解决方法

在群集订阅中注册“Microsoft.PolicyInsights”资源提供程序。 有关说明，请参阅[注册资源提供程序](../../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider)。

### <a name="scenario-the-subscription-is-disabled"></a>方案：订阅已禁用

#### <a name="issue"></a>问题

加载项可以访问 Azure Policy 服务终结点，但会显示以下错误：

`The subscription '{subId}' has been disabled for azure data-plane policy. Please contact support.`

#### <a name="cause"></a>原因

此错误表示订阅明确存在问题，并且添加了功能标志 `Microsoft.PolicyInsights/DataPlaneBlocked` 来阻止订阅。

#### <a name="resolution"></a>解决方法

若要调查并解决此问题，请[联系功能团队](mailto:azuredg@microsoft.com)。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出或者无法解决，请访问以下任一通道以获取支持：

- 通过 [Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-policy.html) 获得专家提供的答案。


- 如果仍需帮助，请转到 [Azure 支持网站](https://support.azure.cn/support/support-azure/)并提交请求。

<!--Update_Description: update meta properties, wording update, update link-->