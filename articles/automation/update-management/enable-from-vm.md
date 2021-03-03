---
title: 从 Azure VM 启用 Azure 自动化更新管理
description: 本文介绍如何从 Azure VM 启用更新管理。
services: automation
origin.date: 11/04/2020
ms.date: 02/22/2021
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: e79be36f0a2f62fc95c518f05c3ecc44121a7fa7
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697278"
---
# <a name="enable-update-management-from-an-azure-vm"></a>从 Azure VM 启用“更新管理”

本文介绍如何在一个或多个 Azure 虚拟机 (VM) 上启用[更新管理](overview.md)功能。 要大规模启用 Azure VM，必须使用更新管理启用现有 Azure VM。

> [!NOTE]
> 在启用更新管理时，只有某些区域支持链接 Log Analytics 工作区和自动化帐户。 有关支持的映射对的列表，请参阅[自动化帐户和 Log Analytics 工作区的区域映射](../how-to/region-mappings.md)。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果还没有订阅，请注册一个[试用订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* 用于管理计算机的[自动化帐户](../automation-security-overview.md)。
* [虚拟机](../../virtual-machines/windows/quick-create-portal.md)。

## <a name="sign-in-to-azure"></a>登录 Azure

登录 [Azure 门户](https://portal.azure.cn)。

## <a name="enable-the-feature-for-deployment"></a>启用相关功能来进行部署

1. 在 [Azure 门户](https://portal.azure.cn)中，选择“虚拟机”，或在“主页”页上搜索并选择“虚拟机” 。

2. 选择要启用更新管理的 VM。 VM 可以位于任何区域，无论自动化帐户的位置如何。 你

3. 在 VM 页的“操作”下选择“更新管理”。

4. 必须拥有 `Microsoft.OperationalInsights/workspaces/read` 权限才能确定是否为工作区启用了 VM。 若要了解所需的其他权限，请参阅[启用计算机所需的权限](../automation-role-based-access-control.md#feature-setup-permissions)。 若要了解如何一次启用多台计算机，请参阅[从自动化帐户启用更新管理](./enable-from-automation-account.md)。

5. 在“启用更新管理”页上，选择 Log Analytics 工作区和自动化帐户，然后单击“启用”以启用更新管理。 启用更新管理后，可能需要大约 15 分钟才能查看 VM 的更新评估。

    ![启用更新管理](media/enable-from-vm/enable-update-management.png)

## <a name="next-steps"></a>后续步骤

* 若要将更新管理用于 VM，请参阅[管理 Azure VM 的更新和修补程序](manage-updates-for-vm.md)。

* 若要对常规更新管理错误进行故障排除，请参阅[排查更新管理问题](../troubleshoot/update-management.md)。
