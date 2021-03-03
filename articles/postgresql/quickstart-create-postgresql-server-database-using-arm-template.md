---
title: 快速入门：创建 Azure DB for PostgreSQL - ARM 模板
description: 本快速入门介绍如何使用 Azure 资源管理器模板创建 Azure Database for PostgreSQL 单一服务器。
author: WenJason
ms.service: postgresql
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: v-jay
origin.date: 02/11/2021
ms.date: 03/08/2021
ms.openlocfilehash: 32f01c1a75e88c15de006ce0ff9bd24bee663916
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697750"
---
# <a name="quickstart-use-an-arm-template-to-create-an-azure-database-for-postgresql---single-server"></a>快速入门：使用 ARM 模板创建 Azure Database for PostgreSQL 单一服务器

用于 PostgreSQL 的 Azure 数据库是一种托管服务，可用于在云中运行、管理和缩放具有高可用性的 PostgreSQL 数据库。 在本快速入门中，使用 Azure 资源管理器模板（ARM 模板）在 Azure 门户、PowerShell 或 Azure CLI 中创建 Azure Database for PostgreSQL 单一服务器。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

如果你的环境满足先决条件，并且你熟悉如何使用 ARM 模板，请选择“部署到 Azure”按钮。 Azure 门户中会打开模板。

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="部署到 Azure":::](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-managed-postgresql-with-vnet%2fazuredeploy.json)

## <a name="prerequisites"></a>先决条件

# <a name="portal"></a>[Portal](#tab/azure-portal)

