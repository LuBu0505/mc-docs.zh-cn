---
title: 扩展资源类型的范围
description: 介绍如何在部署扩展资源类型时使用 scope 属性。
ms.topic: conceptual
origin.date: 01/13/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes|no
ms.testdate: 11/23/2020
ms.author: v-yeche
ms.openlocfilehash: 83e4824c815fafc3cf963d41b3a5b2f02d966c07
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054339"
---
<!--Verified successfully-->
# <a name="setting-scope-for-extension-resources-in-arm-templates"></a>在 ARM 模板中设置扩展资源的范围

扩展资源是用于修改其他资源的资源。 例如，可以为资源分配角色。 角色分配是扩展资源类型。

有关扩展资源类型的完整列表，请参阅[用于扩展其他资源的功能的资源类型](../management/extension-resource-types.md)。

本文介绍如何在使用 Azure 资源管理器模板（ARM 模板）进行部署时设置扩展资源类型的范围。 它介绍了在应用到资源时可用于扩展资源的 scope 属性。

> [!NOTE]
> scope 属性仅适用于扩展资源类型。 若要为非扩展类型的资源类型指定其他范围，请使用嵌套部署或链接部署。 有关详细信息，请参阅[资源组部署](deploy-to-resource-group.md)、[订阅部署](deploy-to-subscription.md)、[管理组部署](deploy-to-management-group.md)和[租户部署](deploy-to-tenant.md)。

## <a name="apply-at-deployment-scope"></a>在部署范围内应用

若要在目标部署范围内应用扩展资源类型，请将该资源添加到模板中，就像应用任何资源类型一样。 可用的范围是[资源组](deploy-to-resource-group.md)、[订阅](deploy-to-subscription.md)、[管理组](deploy-to-management-group.md)和[租户](deploy-to-tenant.md)。 部署范围必须支持该资源类型。

以下模板会部署一个锁。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {  
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/locks",
            "apiVersion": "2016-09-01",
            "name": "rgLock",
            "properties": {
                "level": "CanNotDelete",
                "notes": "Resource Group should not be deleted."
            }
        }
    ]
}
```

部署到资源组时，它会锁定资源组。

[!INCLUDE [azure-resource-manager-update-templateurl-parameter-china](../../../includes/azure-resource-manager-update-templateurl-parameter-china.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group create \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/locktargetscope.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
 New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/locktargetscope.json"
```

---

下一个示例会分配角色。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "roleNameGuid": {
            "type": "string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "A new GUID used to identify the role assignment"
            }
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2020-04-01-preview",
            "name": "[parameters('roleNameGuid')]",
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ],
    "outputs": {}
}
```

部署到订阅时，它会为订阅分配角色。

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment sub create \
  --name demoSubDeployment \
  --location chinaeast \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/roletargetscope.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location chinaeast `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/roletargetscope.json"
```

---

## <a name="apply-to-resource"></a>应用于资源

若要将扩展资源应用于资源，请使用 `scope` 属性。 将 scope 属性设置为要将扩展添加到其中的资源的名称。 scope 属性是扩展资源类型的根属性。

下面的示例创建一个存储帐户并对其应用角色。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "defaultValue": "[newGuid()]",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    },
    "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[parameters('location')]",
      "sku": {
          "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    },
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2020-04-01-preview",
      "name": "[parameters('roleNameGuid')]",
      "scope": "[concat('Microsoft.Storage/storageAccounts', '/', variables('storageName'))]",
      "dependsOn": [
          "[variables('storageName')]"
      ],
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]"
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何在模板中定义参数，请参阅[了解 ARM 模板的结构和语法](template-syntax.md)。
* 有关解决常见部署错误的提示，请参阅[排查使用 Azure Resource Manager 时的常见 Azure 部署错误](common-deployment-errors.md)。
* 有关部署需要 SAS 令牌的模板的信息，请参阅[使用 SAS 令牌部署专用 ARM 模板](secure-template-with-sas-token.md)。

<!--Update_Description: update meta properties, wording update, update link-->