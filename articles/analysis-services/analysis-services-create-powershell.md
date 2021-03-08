---
title: 快速入门 - 使用 PowerShell Azure Analysis Services 创建 Azure Analysis Services | Azure
description: 本快速入门介绍如何使用 PowerShell 创建 Azure Analysis Services 服务器
ms.service: azure-analysis-services
ms.topic: quickstart
origin.date: 08/31/2020
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.reviewer: minewiskan
ms.custom: references_regions , devx-track-azurepowershell
ms.openlocfilehash: 1f67883150af2cef7c41a6429f72aa384ac1603d
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054299"
---
# <a name="quickstart-create-a-server---powershell"></a>快速入门：创建服务器 - PowerShell

本快速入门介绍如何从命令行使用 PowerShell，以便在 Azure 订阅中创建 Azure Analysis Services 服务器。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure 订阅**：访问 [Azure 试用版](https://www.microsoft.com/china/azure/index.html?fromtype=cn)来创建一个帐户。
- **Azure Active Directory**：订阅必须与 Azure Active Directory 租户相关联，且该目录中必须有一个帐户。 若要了解详细信息，请参阅[身份验证和用户权限](analysis-services-manage-users.md)。
- **Azure PowerShell**。 要查找已安装的版本，请运行 `Get-Module -ListAvailable Az`。 若要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-Az-ps)。

## <a name="import-azanalysisservices-module"></a>导入 Az.AnalysisServices 模块

若要在订阅中创建一台服务器，请使用 [Az.AnalysisServices](https://docs.microsoft.com/powershell/module/az.analysisservices) 模块。 将 Az.AnalysisServices 模块加载到 PowerShell 会话中。

```powershell
Import-Module Az.AnalysisServices
```

## <a name="sign-in-to-azure"></a>登录 Azure

使用 [Connect-AzAccount -Environment AzureChinaCloud](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) 命令登录到 Azure 订阅。 按屏幕说明操作。

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

## <a name="create-a-resource-group"></a>创建资源组

[Azure 资源组](../azure-resource-manager/management/overview.md)是在其中以组的形式部署和管理 Azure 资源的逻辑容器。 创建服务器时，必须在订阅中指定资源组。 如果还没有资源组，可以使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令新建一个。 以下示例在中国北部区域中创建名为 `myResourceGroup` 的资源组。

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "ChinaNorth"
```

## <a name="create-a-server"></a>创建服务器

<!--MOONCAKE: CUSTOMIZATION-->
<!--MOONCAKE: REMOVE D1 (free) tier-->
<!--Notice: ChinaNorth is valid and -Sku should be B0,B1,S0-S4-->

使用 [New-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) 命令创建新的服务器。 以下示例在 ChinaNorth 区域的 myResourceGroup 中的 B0 层创建名为 myServer 的服务器，并指定 philipc@adventureworks.com 为服务器管理员。

```powershell
New-AzAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location ChinaNorth -Sku B0 -Administrator "philipc@adventure-works.com"
```

<!--Notice: ServerName should be lower charactor-->
<!--Notice: -Sku should be B0,B1,S0-S4-->
<!--MOONCAKE: CUSTOMIZATION-->

## <a name="clean-up-resources"></a>清理资源

可以使用 [Remove-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) 命令从订阅中删除服务器。 若要继续完成本系列的其他快速入门和教程，请勿删除服务器。 以下示例删除在上一步创建的服务器。

```powershell
Remove-AzAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何使用 PowerShell 在 Azure 订阅中创建服务器。 创建服务器后，可以通过配置（可选）的服务器防火墙对其进行保护。 还可以直接从门户将基本示例数据模型添加到服务器。 拥有一个示例模型有助于了解如何配置模型数据库角色和测试客户端连接。 若要了解更多信息，请继续学习有关添加示例模型的教程。

> [!div class="nextstepaction"]
> [快速入门：配置服务器防火墙 - 门户](analysis-services-qs-firewall.md)

<!-- Update_Description: update meta properties, wording update, update link -->