具有活动订阅的 Azure 帐户。 [创建一个试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

* 具有活动订阅的 Azure 帐户。 [创建一个试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* 若要在本地运行代码，请安装 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)。

# <a name="cli"></a>[CLI](#tab/CLI)

* 具有活动订阅的 Azure 帐户。 [创建一个试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* 若要在本地运行代码，请安装 [Azure CLI](/cli/)。

---

## <a name="review-the-template"></a>查看模板

创建的 Azure Database for PostgreSQL 服务器中包含一组已配置的计算和存储资源。 若要了解详细信息，请参阅[Azure Database for PostgreSQL 中的定价层 - 单一服务器](concepts-pricing-tiers.md)。 请在 [Azure 资源组](../azure-resource-manager/management/overview.md)中创建该服务器。

本快速入门中使用的模板来自 [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/101-managed-postgresql-with-vnet/)。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serverName": {
      "type": "string",
      "metadata": {
        "description": "Server Name for Azure database for PostgreSQL"
      }
    },
    "administratorLogin": {
      "type": "string",
      "minLength": 1,
      "metadata": {
        "description": "Database administrator login name"
      }
    },
    "administratorLoginPassword": {
      "type": "securestring",
      "minLength": 8,
      "metadata": {
        "description": "Database administrator password"
      }
    },
    "skuCapacity": {
      "type": "int",
      "defaultValue": 2,
      "metadata": {
        "description": "Azure database for PostgreSQL compute capacity in vCores (2,4,8,16,32)"
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "GP_Gen5_2",
      "metadata": {
        "description": "Azure database for PostgreSQL sku name "
      }
    },
    "skuSizeMB": {
      "type": "int",
      "defaultValue": 51200,
      "metadata": {
        "description": "Azure database for PostgreSQL Sku Size "
      }
    },
    "skuTier": {
      "type": "string",
      "defaultValue": "GeneralPurpose",
      "metadata": {
        "description": "Azure database for PostgreSQL pricing tier"
      }
    },
    "skuFamily": {
      "type": "string",
      "defaultValue": "Gen5",
      "metadata": {
        "description": "Azure database for PostgreSQL sku family"
      }
    },
    "postgresqlVersion": {
      "type": "string",
      "defaultValue": "11",
      "allowedValues": [
        "9.5",
        "9.6",
        "10",
        "11"
      ],
      "metadata": {
        "description": "PostgreSQL version"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    },
    "backupRetentionDays": {
      "type": "int",
      "defaultValue": 7,
      "metadata": {
        "description": "PostgreSQL Server backup retention days"
      }
    },
    "geoRedundantBackup": {
      "type": "string",
      "defaultValue": "Disabled",
      "metadata": {
        "description": "Geo-Redundant Backup setting"
      }
    },
    "virtualNetworkName": {
      "type": "string",
      "defaultValue": "azure_postgresql_vnet",
      "metadata": {
        "description": "Virtual Network Name"
      }
    },
    "subnetName": {
      "type": "string",
      "defaultValue": "azure_postgresql_subnet",
      "metadata": {
        "description": "Subnet Name"
      }
    },
    "virtualNetworkRuleName": {
      "type": "string",
      "defaultValue": "AllowSubnet",
      "metadata": {
        "description": "Virtual Network RuleName"
      }
    },
    "vnetAddressPrefix": {
      "type": "string",
      "defaultValue": "10.0.0.0/16",
      "metadata": {
        "description": "Virtual Network Address Prefix"
      }
    },
    "subnetPrefix": {
      "type": "string",
      "defaultValue": "10.0.0.0/16",
      "metadata": {
        "description": "Subnet Address Prefix"
      }
    }
  },
  "variables": {
    "firewallrules": {
      "batch": {
        "rules": [
          {
            "Name": "rule1",
            "StartIpAddress": "0.0.0.0",
            "EndIpAddress": "255.255.255.255"
          },
          {
            "Name": "rule2",
            "StartIpAddress": "0.0.0.0",
            "EndIpAddress": "255.255.255.255"
          }
        ]
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2020-06-01",
      "name": "[parameters('virtualNetworkName')]",
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
          "apiVersion": "2020-06-01",
          "name": "[parameters('subnetName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[parameters('virtualNetworkName')]"
          ],
          "properties": {
            "addressPrefix": "[parameters('subnetPrefix')]"
          }
        }
      ]
    },
    {
      "type": "Microsoft.DBforPostgreSQL/servers",
      "apiVersion": "2017-12-01",
      "name": "[parameters('serverName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('skuName')]",
        "tier": "[parameters('skuTier')]",
        "capacity": "[parameters('skuCapacity')]",
        "size": "[parameters('skuSizeMB')]",
        "family": "[parameters('skuFamily')]"
      },
      "properties": {
        "createMode": "Default",
        "version": "[parameters('postgresqlVersion')]",
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
        "storageProfile": {
          "storageMB": "[parameters('skuSizeMB')]",
          "backupRetentionDays": "[parameters('backupRetentionDays')]",
          "geoRedundantBackup": "[parameters('geoRedundantBackup')]"
        }
      },
      "resources": [
        {
          "type": "virtualNetworkRules",
          "apiVersion": "2017-12-01",
          "name": "[parameters('virtualNetworkRuleName')]",
          "dependsOn": [
            "[resourceId('Microsoft.DBforPostgreSQL/servers/', parameters('serverName'))]"
          ],
          "properties": {
            "virtualNetworkSubnetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnetName'))]",
            "ignoreMissingVnetServiceEndpoint": true
          }
        }
      ]
    },
    {
      "type": "Microsoft.DBforPostgreSQL/servers/firewallRules",
      "apiVersion": "2017-12-01",
      "name": "[concat(parameters('serverName'),'/',variables('firewallrules').batch.rules[copyIndex()].Name)]",
      "location": "[parameters('location')]",
      "copy": {
        "name": "firewallRulesCopy",
        "mode": "Serial",
        "batchSize": 1,
        "count": "[length(variables('firewallrules').batch.rules)]"
      },
      "dependsOn": [
        "[resourceId('Microsoft.DBforPostgreSQL/servers/', parameters('serverName'))]"
      ],
      "properties": {
        "startIpAddress": "[variables('firewallrules').batch.rules[copyIndex()].StartIpAddress]",
        "endIpAddress": "[variables('firewallrules').batch.rules[copyIndex()].EndIpAddress]"
      }
    }
  ]
}
```

该模板定义了五个 Azure 资源：

* [**Microsoft.Network/virtualNetworks**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
* [**Microsoft.Network/virtualNetworks/subnets**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)
* [**Microsoft.DBforPostgreSQL/servers**](https://docs.microsoft.com/azure/templates/microsoft.dbforpostgresql/servers)
* [**Microsoft.DBforPostgreSQL/servers/virtualNetworkRules**](https://docs.microsoft.com/azure/templates/microsoft.dbforpostgresql/servers/virtualnetworkrules)
* [**Microsoft.DBforPostgreSQL/servers/firewallRules**](https://docs.microsoft.com/azure/templates/microsoft.dbforpostgresql/servers/firewallrules)

可以在 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Dbforpostgresql&pageNumber=1&sort=Popular)中找到更多 Azure Database for PostgreSQL 模板示例。

## <a name="deploy-the-template"></a>部署模板

# <a name="portal"></a>[Portal](#tab/azure-portal)

选择以下链接以在 Azure 门户中部署 Azure Database for PostgreSQL 服务器模板：

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="部署到 Azure":::](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-managed-postgresql-with-vnet%2fazuredeploy.json)

在“部署具有 VNet 的 Azure Database for PostgreSQL”页上：

1. 对于“资源组”，请选择“新建”，输入新资源组的名称，然后选择“确定”  。

2. 如果你创建了一个新资源组，请为该资源组和新服务器选择一个位置。

3. 输入服务器名称、管理员登录名和管理员登录密码  。

    :::image type="content" source="./media/quickstart-create-postgresql-server-database-using-arm-template/deploy-azure-database-for-postgresql-with-vnet.png" alt-text="“部署具有 VNet 的 Azure Database for PostgreSQL”窗口，Azure 快速入门模板，Azure 门户":::

4. 根据需要更改其他默认设置：

    * **订阅**：要用于服务器的 Azure 订阅。
    * **SKU 容量**：vCore 容量，值可以是 2（默认值）、4、8、16、32 或 64     。
    * **SKU 名称**：SKU 层前缀、SKU 系列和 SKU 容量，以下划线联接，例如 B_Gen5_1、GP_Gen5_2（默认值）或 MO_Gen5_32  。
    * **SKU 大小(MB)** ：Azure Database for PostgreSQL 服务器的存储大小，以 MB 为单位（默认值为 51200）。
    * **SKU 层**：部署层，例如“基本”、“常规用途”（默认值）或“内存优化”  。
    * **SKU 系列**：“Gen4”或“Gen5”（默认值），表示服务器部署的硬件代系 。
    * **PostgreSQL 版本**：要部署的 PostgreSQL 服务器版本，例如 9.5、9.6、10 或 11（默认值）   。
    * **备份保留天数**：异地冗余备份的所需保留期，以天为单位（默认值为 7）。
    * **异地冗余备份**：“已启用”或“已禁用”（默认值），具体取决于异地灾难恢复 (Geo-DR) 要求 。
    * **虚拟网络名称**：虚拟网络的名称（默认值为 azure_postgresql_vnet）。
    * **子网名称**：子网的名称（默认值为 azure_postgresql_subnet）。
    * **虚拟网络规则名称**：允许子网的虚拟网络规则的名称（默认值为 AllowSubnet）。
    * **VNet 地址前缀**：虚拟网络的地址前缀（默认值为 10.0.0.0/16）。
    * **子网前缀**：子网的地址前缀（默认值为 10.0.0.0/16）。

5. 阅读条款和条件，并选择“我同意上述条款和条件”。

6. 选择“购买”。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

使用以下交互式代码通过模板创建新的 Azure Database for PostgreSQL 服务器。 该代码会提示你输入新服务器名称、新资源组的名称和位置，以及管理员帐户名称和密码。

```azurepowershell
$serverName = Read-Host -Prompt "Enter a name for the new Azure Database for PostgreSQL server"
$resourceGroupName = Read-Host -Prompt "Enter a name for the new resource group where the server will exist"
$location = Read-Host -Prompt "Enter an Azure region (for example, chinanorth2) for the resource group"
$adminUser = Read-Host -Prompt "Enter the Azure Database for PostgreSQL server's administrator account name"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location # Use this command when you need to create a new resource group for your deployment
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-managed-postgresql-with-vnet/azuredeploy.json `
    -serverName $serverName `
    -administratorLogin $adminUser `
    -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue: "
```

