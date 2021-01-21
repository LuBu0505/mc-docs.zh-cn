---
title: 使用 Azure 资源管理器模板创建 Azure 数据资源管理器群集和数据库
description: 了解如何使用 Azure 资源管理器模板创建 Azure 数据资源管理器群集和数据库
author: orspod
ms.author: v-tawe
ms.reviewer: lugoldbe
ms.service: data-explorer
ms.topic: how-to
origin.date: 09/26/2019
ms.date: 01/22/2021
ms.openlocfilehash: 0d8fee0c7d02df63ceb503d83a49f3dfb799d302
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611695"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-an-azure-resource-manager-template"></a>使用 Azure 资源管理器模板创建 Azure 数据资源管理器群集和数据库

> [!div class="op_single_selector"]
> * [门户](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [Go](create-cluster-database-go.md)
> * [Azure Resource Manager 模板](create-cluster-database-resource-manager.md)

Azure 数据资源管理器是一项快速且高度可缩放的数据探索服务，适用于日志和遥测数据。 若要使用 Azure 数据资源管理器，请先创建群集，再在该群集中创建一个或多个数据库。 然后将数据引入（加载）到数据库，以便对其运行查询。 

在本文中，我们使用 [Azure 资源管理器模板](/azure-resource-manager/management/overview)创建 Azure 数据资源管理器群集和数据库。 本文介绍如何定义要部署的资源以及如何定义执行部署时指定的参数。 可将此模板用于自己的部署，或自定义此模板以满足要求。 有关创建模板的信息，请参阅[创作 Azure 资源管理器模板](/azure-resource-manager/resource-group-authoring-templates)。 有关要在模板中使用的 JSON 语法和属性，请参阅 [Microsoft.Kusto 资源类型](https://docs.microsoft.com/azure/templates/microsoft.kusto/allversions)。

如果没有 Azure 订阅，请在开始前创建一个[试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

## <a name="azure-resource-manager-template-for-cluster-and-database-creation"></a>用于创建群集和数据库的 Azure 资源管理器模板

本文使用[现有快速入门模板](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-kusto-cluster-database/azuredeploy.json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "clusters_kustocluster_name": {
          "type": "string",
          "defaultValue": "[concat('kusto', uniqueString(resourceGroup().id))]",
          "metadata": {
            "description": "Name of the cluster to create"
          }
      },
      "databases_kustodb_name": {
          "type": "string",
          "defaultValue": "kustodb",
          "metadata": {
            "description": "Name of the database to create"
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
          "name": "[parameters('clusters_kustocluster_name')]",
          "type": "Microsoft.Kusto/clusters",
          "sku": {
              "name": "Standard_D13_v2",
              "tier": "Standard",
              "capacity": 2
          },
          "apiVersion": "2020-18-09",
          "location": "[parameters('location')]",
          "tags": {
            "Created By": "GitHub quickstart template"
          },
          "properties": {
              "trustedExternalTenants": [],
              "optimizedAutoscale": {
                  "version": 1,
                  "isEnabled": true,
                  "minimum": 2,
                  "maximum": 10
              },
              "enableDiskEncryption": false,
              "enableStreamingIngest": false,
              "virtualNetworkConfiguration":{
                  "subnetId": "<subnet resource id>",
                  "enginePublicIpId": "<Engine service's public IP address resource id>",
                  "dataManagementPublicIpId": "<Data management's service public IP address resource id>"
              },
              "keyVaultProperties":{
                  "keyName": "<Key name>",
                  "keyVaultUri": "<Key vault uri>"
              },
              "enablePurge": false,
              "enableDoubleEncryption": false,
              "engineType": "V3",
          }
      },
      {
          "name": "[concat(parameters('clusters_kustocluster_name'), '/', parameters('databases_kustodb_name'))]",
          "type": "Microsoft.Kusto/clusters/databases",
          "apiVersion": "2020-18-09",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[resourceId('Microsoft.Kusto/clusters', parameters('clusters_kustocluster_name'))]"
          ],
          "properties": {
              "softDeletePeriodInDays": 365,
              "hotCachePeriodInDays": 31
          }
      }
  ]
}
```

若要查找更多模板示例，请参阅 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/)。

## <a name="deploy-the-template-and-verify-template-deployment"></a>部署模板并验证模板部署

可以通过 [Azure 门户](#use-the-azure-portal-to-deploy-the-template-and-verify-template-deployment)或 [powershell](#use-powershell-to-deploy-the-template-and-verify-template-deployment) 部署 Azure 资源管理器模板。

### <a name="use-the-azure-portal-to-deploy-the-template-and-verify-template-deployment"></a>使用 Azure 门户部署模板并验证模板部署

1. 若要创建群集和数据库，请使用以下按钮开始部署。 右键单击并选择“在新窗口中打开”，以便按本文中的剩余步骤操作。

    [![描绘云且标记为“部署到 Azure”的蓝色按钮的屏幕截图。](media/create-cluster-database-resource-manager/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-kusto-cluster-database%2Fazuredeploy.json)

    “部署到 Azure”按钮将转到 Azure 门户以填写部署窗体。

    :::image type="content" source="media/create-cluster-database-resource-manager/deploy-2-azure.png" alt-text="Azure 门户上模板的屏幕截图。编辑中使用的所有按钮、方框和复选框都已突出显示。" border="false":::

    可以使用此窗体[在 Azure 门户中编辑和部署模板](/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal#edit-and-deploy-the-template)。

1. 完成“基本情况”和“设置”部分。 选择唯一的群集和数据库名称。
创建 Azure 数据资源管理器群集和数据库需要数分钟的时间。

1. 若要验证部署，请在 [Azure 门户](https://portal.azure.cn)中打开资源组，找到新的群集和数据库。 

### <a name="use-powershell-to-deploy-the-template-and-verify-template-deployment"></a>使用 PowerShell 部署模板并验证模板部署

#### <a name="deploy-the-template-using-powershell"></a>使用 PowerShell 部署模板

1. 从以下代码块中选择“试一试”，然后按照说明登录到 Azure PowerShell。

    ```powershell
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. chinaeast2)"
    $resourceGroupName = "${projectName}rg"
    $clusterName = "${projectName}cluster"
    $parameters = @{}
    $parameters.Add("clusters_kustocluster_name", $clusterName)
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-kusto-cluster-database/azuredeploy.json"
    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -TemplateParameterObject $parameters
    Write-Host "Press [ENTER] to continue ..."
    ```

1. 选择“复制”以复制 PowerShell 脚本。
1. 右键单击 shell 控制台并选择“粘贴”  。
创建 Azure 数据资源管理器群集和数据库需要数分钟的时间。

#### <a name="verify-the-deployment-using-powershell"></a>使用 PowerShell 验证部署

若要验证部署，请使用以下 Azure PowerShell 脚本。 如果 Azure PowerShell 仍处于打开状态，则无需复制/运行第一行 (Read-Host)。 若要详细了解如何在 PowerShell 中管理 Azure 数据资源管理器资源，请阅读 [Az.Kusto](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-2.7.0)。

```powershell
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"

Install-Module -Name Az.Kusto
$resourceGroupName = "${projectName}rg"
$clusterName = "${projectName}cluster"

Get-AzKustoCluster -ResourceGroupName $resourceGroupName -Name $clusterName
Write-Host "Press [ENTER] to continue ..."
```

[!INCLUDE [data-explorer-clean-resources](includes/data-explorer-clean-resources.md)]

## <a name="next-steps"></a>后续步骤

[将数据引入 Azure 数据资源管理器群集和数据库](ingest-data-overview.md)
