---
title: 排查 Synapse Studio 连接问题
description: 使用 PowerShell 排查 Azure Synapse Studio 连接问题
author: WenJason
ms.service: synapse-analytics
ms.topic: conceptual
origin.date: 10/30/2020
ms.date: 03/22/2021
ms.author: v-jay
ms.reviewer: jrasnick
ms.openlocfilehash: 2a53aad92b06ef082e731558e81211bba75e3953
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766449"
---
# <a name="troubleshoot-synapse-studio-connectivity-with-powershell"></a>使用 PowerShell 排查 Synapse Studio 连接问题

Azure Synapse Studio 必须依靠一组 Web API 终结点才能正常工作。 本指南将帮助你在以下情况下确定连接问题的原因：
- 配置本地网络（例如公司防火墙后面的网络）以便访问 Azure Synapse Studio。
- 使用 Azure Synapse Studio 遇到连接问题。

## <a name="prerequisite"></a>先决条件

* 在 Windows 上安装了 PowerShell 5.0 或更高版本，或者
* 在 Windows 或 Linux 上安装了 PowerShell Core 6.0 或更高版本。

## <a name="troubleshooting-steps"></a>疑难解答步骤

右键单击下面的链接，然后选择“将目标另存为”：

- [Test-AzureSynapse.ps1](https://go.microsoft.com/fwlink/?linkid=2119734)

或者，可以直接打开链接，并保存打开的脚本文件。 请勿保存上述链接地址，因为它将来可能会更改。

在文件资源管理器中，右键单击下载的脚本文件，然后选择“使用 PowerShell 运行”。

![使用 PowerShell 运行下载的脚本文件](media/troubleshooting-synapse-studio-powershell/run-with-powershell.png)

出现提示时，输入当前遇到问题或要测试其连接问题的 Azure Synapse 工作区名称，然后按 Enter。

![输入工作区名称](media/troubleshooting-synapse-studio-powershell/enter-workspace-name.png)

此时将启动诊断会话。 等待检查完成。

![等待诊断完成](media/troubleshooting-synapse-studio-powershell/wait-for-diagnosis.png)

最后，将显示诊断摘要。 如果你的电脑无法连接到一个或多个终结点，它将在“摘要”部分显示一些建议。

![查看诊断摘要](media/troubleshooting-synapse-studio-powershell/diagnosis-summary.png)

此外，系统将在故障排除脚本所在的文件夹中生成此会话的诊断日志文件。 其位置显示在“常规提示”部分 (`D:\TestAzureSynapse_2020....log`)。 你可以根据需要将此文件发送给支持团队。

如果你是网络管理员并且正在优化 Azure Synapse Studio 的防火墙配置，则“摘要”部分上方显示的技术详细信息可能会有所帮助。

* 所有标记为“已通过”的测试项（请求）表示它们已经通过连接测试，而不管 HTTP 状态代码如何。
 对于失败的请求，原因显示为黄色，例如 `NamedResolutionFailure` 或 `ConnectFailure`。 这些原因可能会帮助你确定网络环境是否存在配置错误。


