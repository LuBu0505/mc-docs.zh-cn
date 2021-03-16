---
title: 将资源部署到订阅
description: 介绍了如何在 Azure 资源管理器模板中创建资源组。 它还展示了如何在 Azure 订阅范围内部署资源。
ms.topic: conceptual
origin.date: 01/13/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 08/24/2020
ms.author: v-yeche
ms.openlocfilehash: 73d38ad8a4df0b1b9c5668dab4cbe0d0304cb934
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052771"
---
<!--Verify Successfully-->
# <a name="subscription-deployments-with-arm-templates"></a>使用 ARM 模板进行订阅部署

若要简化资源管理，可以使用 Azure 资源管理器模板（ARM 模板）在 Azure 订阅级别部署资源。 例如，可以将[策略](../../governance/policy/overview.md)和 [Azure 基于角色的访问控制 (Azure RBAC)](../../role-based-access-control/overview.md) 部署到你的订阅中，从而将它们应用于整个订阅。 还可以在订阅中创建资源组，然后将资源部署到订阅中的资源组。

> [!NOTE]
> 可在订阅级别部署中部署到 800 个不同的资源组。

若要在订阅级别部署模板，请使用 Azure CLI、PowerShell、REST API 或门户。

<!--Mooncake Customization on ## Supported resources-->

## <a name="supported-resources"></a>支持的资源

并非所有资源类型都可以部署到订阅级别。 本部分列出了支持的资源类型。

对于 Azure 蓝图，请使用：

* 项目
* blueprints
* blueprintAssignments
* versions

对于 Azure 策略，请使用：

* policyAssignments
* policyDefinitions
* policySetDefinitions
* remediations

对于 Azure 基于角色的访问控制 (Azure RBAC)，请使用：

* roleAssignments
* roleDefinitions

对于部署到资源组的嵌套模板，请使用：

* deployments

若要创建新的资源组，请使用：

* resourceGroups

若要管理订阅，请使用：

* 顾问配置
* 预算
* 更改分析配置文件
* supportPlanTypes
* 标记

其他支持的类型包括：

* scopeAssignments
* eventSubscriptions
* peerAsns

<!--Mooncake Customization on ## Supported resources-->

## <a name="schema"></a>架构

用于订阅级别部署的架构不同于资源组部署的架构。

对于模板，请使用：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    ...
}
```

对于所有部署范围，参数文件的架构都相同。 对于参数文件，请使用：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>部署命令

若要部署到订阅，请使用订阅级别部署命名。

[!INCLUDE [azure-resource-manager-update-templateurl-parameter-china](../../../includes/azure-resource-manager-update-templateurl-parameter-china.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

对于 Azure CLI，请使用 [az deployment sub create](https://docs.azure.cn/cli/deployment/sub#az_deployment_sub_create)。 以下示例会部署一个模板来创建资源组：

```azurecli
az deployment sub create \
  --name demoSubDeployment \
  --location chinaeast \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" \
  --parameters rgName=demoResourceGroup rgLocation=chinaeast
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

对于 PowerShell 部署命令，请使用 [New-AzDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azdeployment) 或其别名 `New-AzSubscriptionDeployment`。 以下示例会部署一个模板来创建资源组：

```powershell
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location chinaeast `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" `
  -rgName demoResourceGroup `
  -rgLocation chinaeast
```

---

有关部署命令和部署 ARM 模板的选项的更多详细信息，请参阅：

* [使用 ARM 模板和 Azure 门户部署资源](deploy-portal.md)
* [使用 ARM 模板和 Azure CLI 部署资源](deploy-cli.md)
* [使用 ARM 模板和 Azure PowerShell 部署资源](deploy-powershell.md)
* [使用 ARM 模板和 Azure 资源管理器 REST API 部署资源](deploy-rest.md)
* [使用部署按钮从 GitHub 存储库部署模板](deploy-to-azure-button.md)

<!--NOT AVAILABLE ON * [Deploy ARM templates from local Shell](deploy-cloud-shell.md)-->