# <a name="cli"></a>[CLI](#tab/CLI)

使用以下交互式代码通过模板创建新的 Azure Database for PostgreSQL 服务器。 该代码会提示你输入新服务器名称、新资源组的名称和位置，以及管理员帐户名称和密码。

```azurecli
read -p "Enter a name for the new Azure Database for PostgreSQL server:" serverName &&
read -p "Enter a name for the new resource group where the server will exist:" resourceGroupName &&
read -p "Enter an Azure region (for example, chinanorth2) for the resource group:" location &&
read -p "Enter the Azure Database for PostgreSQL server's administrator account name:" adminUser &&
read -p "Enter the administrator password:" adminPassword &&
params='serverName='$serverName' administratorLogin='$adminUser' administratorLoginPassword='$adminPassword &&
az group create --name $resourceGroupName --location $location &&
az deployment group create --resource-group $resourceGroupName --parameters $params --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-managed-postgresql-with-vnet/azuredeploy.json &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="review-deployed-resources"></a>查看已部署的资源

# <a name="portal"></a>[Portal](#tab/azure-portal)

按照以下步骤查看新 Azure Database for PostgreSQL 服务器的概览：

1. 在 [Azure 门户](https://portal.azure.cn)中，搜索并选择“Azure Database for PostgreSQL 服务器”。

2. 在数据库列表中选择你的新服务器。 随后会显示新 Azure Database for PostgreSQL 服务器的“概览”页。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

运行以下交互式代码来查看有关 Azure Database for PostgreSQL 服务器的详细信息。 必须输入新服务器的名称。

```azurepowershell
$serverName = Read-Host -Prompt "Enter the name of your Azure Database for PostgreSQL server"
Get-AzResource -ResourceType "Microsoft.DbForPostgreSQL/servers" -Name $serverName | ft
Read-Host -Prompt "Press [ENTER] to continue: "
```

# <a name="cli"></a>[CLI](#tab/CLI)

运行以下交互式代码来查看有关 Azure Database for PostgreSQL 服务器的详细信息。 必须输入新服务器的名称和资源组。

```azurecli
read -p "Enter your Azure Database for PostgreSQL server name: " serverName &&
read -p "Enter the resource group where the Azure Database for PostgreSQL server exists: " resourcegroupName &&
az resource show --resource-group $resourcegroupName --name $serverName --resource-type "Microsoft.DbForPostgreSQL/servers" &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="exporting-arm-template-from-the-portal"></a>从门户导出 ARM 模板
可从 Azure 门户[导出 ARM 模板](../azure-resource-manager/templates/export-template-portal.md)。 可以通过两种方式来导出模板：

