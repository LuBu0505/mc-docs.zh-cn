---
title: 在 Azure Windows VM 中运行脚本
description: 本主题介绍如何在 Windows 虚拟机中运行脚本
services: automation
ms.service: virtual-machines
origin.date: 05/02/2018
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 08/31/2020
ms.author: v-yeche
ms.topic: how-to
manager: carmonm
ms.openlocfilehash: b5e317a9efc69967ae6cab3897c27136fde3c28b
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055247"
---
# <a name="run-scripts-in-your-windows-vm"></a>在 Windows VM 中运行脚本

可能需要在 VM 中运行命令来自动完成任务或排查问题。 下文概述了可以用来在 VM 中运行脚本和命令的功能。

## <a name="custom-script-extension"></a>自定义脚本扩展

[自定义脚本扩展](../extensions/custom-script-windows.md)主要用于部署后配置和软件安装。

* 在 Azure 虚拟机中下载并运行脚本。
* 可以使用 Azure 资源管理器模板、Azure CLI、REST API、PowerShell 或 Azure 门户来运行。
* 脚本文件可以从 Azure 存储或 GitHub 下载，也可以从电脑获取（通过 Azure 门户运行时）。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于部署后配置、软件安装以及其他配置或管理任务。

<!--Verify successfully-->

## <a name="run-command"></a>运行命令

[运行命令](run-command.md)功能支持使用脚本进行虚拟机、应用程序管理和故障排除，并且即使在计算机不可访问时也可用，例如，如果来宾防火墙没有打开 RDP 或 SSH 端口。

* 在 Azure 虚拟机中运行脚本。
* 可以使用 [Azure 门户](run-command.md)、[REST API](https://docs.microsoft.com/rest/api/compute/virtual%20machines%20run%20commands/runcommand)、[Azure CLI](https://docs.azure.cn/cli/vm/run-command#az_vm_run_command_invoke) 或 [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) 运行
* 在 Azure 门户中快速运行脚本并查看输出，并根据需要重复此过程。
* 可以直接键入脚本，也可以运行其中一个内置脚本。
* 在 Windows 计算机中运行 PowerShell 脚本，在 Linux 计算机中运行 Bash 脚本。
* 适用于虚拟机和应用程序管理以及在无法访问的虚拟机中运行脚本。

## <a name="hybrid-runbook-worker"></a>混合 Runbook 辅助角色

[混合 Runbook 辅助角色](../../automation/automation-hybrid-runbook-worker.md)使用存储在自动化帐户中的用户自定义脚本提供常规计算机、应用程序和环境管理。

* 在 Azure 和非 Azure 计算机中运行脚本。
* 可以使用 Azure 门户、Azure CLI、REST API、PowerShell、Webhook 运行。
* 在自动化帐户中存储和管理的脚本。
* 运行 PowerShell、PowerShell 工作流、Python 或图形 runbook
* 脚本运行时间没有时间限制。
* 可以同时运行多个脚本。
* 返回并存储完整的脚本输出。
* 作业历史记录在 90 天内可用。
* 脚本可以作为本地系统运行，也可以使用用户提供的凭据运行。
* 需要[手动安装](../../automation/automation-windows-hrw-install.md)

<!-- Not Available on ## Serial console-->
## <a name="next-steps"></a>后续步骤

详细了解可以用来在 VM 中运行脚本和命令的不同功能。

* [自定义脚本扩展](../extensions/custom-script-windows.md)
* [运行命令](run-command.md)
* [混合 Runbook 辅助角色](../../automation/automation-hybrid-runbook-worker.md)

<!--NOT AVAILABLE ON * [Serial console](../troubleshooting/serial-console-windows.md)-->
<!--Update_Description: update meta properties, wording update, update link-->