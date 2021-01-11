---
title: 教程 - 从 Azure 门户导出模板
description: 了解如何使用导出的模板完成模板开发。
origin.date: 09/09/2020
author: rockboyfor
ms.date: 01/11/2021
ms.testscope: yes
ms.testdate: 08/24/2020
ms.topic: tutorial
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: 6286c98ee4f3bf530545ff06b275e1467ae09d30
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022259"
---
# <a name="tutorial-use-exported-template-from-the-azure-portal"></a>教程：从 Azure 门户使用导出的模板

在本教程系列中，你已创建一个用于部署 Azure 存储帐户的模板。 在接下来的两篇教程中，你将添加一个应用服务计划和一个网站。   本教程介绍如何从 Azure 门户导出模板（无需从头开始创建模板），以及如何使用 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/)中的示例模板。 你可以根据自己的用途自定义这些模板。 本教程重点介绍如何导出模板，以及自定义模板的结果。 完成本教程需要大约 **14 分钟**。

## <a name="prerequisites"></a>先决条件

建议完成[有关输出的教程](template-tutorial-add-outputs.md)，但这不是一项要求。

必须已安装带有资源管理器工具扩展的 Visual Studio Code，以及 Azure PowerShell 或 Azure CLI。 有关详细信息，请参阅[模板工具](template-tutorial-create-first-template.md#get-tools)。

## <a name="review-template"></a>审阅模板

在上一篇教程的结束时，模板包含以下 JSON：

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "type": "string",
      "minLength": 3,
      "maxLength": 11
    },
    "storageSKU": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ]
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "uniqueStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[variables('uniqueStorageName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true
      }
    }
  ],
  "outputs": {
    "storageEndpoint": {
      "type": "object",
      "value": "[reference(variables('uniqueStorageName')).primaryEndpoints]"
    }
  }
}
```

此模板非常适合用于部署存储帐户，但可以在其中添加更多的资源。 可以从现有的资源导出模板，以快速获取该资源的 JSON。

## <a name="create-app-service-plan"></a>创建应用服务计划

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 选择“创建资源”。 
1. 在“搜索市场”中输入“应用服务计划”，然后选择“应用服务计划”。     请不要选择“应用服务计划(经典)” 
1. 选择“创建”  。
1. 输入：

    - **订阅**：选择 Azure 订阅。
    - **资源组**：选择“新建”，然后指定名称。  提供的资源组名称应该与过去在本教程系列中使用的名称不同。
    - **名称**：输入应用服务计划的名称。
    - **操作系统**：选择“Linux”。 
    - **区域**：选择一个 Azure 位置。 例如，**中国北部**。
    - **定价层**：为了节省成本，请将 SKU 更改为“基本 B1”（在“开发/测试”下）。 

    :::image type="content" source="./media/template-tutorial-export-template/resource-manager-template-export.png" alt-text="资源管理器模板 - 在门户中导出模板":::
1. 选择“查看并创建”。 
1. 选择“创建”  。 创建资源需要花费片刻时间。

## <a name="export-template"></a>导出模板

1. 选择“转到资源”。 

    :::image type="content" source="./media/template-tutorial-export-template/resource-manager-template-export-go-to-resource.png" alt-text="转到资源":::

1. 选择“导出模板”  。

    :::image type="content" source="./media/template-tutorial-export-template/resource-manager-template-export-template.png" alt-text="资源管理器模板 - 导出模板":::

   导出模板功能将提取资源的当前状态，并生成用于部署该资源的模板。 导出模板可能有助于快速获取部署资源所需的 JSON。

1. 查看导出的模板中的 `Microsoft.Web/serverfarms` 定义和参数定义。 不需要复制这些部分。 可以使用此导出的模板作为示例，了解如何将此资源添加到模板。

    :::image type="content" source="./media/template-tutorial-export-template/resource-manager-template-exported-template.png" alt-text="资源管理器模板 - 导出模板 - 导出的模板":::

> [!IMPORTANT]
> 通常，导出的模板比创建模板时所需的信息更详细。 例如，导出的模板中的 SKU 对象包含五个属性。 此模板是可行的，但你只需使用 `name` 属性。 可以从导出的模板着手，然后根据要求对其进行修改。

## <a name="revise-existing-template"></a>修订现有模板

导出的模板提供所需的大部分 JSON，但你需要根据模板自定义这些 JSON。 请特别注意你的模板与导出的模板之间的参数和变量差异。 很明显，导出过程并不知道你已在模板中定义的参数和变量。

以下示例突出显示了在模板中添加的内容。 其中包含导出的代码以及一些更改。 第一，它会更改参数的名称以符合命名约定。 第二，它对应用服务计划的位置使用 location 参数。 第三，它删除默认值正常的所有属性。

复制整个文件，将模板替换为该文件的内容。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "type": "string",
      "minLength": 3,
      "maxLength": 11
    },
    "storageSKU": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ]
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    },
    "appServicePlanName": {
      "type": "string",
      "defaultValue": "exampleplan"
    }
  },
  "variables": {
    "uniqueStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[variables('uniqueStorageName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true
      }
    },
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2016-09-01",
      "name": "[parameters('appServicePlanName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "B1",
        "tier": "Basic",
        "size": "B1",
        "family": "B",
        "capacity": 1
      },
      "kind": "linux",
      "properties": {
        "perSiteScaling": false,
        "reserved": true,
        "targetWorkerCount": 0,
        "targetWorkerSizeId": 0
      }
    }
  ],
  "outputs": {
    "storageEndpoint": {
      "type": "object",
      "value": "[reference(variables('uniqueStorageName')).primaryEndpoints]"
    }
  }
}
```

## <a name="deploy-template"></a>部署模板

使用 Azure CLI 或 Azure PowerShell 来部署模板。

如果尚未创建资源组，请参阅[创建资源组](template-tutorial-create-first-template.md#create-resource-group)。 此示例假设已根据[第一篇教程](template-tutorial-create-first-template.md#deploy-template)中所述，将 `templateFile` 变量设置为模板文件的路径。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addappserviceplan `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要运行此部署命令，必须具有 Azure CLI 的 [最新版本](https://docs.azure.cn/cli/install-azure-cli)。

```azurecli
az deployment group create \
  --name addappserviceplan \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

> [!NOTE]
> 如果部署失败，请使用 `verbose` 开关获取有关正在创建的资源的信息。 使用 `debug` 开关获取调试的详细信息。

## <a name="verify-deployment"></a>验证部署

可以通过在 Azure 门户中浏览资源组来验证部署。

1. 登录 [Azure 门户](https://portal.azure.cn)。
1. 在左侧菜单中选择“资源组”。 
1. 选择已部署到的资源组。
1. 该资源组包含一个存储帐户和一个应用服务计划。

## <a name="clean-up-resources"></a>清理资源

若要继续学习下一篇教程，则不需删除该资源组。

如果你不打算继续学习，请删除该资源组以清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”  。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择资源组名称。
4. 在顶部菜单中选择“删除资源组”。 

## <a name="next-steps"></a>后续步骤

你已了解如何从 Azure 门户导出模板，以及如何使用导出的模板进行模板开发。 你还可以使用 Azure 快速入门模板来简化模板开发。

> [!div class="nextstepaction"]
> [使用 Azure 快速入门模板](template-tutorial-quickstart-template.md)

<!-- Update_Description: update meta properties, wording update, update link -->