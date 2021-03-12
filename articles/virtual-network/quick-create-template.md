---
title: 快速入门 - 使用资源管理器模板创建虚拟网络
titleSuffix: Azure Virtual Network
description: 了解如何使用资源管理器模板创建 Azure 虚拟网络。
services: virtual-network
ms.service: virtual-network
ms.topic: quickstart
origin.date: 06/23/2020
author: rockboyfor
ms.date: 02/22/2021
ms.testscope: yes
ms.testdate: 10/05/2020
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: ce778fbdd3804693ddbf93f04d1a4d3765879706
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102053783"
---
<!--Verified Successfully-->
# <a name="quickstart-create-a-virtual-network---resource-manager-template"></a>快速入门：创建虚拟网络 - 资源管理器模板

在本快速入门中，你将了解如何使用 Azure 资源管理器模板创建具有两个子网的虚拟网络。 虚拟网络是 Azure 中专用网络的基本构建块。 它能让 Azure 资源（例如 VM）互相安全通信以及与 Internet 通信。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

还可以使用 [Azure 门户](quick-create-portal.md)、[Azure PowerShell](quick-create-powershell.md) 或 [Azure CLI](quick-create-cli.md) 完成本快速入门。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，请在开始前创建一个[试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

## <a name="review-the-template"></a>查看模板

本快速入门中使用的模板来自 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vnet-two-subnets/azuredeploy.json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vnetName": {
      "type": "string",
      "defaultValue": "VNet1",
      "metadata": {
        "description": "VNet name"
      }
    },
    "vnetAddressPrefix": {
      "type": "string",
      "defaultValue": "10.0.0.0/16",
      "metadata": {
        "description": "Address prefix"
      }
    },
    "subnet1Prefix": {
      "type": "string",
      "defaultValue": "10.0.0.0/24",
      "metadata": {
        "description": "Subnet 1 Prefix"
      }
    },
    "subnet1Name": {
      "type": "string",
      "defaultValue": "Subnet1",
      "metadata": {
        "description": "Subnet 1 Name"
      }
    },
    "subnet2Prefix": {
      "type": "string",
      "defaultValue": "10.0.1.0/24",
      "metadata": {
        "description": "Subnet 2 Prefix"
      }
    },
    "subnet2Name": {
      "type": "string",
      "defaultValue": "Subnet2",
      "metadata": {
        "description": "Subnet 2 Name"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2020-05-01",
      "name": "[parameters('vnetName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('vnetAddressPrefix')]"
          ]
        }
      },
      "resources": [
        {
          "type": "subnets",
          "apiVersion": "2020-05-01",
          "location": "[parameters('location')]",
          "name": "[parameters('subnet1Name')]",
          "dependsOn": [
            "[parameters('vnetName')]"
          ],
          "properties": {
            "addressPrefix": "[parameters('subnet1Prefix')]"
          }
        },
        {
          "type": "subnets",
          "apiVersion": "2020-05-01",
          "location": "[parameters('location')]",
          "name": "[parameters('subnet2Name')]",
          "dependsOn": [
            "[parameters('vnetName')]",
            "[parameters('subnet1Name')]"
          ],
          "properties": {
            "addressPrefix": "[parameters('subnet2Prefix')]"
          }
        }
      ]
    }
  ]
}
```

<!--MOONCAKE CUSTOMIZATION ON THE END BRACKET-->

<!--Not Available on - [**Microsoft.Network/virtualNetworks**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)-->
<!--Not Available on - [**Microsoft.Network/virtualNetworks/subnets**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)-->

## <a name="deploy-the-template"></a>部署模板

将资源管理器模板部署到 Azure：

<!--MOONCAKE CUSTOMIZATION till 02/22/2021-->

1. 选择“部署到 Azure”，登录到 Azure 并打开模板。 该模板创建包含两个子网的虚拟网络。

   [:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="部署到 Azure":::](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-virtual-network-2vms-create%2Fazuredeploy.json)

1. 在“自定义部署”页上，选择“在编辑器中生成自己的模板”，然后在“编辑模板”页上保存上述模板内容。 该模板创建包含两个子网的虚拟网络。 最后，选择“保存”以返回“自定义部署”页。

2. 在门户中的“自定义部署”页上，键入或选择以下值：
    - 资源组：选择“新建”，键入资源组的名称，然后选择“确定”。
    - **虚拟网络名称**：键入新虚拟网络的名称。
3. 选择“我同意上述条款和条件”，然后选择“购买” 。

<!--MOONCAKE CUSTOMIZATION till 02/22/2021-->

## <a name="review-deployed-resources"></a>查看已部署的资源

浏览使用虚拟网络创建的资源。

若要了解模板中虚拟网络的 JSON 语法和属性，请参阅 [Microsoft.Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)。

## <a name="clean-up-resources"></a>清理资源

如果不再需要使用虚拟网络创建的资源，请删除资源组。 这会删除该虚拟网络和所有相关资源。

若要删除资源组，请调用 `Remove-AzResourceGroup` cmdlet：

```powershell
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>后续步骤
在本快速入门中，你部署了具有两个子网的 Azure 虚拟网络。 若要详细了解 Azure 虚拟网络，请继续学习虚拟网络的教程。

> [!div class="nextstepaction"]
> [筛选网络流量](tutorial-filter-network-traffic.md)

<!--Update_Description: update meta properties, wording update, update link-->