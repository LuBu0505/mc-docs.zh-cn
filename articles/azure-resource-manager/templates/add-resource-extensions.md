---
title: 使用扩展提供部署后配置
description: 了解如何使用 Azure 资源管理器模板扩展提供部署后配置。
ms.topic: conceptual
origin.date: 12/14/2018
author: rockboyfor
ms.date: 01/25/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: c24aeda966fc2dd18614fb6ee3660c206b5e96bb
ms.sourcegitcommit: 102a21dc30622e4827cc005bdf71ade772c1b8de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98751383"
---
<!--Verify successfully-->
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>使用扩展提供部署后配置

Azure 资源管理器模板（ARM 模板）扩展是小型应用程序，用于在 Azure 资源上提供部署后配置和自动化任务。 最受欢迎的扩展是虚拟机扩展。 请参阅[适用于 Windows 的虚拟机扩展和功能](../../virtual-machines/extensions/features-windows.md)或[适用于 Linux 的虚拟机扩展和功能](../../virtual-machines/extensions/features-linux.md)。

## <a name="extensions"></a>扩展

现有的扩展如下：

- `Microsoft.Compute/virtualMachines/extensions`
- `Microsoft.Compute virtualMachineScaleSets/extensions`
- `Microsoft.HDInsight clusters/extensions`
- `Microsoft.Sql servers/databases/extensions`
- `Microsoft.Web/sites/siteextensions`

<!--Not Available on [template reference](https://docs.microsoft.com/azure/templates/)-->

若要了解如何使用这些扩展，请参阅：

- [教程：使用 ARM 模板部署虚拟机扩展](template-tutorial-deploy-vm-extensions.md)。
- [教程：使用 ARM 模板导入 SQL BACPAC 文件](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [教程：使用 ARM 模板部署虚拟机扩展](template-tutorial-deploy-vm-extensions.md)

<!-- Update_Description: update meta properties, wording update, update link -->
