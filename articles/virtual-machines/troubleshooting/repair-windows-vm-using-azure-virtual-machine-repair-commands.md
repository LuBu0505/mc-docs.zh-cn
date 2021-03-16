---
title: 使用 Azure 虚拟机修复命令修复 Windows VM | Azure
description: 本文详细介绍如何使用 Azure VM 修复命令将磁盘连接到另一个 Windows VM 来修复所有错误，然后重新生成原始 VM。
services: virtual-machines-windows
manager: dcscontentpm
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
origin.date: 09/10/2019
author: rockboyfor
ms.date: 02/22/2021
ms.testscope: yes
ms.testdate: 10/19/2020
ms.author: v-yeche
ms.openlocfilehash: 4653d0944fdfb474b274b5d81c488acec9527235
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052494"
---
# <a name="repair-a-windows-vm-by-using-the-azure-virtual-machine-repair-commands"></a>使用 Azure 虚拟机修复命令修复 Windows VM

如果 Windows 虚拟机 (VM) 在 Azure 中遇到启动或磁盘错误，可能需要对磁盘本身执行缓解操作。 一个常见示例是应用程序更新失败，使 VM 无法成功启动。 本文详细介绍如何使用 Azure VM 修复命令将磁盘连接到另一个 Windows VM 来修复所有错误，然后重新生成原始 VM。

> [!IMPORTANT]
> * 本文中的脚本仅适用于使用 [Azure 资源管理器](../../azure-resource-manager/management/overview.md)的 VM。
> * 运行脚本需要 VM 的出站连接（端口 443）。
> * 一次只能运行一个脚本。
> * 无法取消正在运行的脚本。
> * 脚本最多可以运行 90 分钟，之后它将超时。
> * 对于使用 Azure 磁盘加密的 VM，仅支持通过单次传递加密（带有或不带 KEK）加密的托管磁盘。

## <a name="repair-process-overview"></a>修复过程概述

现在可以使用 Azure VM 修复命令更改 VM 的 OS 磁盘，而不再需要删除并重新创建 VM。

请按照下列步骤排查 VM 问题：

1. 启动 Azure 本地 Shell
2. 运行 az extension add/update。
3. 运行 az vm repair create。
4. 运行 az vm repair run 或执行缓解步骤。
5. 运行 az vm repair restore。

有关其他文档和说明，请参阅 [az vm repair](https://docs.azure.cn/cli/ext/vm-repair/vm/repair#az_vm_repair)。

## <a name="repair-process-example"></a>修复过程示例

1. 启动 Azure 本地 Shell

    <!--Not Available on The Azure Cloud Shell-->
    <!--Not Available on select **Try it** from the upper-right corner of a code block.-->
    <!--Not Available on Select **Copy** to copy the blocks of code, then paste the code into the local Shell, and select **Enter** to run it.-->

    如果希望在本地安装并使用 CLI，则本快速入门需要 Azure CLI 2.0.30 版或更高版本。 运行 ``az --version`` 即可查找版本。 如果需要安装或升级 Azure CLI，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli)。

    如果需要使用与当前登录 Azure 门户所用帐户不同的帐户登录本地 Shell，可使用 ``az login`` [az login reference](https://docs.azure.cn/cli/reference-index#az_login)。 若要在与你的帐户关联的订阅之间切换，可使用 ``az account set --subscription`` [az account set reference](https://docs.azure.cn/cli/account#az_account_set)。

    <--Mooncake 自定义句子逻辑-->

2. 如果是首次使用 `az vm repair` 命令，请添加 vm-repair CLI 扩展。

    ```azurecli
    az extension add -n vm-repair
    ```

    如果以前使用过 `az vm repair` 命令，请将任何更新应用于 vm-repair 扩展。

    ```azurecli
    az extension update -n vm-repair
    ```

3. 运行 `az vm repair create`。 此命令将为非功能性 VM 创建 OS 磁盘的副本，在新资源组中创建修复 VM，并附加 OS 磁盘副本。  修复 VM 的大小和区域将与指定的非功能性 VM 相同。 所有步骤中使用的资源组和 VM 名称都将用于非功能性 VM。 如果 VM 使用的是 Azure 磁盘加密，该命令将尝试解锁已加密的磁盘，以便在附加到修复 VM 时可对其进行访问。

    ```azurecli
    az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password 'password!234' --verbose
    ```

4. 运行 `az vm repair run`。 此命令将通过修复 VM 在附加的磁盘上运行指定的修复脚本。 如果你使用的故障排除指南指定了 run-id，请在此处使用它，否则可以使用 `az vm repair list-scripts` 来查看可用的修复脚本。 此处使用的资源组和 VM 名称适用于第 3 步中使用的非功能性 VM。 有关修复脚本的其他信息可在[修复脚本库](https://github.com/Azure/repair-script-library)中找到。

    ```azurecli
    az vm repair run -g MyResourceGroup -n MyVM --run-on-repair --run-id win-hello-world --verbose
    ```

    （可选）可以使用修复 VM 来执行任何所需的手动缓解步骤，然后继续执行步骤 5。

5. 运行 `az vm repair restore`。 此命令会将已修复的 OS 磁盘与 VM 的原始 OS 磁盘交换。 此处使用的资源组和 VM 名称适用于第 3 步中使用的非功能性 VM。

    ```azurecli
    az vm repair restore -g MyResourceGroup -n MyVM --verbose
    ```

## <a name="verify-and-enable-boot-diagnostics"></a>验证和启用启动诊断

以下示例在名为 ``myResourceGroup`` 的资源组中名为 ``myVMDeployed`` 的 VM 上启用诊断扩展：

Azure CLI

```azurecli
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.chinacloudapi.cn/
```

## <a name="next-steps"></a>后续步骤

* 如果在连接到 VM 时遇到问题，请参阅[对 Azure VM 的 RDP 连接进行故障排除](./troubleshoot-rdp-connection.md)。
* 如果在访问 VM 上运行的应用程序时遇到问题，请参阅[排查 Azure 中虚拟机上的应用程序连接问题](./troubleshoot-app-connection.md)。
* 有关资源组的详细信息，请参阅 [Azure 资源管理器概述](../../azure-resource-manager/management/overview.md)。

<!--Update_Description: update meta properties, wording update, update link-->