## <a name="deployment-location-and-name"></a>部署位置和名称

对于订阅级别部署，必须为部署提供位置。 部署位置独立于部署的资源的位置。 部署位置指定何处存储部署数据。 [管理组](deploy-to-management-group.md)和[租户](deploy-to-tenant.md)部署也需要位置。 对于[资源组](deploy-to-resource-group.md)部署，资源组的位置用于存储部署数据。

可以为部署提供一个名称，也可以使用默认部署名称。 默认名称是模板文件的名称。 例如，部署一个名为 _azuredeploy.json_ 的模板将创建默认部署名称 **azuredeploy**。

每个部署名称的位置不可变。 当某个位置中已有某个部署时，无法在另一位置创建同名的部署。 例如，如果在 chinaeast 中创建名为“deployment1”的订阅部署，则以后不能创建另一个名为“deployment1”但位置为“chinanorth”的部署。 如果出现错误代码 `InvalidDeploymentLocation`，请使用其他名称或使用与该名称的以前部署相同的位置。

## <a name="deployment-scopes"></a>部署范围

部署到订阅时，可以将资源部署到：

* 操作中的目标订阅
* 租户中的任何订阅
* 该订阅或其他订阅中的资源组
* 订阅的租户

可以将[扩展资源](scope-extension-resources.md)的范围设置为与部署目标不同的范围。

部署模板的用户必须有权访问指定的作用域。

本部分演示如何指定不同范围。 可以在单个模板中组合这些不同范围。

### <a name="scope-to-target-subscription"></a>将范围限制为目标订阅

若要将资源部署到目标订阅，请将这些资源添加到模板的资源部分。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        subscription-level-resources
    ],
    "outputs": {}
}
```

有关部署到订阅的示例，请参阅[创建资源组](#create-resource-groups)和[分配策略定义](#assign-policy-definition)。

### <a name="scope-to-other-subscription"></a>将范围限制为其他订阅

若要将资源部署到与操作中的订阅不同的订阅，请添加嵌套部署。 将 `subscriptionId` 属性设置为要部署到的订阅的 ID。 为嵌套部署设置 `location` 属性。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "nestedDeployment",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "location": "chinanorth",
            "properties": {
                "mode": "Incremental",
                "template": {
                    subscription-resources
                }
            }
        }
    ],
    "outputs": {}
}
```

### <a name="scope-to-resource-group"></a>将范围限定于资源组

若要将资源部署到订阅中的资源组，请添加嵌套部署并包括 `resourceGroup` 属性。 在以下示例中，嵌套部署以名为 `demoResourceGroup` 的资源组为目标。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "nestedDeployment",
            "resourceGroup": "demoResourceGroup",
            "properties": {
                "mode": "Incremental",
                "template": {
                    resource-group-resources
                }
            }
        }
    ],
    "outputs": {}
}
```

有关部署到资源组的示例，请参阅[创建资源组和资源](#create-resource-group-and-resources)。

### <a name="scope-to-tenant"></a>将范围设定为租户

若要在租户中创建资源，请将 `scope` 设置为 `/`。 部署模板的用户必须具有[在租户中进行部署所需的访问权限](deploy-to-tenant.md#required-access)。

若要使用嵌套部署，请设置 `scope` 和 `location`。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "nestedDeployment",
            "location": "chinaeast",
            "scope": "/",
            "properties": {
                "mode": "Incremental",
                "template": {
                    tenant-resources
                }
            }
        }
    ],
    "outputs": {}
}
```