- [从资源组或资源导出](../azure-resource-manager/templates/export-template-portal.md#export-template-from-a-resource)。 此选项基于现有的资源生成新模板。 导出的模板是资源组当前状态的“快照”。 可以导出整个资源组，或该资源组中的特定资源。
- [在部署之前导出或从历史记录导出](../azure-resource-manager/templates/export-template-portal.md#export-template-before-deployment)。 此选项检索用于部署的确切模板副本。

导出模板时，将在 PostgreSQL 服务器资源的 ```"properties":{ }``` 部分中看到，出于安全原因不会包含 ```administratorLogin``` 和 ```administratorLoginPassword```。 部署模板前，必须将这些参数添加到模板中，否则模板会失败。

```
"resources": [
    {
      "type": "Microsoft.DBforPostgreSQL/servers",
      "apiVersion": "2017-12-01",
      "name": "[parameters('servers_name')]",
      "location": "chinaeast2",
      "sku": {
                "name": "B_Gen5_1",
                "tier": "Basic",
                "family": "Gen5",
                "capacity": 1
            },
      "properties": {
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
```



## <a name="clean-up-resources"></a>清理资源

如果不再需要该资源组，可以将其删除，这将删除资源组中的资源。

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. 在 [Azure 门户](https://portal.azure.cn)中，搜索并选择“资源组”。

2. 在资源组列表中，选择你的资源组的名称。

3. 在资源组的“概览”页中，选择“删除资源组” 。

4. 在确认对话框中，键入资源组的名称，然后选择“删除”。

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Read-Host -Prompt "Press [ENTER] to continue: "
```

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
read -p "Enter the Resource Group name: " resourceGroupName &&
az group delete --name $resourceGroupName &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="next-steps"></a>后续步骤

有关引导你完成模板创建过程的分步教程，请参阅：

> [!div class="nextstepaction"]
> [教程：创建和部署你的第一个 ARM 模板](../azure-resource-manager/templates/template-tutorial-create-first-template.md)
