---
title: 创建 Azure 自动化运行方式帐户
description: 本文介绍如何使用 PowerShell 或从 Azure 门户创建运行方式帐户。
services: automation
ms.subservice: process-automation
origin.date: 01/06/2021
ms.date: 02/22/2021
ms.topic: conceptual
ms.openlocfilehash: c235e4c333c4c455582f20a54df0bcd571f8bb85
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751865"
---
# <a name="how-to-create-an-azure-automation-run-as-account"></a>如何创建 Azure 自动化运行方式帐户

Azure 自动化中的运行方式帐户提供身份验证，以使用自动化 runbook 和其他自动化功能管理 Azure 资源管理器或 Azure 经典部署模型上的资源。 本文介绍如何从 Azure 门户或 Azure PowerShell 创建运行方式帐户或经典运行方式帐户。

## <a name="create-account-in-azure-portal"></a>在 Azure 门户中创建帐户

请执行以下步骤，在 Azure 门户中更新 Azure 自动化帐户。 运行方式帐户和经典运行方式帐户是单独创建的。 如果不需管理经典资源，可以只创建 Azure 运行方式帐户。

1. 以订阅管理员角色成员和订阅共同管理员的帐户登录 Azure 门户。

2. 搜索并选择“自动化帐户”。

3. 在“自动化帐户”页上，从列表中选择你的自动化帐户。

4. 在左侧窗格中的“帐户设置”部分选择“运行方式帐户” 。

    :::image type="content" source="media/create-run-as-account/automation-account-properties-pane.png" alt-text="选择“运行方式帐户”选项。":::

5. 根据所需帐户，使用“+ Azure 运行方式帐户”或“+ Azure 经典运行方式帐户”窗格 。 查看概述信息后，单击“创建”。

    :::image type="content" source="media/create-run-as-account/automation-account-create-run-as.png" alt-text="选择创建运行方式帐户的选项":::

6. 在 Azure 创建运行方式帐户时，可以在菜单的“通知”下面跟踪进度。 此外还显示一个横幅，指出正在创建帐户。 此过程可能需要几分钟才能完成。

## <a name="create-account-using-powershell"></a>使用 PowerShell 创建帐户

以下列表提供了使用提供的脚本在 PowerShell 中创建运行方式帐户所要满足的要求。 这些要求适用于这两种类型的运行方式帐户。

* 装有 Azure 资源管理器模块 3.4.1 和更高版本的 Windows 10 或 Windows Server 2016。 PowerShell 脚本不支持早期版本的 Windows。
* Azure PowerShell 6.2.4 或更高版本。 有关信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。
* 一个自动化帐户，将其作为 `AutomationAccountName` 和 `ApplicationDisplayName` 参数的值引用。
* 与[配置运行方式帐户时所需的权限](automation-security-overview.md#permissions)中所列权限相当的权限。

若要获取 PowerShell 脚本的必需参数 `AutomationAccountName`、`SubscriptionId` 和 `ResourceGroupName` 的值，请完成以下步骤。

1. 登录到 Azure 门户。

1. 搜索并选择“自动化帐户”。

1. 在“自动化帐户”页上，从列表中选择你的自动化帐户。

1. 在左窗格中选择“属性”。

1. 记下“属性”页上的“名称”、“订阅 ID”和“资源组”的值   。

   ![自动化帐户属性页](media/create-run-as-account/automation-account-properties.png)

## <a name="next-steps"></a>后续步骤

* 若要详细了解图形创作，请参阅[在 Azure 自动化中创作图形 Runbook](automation-graphical-authoring-intro.md)。
* 若要开始使用 PowerShell Runbook，请参阅[教程：创建 PowerShell Runbook](learn/automation-tutorial-runbook-textual-powershell.md)。
* 若要开始使用 Python 3 runbook，请参阅[教程：创建 Python 3 runbook](learn/automation-tutorial-runbook-textual-python-3.md)。