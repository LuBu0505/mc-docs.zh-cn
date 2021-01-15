---
title: 诊断日志收集
description: 了解诊断日志收集。
author: WenJason
ms.topic: article
origin.date: 10/30/2020
ms.date: 01/11/2021
ms.author: v-jay
ms.reviewer: shisab
ms.lastreviewed: 12/08/2020
ms.openlocfilehash: 2e7801cb82ab51b2efd06dc21968f0385d808e96
ms.sourcegitcommit: 3f54ab515b784c9973eb00a5c9b4afbf28a930a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97894382"
---
# <a name="diagnostic-log-collection"></a>诊断日志收集

Azure Stack Hub 是一个集合，同时包含可以彼此交互的 Windows 组件和本地 Azure 服务。 所有这些组件和服务都会生成自己的日志集。 由于 Azure 支持使用这些日志来识别和修复问题，因此我们提供诊断日志收集。 使用诊断日志收集功能，你可以快速收集诊断日志并与 Azure 支持共享该日志。

> [!IMPORTANT]
> 必须注册 Azure Stack Hub 才能使用诊断日志收集。 如果尚未注册 Azure Stack Hub，请使用[特权终结点 (PEP)](azure-stack-get-azurestacklog.md) 来共享日志。 

Azure Stack Hub 提供了多种方式来收集和保存诊断日志并将其发送到 Azure 支持。 根据与 Azure 的连接情况，用于收集和发送日志的选项包括：
* [主动发送日志（建议）](#send-logs-proactively)
* [立即发送日志](#send-logs-now)
* [在本地保存日志](#save-logs-locally)

以下流程图显示了各种情况下用于发送诊断日志的选项。 如果 Azure Stack Hub 可以连接到 Azure，建议启用“主动日志收集”，这会在引发关键警报时自动将诊断日志上传到 Azure 中由 Azure 控制的存储 blob。 还可使用“立即发送日志”按需收集日志。 如果 Azure Stack Hub 与 Azure 断开连接，可以“在本地保存日志”。 

![流程图，显示如何将日志立即发送到 Microsoft](media/azure-stack-help-and-support/send-logs-now-flowchart.png)

## <a name="send-logs-proactively"></a>主动发送日志

在你建立支持案例之前，主动收集日志功能会自动从 Azure Stack Hub 收集诊断日志并将其发送给我们。 这些日志仅在发出了[系统运行状况警报](#proactive-diagnostic-log-collection-alerts)的情况下收集，并且仅在建立了支持案例的情况下供 Azure 支持访问。

从 Azure Stack Hub 版本 2008 开始，主动收集日志的功能使用改进的算法，即使在操作员看不到的错误情况下也可以捕获日志。 这样可以确保在适当的时间收集正确的诊断信息，无需操作员进行任何交互。 在某些情况下，Azure 支持可以更快地开始故障排除工作并解决问题。 初始算法改进侧重于修补升级操作。 建议启用主动收集日志的功能，因为更多的操作已经过优化，好处更多。

随时可以禁用和重新启用主动日志收集。 按照以下步骤设置主动收集日志功能。

1. 登录到 Azure Stack Hub 管理员门户。
1. 打开“帮助 + 支持概述”。
1. 如果出现横幅，请选择“启用主动收集日志功能”。 也可以选择“设置”，将“主动收集日志”设置为“启用”，然后选择“保存”   。

> [!NOTE]
> 如果为本地文件共享配置了日志位置设置，请确保生命周期管理策略会阻止共享存储达到其容量配额。 Azure Stack Hub 不会监视本地文件共享或强制执行任何保留策略。   

### <a name="how-the-data-is-handled"></a>数据处理方式

你同意我们可以仅根据 Azure Stack Hub 系统运行状况警报定期自动收集日志。 你还确认并同意，这些日志可以上传并保留在由 Azure 管理和控制的 Azure 存储帐户中。

这些数据只用于排查系统运行状况警报问题，在未经你同意的情况下不会用于市场营销、广告或任何其他商业目的。 这些数据最多可保留 90 天，由我们收集的任何数据都会按照我们的[标准隐私做法](https://www.trustcenter.cn/zh-cn/privacy/)进行处理。

如果你不再同意，则以前在你同意的情况下收集的任何数据都不会受到影响。

通过“主动收集日志”方式收集的日志会上传到由 Azure 管理和控制的 Azure 存储帐户中。 在收到支持案例的情况下，或者是为了改善 Azure Stack Hub 的运行状况，我们可能会访问这些日志。

### <a name="proactive-diagnostic-log-collection-alerts"></a>主动收集诊断日志时的警报

如果已启用，则当引发以下事件之一时，主动收集日志的功能会上传日志。

例如，“更新失败”是一个警报，会触发主动收集诊断日志的操作。 如果启用了主动收集功能，则会在更新失败时主动捕获诊断日志，这样有助于 Azure 支持排查问题。 仅在引发 **更新失败** 的警报时才收集诊断日志。

| 警报标题 | FaultIdType |
|---|---|
|无法连接到远程服务 | UsageBridge.NetworkError|
|更新失败 | Urp.UpdateFailure |
|存储资源提供程序基础结构/依赖项不可用 |    StorageResourceProviderDependencyUnavailable |
|节点未连接到控制器| ServerHostNotConnectedToController |  
|路由发布失败 | SlbMuxRoutePublicationFailure |
|存储资源提供程序内部数据存储不可用 |    StorageResourceProvider。 DataStoreConnectionFail |
|存储设备发生故障 | Microsoft.Health.FaultType.VirtualDisks.Detached |
|运行状况控制器无法访问存储帐户 | Microsoft.Health.FaultType.StorageError |
|与物理磁盘的连接已丢失 | Microsoft.Health.FaultType.PhysicalDisk.LostCommunication |
|Blob 服务未在节点上运行 | StorageService.The.blob.service.is.not.running.on.a.node-Critical |
|基础结构角色不正常 | Microsoft.Health.FaultType.GenericExceptionFault |
|表服务错误 | StorageService.Table.service.errors-Critical |
|文件共享已利用超过 80% | Microsoft.Health.FaultType.FileShare.Capacity.Warning.Infra |
|缩放单元节点已脱机 | FRP.Heartbeat.PhysicalNode |
|基础结构角色实例不可用 | FRP.Heartbeat.InfraVM |
|基础结构角色实例不可用  | FRP.Heartbeat.NonHaVm |
|基础结构角色“目录管理”报告了时间同步错误 | DirectoryServiceTimeSynchronizationError |
|挂起的外部证书过期 | CertificateExpiration.ExternalCert.Warning |
|挂起的外部证书过期 | CertificateExpiration.ExternalCert.Critical |
|由于内存容量不足，无法针对特定类别和大小预配虚拟机 | AzureStack.ComputeController.VmCreationFailure.LowMemory |
|无法访问节点以供虚拟机放置 | AzureStack.ComputeController.HostUnresponsive |
|备份失败  | AzureStack.BackupController.BackupFailedGeneralFault |
|由于与失败的操作发生冲突，已跳过计划的备份    | AzureStack.BackupController.BackupSkippedWithFailedOperationFault |

## <a name="send-logs-now"></a>立即发送日志

> [!TIP]
> 通过使用[主动发送日志](#send-logs-proactively)而不是“立即发送日志”来节省时间。

“立即发送日志”是一个选项，通常用于在建立支持案例之前从 Azure Stack Hub 手动收集并上传诊断日志。

可以通过两种方式手动将诊断日志发送到 Azure 支持：
* [管理员门户（建议）](#send-logs-now-with-the-administrator-portal)
* [PowerShell](#send-logs-now-with-powershell)

如果 Azure Stack Hub 已连接到 Azure，建议你使用管理员门户，因为这是直接将日志发送给我们的最简单方法。 如果该门户不可用，则应改为使用 PowerShell 发送日志。

### <a name="send-logs-now-with-the-administrator-portal"></a>通过管理员门户立即发送日志

通过管理员门户立即发送日志：

1. 打开“帮助 + 支持”>“日志收集”>“立即发送日志”。 
1. 指定日志收集的开始时间和结束时间。 
1. 选择本地时区。
1. 选择“收集并上传”。

如果已断开与 Internet 的连接，或者只想在本地保存日志，请使用 [Get-AzureStackLog](azure-stack-get-azurestacklog.md) 方法发送日志。

### <a name="send-logs-now-with-powershell"></a>通过 PowerShell 立即发送日志

如果使用“立即发送日志”方法并想要使用 PowerShell 而不是管理员门户，则可使用 `Send-AzureStackDiagnosticLog` cmdlet 来收集和发送特定日志。

* 可以使用 **FromDate** 和 **ToDate** 参数来收集特定时间段的日志。 如果未指定这些参数，则默认收集过去四小时的日志。

* 使用 **FilterByNode** 参数按计算机名筛选日志。 例如：

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByNode azs-xrp01
  ```

* 使用 **FilterByLogType** 参数按类型筛选日志。 可以选择按文件、共享或 WindowsEvent 进行筛选。 例如：

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByLogType File
  ```

* 使用 FilterByResourceProvider 参数为增值资源提供程序 (RP) 发送诊断日志。 常规语法为：
 
  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvider <<value-add RP name>>
  ```
 
  为 IoT 中心发送诊断日志： 

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvider IotHub
  ```
 
  为事件中心发送诊断日志：

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvider eventhub
  ```
 
* 使用 FilterByRole 参数从 VirtualMachines 和 BareMetal 角色发送诊断日志：

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByRole VirtualMachines,BareMetal
  ```

* 从 VirtualMachines 和 BareMetal 角色发送诊断日志，通过日期筛选功能筛选出过去 8 小时的日志文件：

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

* 从 VirtualMachines 和 BareMetal 角色发送诊断日志，通过日期筛选功能筛选出 8 小时前到 2 小时前这个时间段的日志文件：

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

> [!NOTE]
> 如果已断开与 Internet 的连接，或者只想在本地保存日志，请使用 [Get-AzureStackLog](azure-stack-get-azurestacklog.md) 方法发送日志。 

### <a name="how-the-data-is-handled"></a>数据处理方式

启动从 Azure Stack Hub 收集诊断日志的操作即表明你确认并同意，这些日志可以上传并保留到 Azure 管理和控制的 Azure 存储帐户中。 Azure 支持人员可以通过支持案例立即访问这些日志，而不必与客户联系以收集日志。

## <a name="save-logs-locally"></a>在本地保存日志

当 Azure Stack Hub 与 Azure 断开连接时，可以将日志保存到本地服务器消息块 (SMB) 共享。 在“设置”边栏选项卡中，输入具有共享写入权限的路径、用户名和密码。 在支持案例期间，Azure 支持会提供详细步骤来说明如何传输这些本地日志。 如果管理员门户不可用，则可以使用 [Get-AzureStackLog](azure-stack-get-azurestacklog.md) 在本地保存日志。

![诊断日志收集选项的屏幕截图](media/azure-stack-help-and-support/save-logs-locally.png)

## <a name="bandwidth-considerations"></a>带宽注意事项

进行诊断日志收集时日志的平均大小各不相同，具体取决于它是主动运行还是手动运行。 “主动收集日志”选项对应的日志平均大小约为 2 GB。 “立即发送日志”选项对应的收集大小取决于需要收集多少小时。

下表列出了在以受限或计量方式连接到 Azure 时的环境注意事项。

| 网络连接 | 影响 |
|----|---|
| 低带宽/高延迟连接 | 完成日志上传的时间会延长。 |
| 共享连接 | 上传也可能影响共享网络连接的其他应用/用户。 |
| 计量连接 | ISP 可能会针对你额外使用网络的情况收取额外费用。 |

## <a name="view-log-collection"></a>查看日志集合

以往从 Azure Stack Hub 收集的日志显示在“帮助 + 支持”中的“日志收集”页上，其中提供了以下日期和时间 ：

- **收集的时间**：日志收集操作的开始时间。
- **状态**：“正在进行”或“已完成”。
- **日志开始**：要收集日志的时段的开始时间。
- **日志结束**：收集时段的结束时间。
- **类型**：是手动收集日志还是主动收集日志。

![“帮助 + 支持”中的日志集合](media/azure-stack-help-and-support/azure-stack-log-collection.png)

## <a name="see-also"></a>另请参阅

[Azure Stack Hub 日志和客户数据处理](./azure-stack-data-collection.md)