或者，可将某些资源类型（如管理组）的范围设置为 `/`。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mgName": {
            "type": "string",
            "defaultValue": "[concat('mg-', uniqueString(newGuid()))]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Management/managementGroups",
            "apiVersion": "2020-05-01",
            "name": "[parameters('mgName')]",
            "scope": "/",
            "location": "chinaeast",
            "properties": {}
        }
    ],
    "outputs": {
        "output": {
            "type": "string",
            "value": "[parameters('mgName')]"
        }
    }
}
```

有关详细信息，请参阅[管理组](deploy-to-management-group.md#management-group)。

## <a name="resource-groups"></a>资源组

### <a name="create-resource-groups"></a>创建资源组

若要在 ARM 模板中创建资源组，请为该资源组定义包含名称和位置的 Microsoft.Resources/resourceGroups 资源。

<!--Not Available on [Microsoft.Resources/resourceGroups](https://docs.microsoft.com/azure/templates/microsoft.resources/allversions)-->

以下模板创建空资源组。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-06-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    }
  ],
  "outputs": {}
}
```

结合使用 [copy 元素](copy-resources.md)与资源组来创建多个资源组。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgNamePrefix": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "instanceCount": {
      "type": "int"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-06-01",
      "location": "[parameters('rgLocation')]",
      "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
      "copy": {
        "name": "rgCopy",
        "count": "[parameters('instanceCount')]"
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

有关资源迭代的信息，请参阅 [ARM 模板中的资源迭代](./copy-resources.md)和[教程：使用 ARM 模板创建多个资源实例](./template-tutorial-create-multiple-instances.md)。

### <a name="create-resource-group-and-resources"></a>创建资源组和资源

若要创建资源组并向其部署资源，请使用嵌套模板。 嵌套模板定义要部署到资源组的资源。 将嵌套模板设置为依赖于资源组，确保资源组存在，然后再部署资源。 最多可部署到 800 个资源组。

以下示例将创建一个资源组，并向该资源组部署存储帐户。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "storagePrefix": {
      "type": "string",
      "maxLength": 11
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
  },
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-06-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "storageDeployment",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2019-06-01",
              "name": "[variables('storageName')]",
              "location": "[parameters('rgLocation')]",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "StorageV2"
            }
          ],
          "outputs": {}
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="azure-policy"></a>Azure Policy

### <a name="assign-policy-definition"></a>分配策略定义

以下示例将现有的策略定义分配到订阅。 如果策略定义使用参数，请将参数作为对象提供。 如果策略定义不使用参数，请使用默认的空对象。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string"
    },
    "policyName": {
      "type": "string"
    },
    "policyParameters": {
      "type": "object",
      "defaultValue": {}
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

若要使用 Azure CLI 部署此模板，请使用：

```azurecli
# Built-in policy definition that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment sub create \
  --name demoDeployment \
  --location chinaeast \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['chinanorth']} }"
```

若要使用 PowerShell 部署此模板，请使用：

```powershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("chinanorth", "chinanorth2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzSubscriptionDeployment `
  -Name policyassign `
  -Location chinaeast `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="create-and-assign-policy-definitions"></a>创建和分配策略定义

可在同一模板中[定义](../../governance/policy/concepts/definition-structure.md)和分配策略定义。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
      "properties": {
        "policyType": "Custom",
        "parameters": {},
        "policyRule": {
          "if": {
            "field": "location",
            "equals": "chinaeast2"
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    },
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-05-01",
      "name": "location-lock",
      "dependsOn": [
        "locationpolicy"
      ],
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[subscriptionResourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
      }
    }
  ]
}
```

若要在订阅中创建策略定义，然后将其分配到订阅，请使用以下 CLI 命令：

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location chinaeast \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

若要使用 PowerShell 部署此模板，请使用：

```azurepowershell
New-AzSubscriptionDeployment `
  -Name definePolicy `
  -Location chinaeast `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

<!--MOONCAKE CUSTOMIZTION: CORRECT ON Azure Blueprints-->

## <a name="azure-blueprints"></a>Azure 蓝图

### <a name="create-blueprint-definition"></a>创建蓝图定义

可通过模板创建蓝图定义。

<!--NOT AVAILABLE ON [create](../../governance/blueprints/tutorials/create-from-sample.md)-->

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "blueprintName": {
      "defaultValue": "sample-blueprint",
      "type": "String",
      "metadata": {
        "description": "The name of the blueprint definition."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Blueprint/blueprints",
      "apiVersion": "2018-11-01-preview",
      "name": "[parameters('blueprintName')]",
      "properties": {
        "targetScope": "subscription",
        "description": "Blueprint with a policy assignment artifact.",
        "resourceGroups": {
          "sampleRg": {
            "description": "Resource group to add the assignment to."
          }
        },
        "parameters": {
          "listOfResourceTypesNotAllowed": {
            "type": "array",
            "metadata": {
              "displayName": "Resource types to pass to the policy assignment artifact."
            },
            "defaultValue": [
              "Citrix.Cloud/accounts"
            ]
          }
        }
      }
    },
    {
      "type": "Microsoft.Blueprint/blueprints/artifacts",
      "apiVersion": "2018-11-01-preview",
      "name": "[concat(parameters('blueprintName'), '/policyArtifact')]",
      "kind": "policyAssignment",
      "dependsOn": [
        "[parameters('blueprintName')]"
      ],
      "properties": {
        "displayName": "Blocked Resource Types policy definition",
        "description": "Block certain resource types",
        "policyDefinitionId": "[tenantResourceId('Microsoft.Authorization/policyDefinitions', '6c112d4e-5bc7-47ae-a041-ea2d9dccd749')]",
        "resourceGroup": "sampleRg",
        "parameters": {
          "listOfResourceTypesNotAllowed": {
            "value": "[[parameters('listOfResourceTypesNotAllowed')]"
          }
        }
      }
    }
  ]
}
```

若要在订阅中创建蓝图定义，请使用以下 CLI 命令：

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location chinaeast \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/blueprints-new-blueprint/azuredeploy.json"
```

若要使用 PowerShell 部署此模板，请使用：

```azurepowershell
New-AzSubscriptionDeployment `
  -Name demoDeployment `
  -Location chinaeast `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/blueprints-new-blueprint/azuredeploy.json"
```

## <a name="access-control"></a>访问控制

若要了解如何分配角色，请参阅[使用 Azure 资源管理器模板添加 Azure 角色分配](../../role-based-access-control/role-assignments-template.md)。

以下示例创建一个资源组，对其应用锁定，并为主体分配一个角色。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the resourceGroup to create"
      }
    },
    "rgLocation": {
      "type": "string",
      "metadata": {
        "description": "Location for the resourceGroup"
      }
    },
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "principalId of the user that will be given contributor access to the resourceGroup"
      }
    },
    "roleDefinitionId": {
      "type": "string",
      "defaultValue": "b24988ac-6180-42a0-ab88-20f7382dd24c",
      "metadata": {
        "description": "roleDefinition to apply to the resourceGroup - default is contributor"
      }
    },
    "roleAssignmentName": {
      "type": "string",
      "defaultValue": "[guid(parameters('principalId'), parameters('roleDefinitionId'), parameters('rgName'))]",
      "metadata": {
        "description": "Unique name for the roleAssignment in the format of a guid"
      }
    }
  },
  "variables": { },
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2019-10-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "tags": {
        "Note": "subscription level deployment"
      },
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-10-01",
      "name": "applyLock",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[parameters('rgName')]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.Authorization/locks",
              "apiVersion": "2017-04-01",
              "name": "DontDelete",
              "properties": {
                "level": "CanNotDelete",
                "notes": "Prevent deletion of the resourceGroup"
              }
            },
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2020-04-01-preview",
              "name": "[guid(parameters('roleAssignmentName'))]",
              "properties": {
                "roleDefinitionId": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[subscriptionResourceId('Microsoft.Resources/resourceGroups', parameters('rgName'))]"
              }
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

* 若要通过示例来了解如何为 Azure 安全中心部署工作区设置，请参阅 [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)。
* 示例模板可在 [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments) 找到。
* 还可在[管理组级别](deploy-to-management-group.md)和[租户级别](deploy-to-tenant.md)部署模板。

<!--Update_Description: update meta properties, wording update, update link-->