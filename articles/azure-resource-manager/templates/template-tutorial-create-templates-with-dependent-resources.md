---
title: 具有依赖资源的模板
description: 了解如何使用多个资源创建 Azure 资源管理器模板，以及如何使用 Azure 门户部署该模板
origin.date: 04/23/2020
author: rockboyfor
ms.date: 01/11/2021
ms.testscope: yes
ms.testdate: 08/24/2020
ms.topic: tutorial
ms.author: v-yeche
ms.openlocfilehash: 20432ca7e4f1bc66fc52ca544afb5df923924084
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022263"
---
<!--Verify successfully on 06/17/2020 by harris-->
# <a name="tutorial-create-arm-templates-with-dependent-resources"></a>教程：创建包含所依赖资源的 ARM 模板

了解如何创建 Azure 资源管理器模板（ARM 模板）以部署多个资源和配置部署顺序。 创建模板后，使用本地 Shell 部署该模板。

<!--Not Available on Cloud Shell-->

本教程介绍如何创建存储帐户、虚拟机、虚拟网络以及一些其他的依赖资源。 某些资源的部署依赖于另一资源的存在。 例如，创建虚拟机的前提是其存储帐户和网络接口存在。 可通过将一个资源标记为依赖于其他资源来定义此关系。 Resource Manager 将评估资源之间的依赖关系，并根据其依赖顺序进行部署。 如果资源互不依赖，资源管理器将以并行方式部署资源。 有关详细信息，请参阅[在 ARM 模板中定义部署资源的顺序](./define-resource-dependency.md)。

:::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-dependent-resources-diagram.png" alt-text="资源管理器模板依赖资源部署顺序图":::

本教程涵盖以下任务：

> [!div class="checklist"]
> * 打开快速入门模板
> * 浏览模板
> * 部署模板

如果没有 Azure 订阅，请在开始前[创建试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

<!--Not Available on [Manage complex cloud deployments by using advanced ARM template features](https://docs.microsoft.com/learn/modules/manage-deployments-advanced-arm-template-features/)-->

## <a name="prerequisites"></a>先决条件

若要完成本文，需要做好以下准备：

* 包含资源管理器工具扩展的 Visual Studio Code。 请参阅[快速入门：使用 Visual Studio Code 创建 ARM 模板](quickstart-create-templates-use-visual-studio-code.md)。
* 若要提高安全性，请使用为虚拟机管理员帐户生成的密码。 以下是密码生成示例：

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault 旨在保护加密密钥和其他机密。 有关详细信息，请参阅[教程：在 ARM 模板部署中集成 Azure Key Vault](./template-tutorial-use-key-vault.md)。 我们还建议你每三个月更新一次密码。

## <a name="open-a-quickstart-template"></a>打开快速入门模板

Azure 快速入门模板是 ARM 模板的存储库。 无需从头开始创建模板，只需找到一个示例模板并对其自定义即可。 本教程中使用的模板称为[部署简单的 Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/)。

1. 在 Visual Studio Code 中，选择“文件” > “打开文件”。 
2. 在“文件名”中粘贴以下 URL：

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. 选择“打开”以打开该文件。
4. 选择“文件” > “另存为”，将该文件的副本保存到名为 _azuredeploy.json_ 的本地计算机。 

## <a name="explore-the-template"></a>浏览模板

浏览此部分的模板时，请尝试回答以下问题：

* 在此模板中定义了多少 Azure 资源？
* 其中一个资源是 Azure 存储帐户。 该定义是否与上一教程中使用的定义类似？
* 对于此模板中定义的资源，能否找到模板参考？
* 能否找到资源的依赖项？

1. 在 Visual Studio Code 中折叠元素，直到只能在 `resources` 中看到第一级元素和第二级元素：

    :::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png" alt-text="Visual Studio Code ARM 模板":::

    有六个通过此模板定义的资源：

    * `Microsoft.Storage/storageAccounts`.
    * `Microsoft.Network/publicIPAddresses`.
    * `Microsoft.Network/networkSecurityGroups`.
    * `Microsoft.Network/virtualNetworks`.
    * `Microsoft.Network/networkInterfaces`.
    * `Microsoft.Compute/virtualMachines`.
    
     <!-- Not Available on template -->
     
     在自定义模板之前查看模板参考会很有帮助。

1. 展开第一个资源。 它是一个存储帐户。 
    
    <!-- Not Available on [template reference](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)-->

    :::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png" alt-text="Visual Studio Code ARM 模板存储帐户定义":::

1. 展开第二个资源。 资源类型为 `Microsoft.Network/publicIPAddresses`。
    
    <!-- Not Available on [template reference](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)-->

    :::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png" alt-text="Visual Studio Code ARM 模板公共 IP 地址定义":::

1. 展开第三个资源。 资源类型为 `Microsoft.Network/networkSecurityGroups`。

    <!--Not Avaiable on [template reference](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)-->

    :::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-network-security-group-definition.png" alt-text="Visual Studio Code ARM 模板网络安全组定义":::

1. 展开第四个资源。 资源类型为 `Microsoft.Network/virtualNetworks`：

    :::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-virtual-network-definition.png" alt-text="Visual Studio Code ARM 模板虚拟网络 dependsOn":::

    使用 `dependsOn` 元素可将一个资源定义为与一个或多个资源相依赖。 此资源依赖于另一个资源：

    * `Microsoft.Network/networkSecurityGroups`

1. 展开第五个资源。 资源类型为 `Microsoft.Network/networkInterfaces`。 此资源依赖于两个其他的资源：

    * `Microsoft.Network/publicIPAddresses`
    * `Microsoft.Network/virtualNetworks`

1. 展开第六个资源。 此资源为虚拟机。 它依赖于两个其他的资源：

    * `Microsoft.Storage/storageAccounts`
    * `Microsoft.Network/networkInterfaces`

下图演示了此模板的资源和依赖项信息：

:::image type="content" source="./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png" alt-text="Visual Studio Code ARM 模板依赖项图":::

指定依赖项可以让资源管理器有效地部署此解决方案。 它以并行方式部署存储帐户、公共 IP 地址和虚拟网络，因为这些没有依赖项。 部署公共 IP 地址和虚拟网络资源以后，会创建网络接口。 所有其他的资源都部署以后，资源管理器会部署虚拟机。

## <a name="deploy-the-template"></a>部署模板

<!--Not Available on Cloud Shell-->

1. 选择并验证先前在教程中保存的模板。 文件名为 azuredeploy.json。

1. 运行以下 PowerShell 脚本以部署该模板。

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. chinaeast):" &&
    read location &&
    echo "Enter the virtual machine admin username:" &&
    read adminUsername &&
    echo "Enter the DNS label prefix:" &&
    read dnsLabelPrefix &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location $location &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json" --parameters adminUsername=$adminUsername dnsLabelPrefix=$dnsLabelPrefix
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. chinaeast)"
    $adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
    $adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
    $dnsLabelPrefix = Read-Host -Prompt "Enter the DNS label prefix"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -adminUsername $adminUsername `
        -adminPassword $adminPassword `
        -dnsLabelPrefix $dnsLabelPrefix `
        -TemplateFile "$HOME/azuredeploy.json"

    Write-Host "Press [ENTER] to continue ..."
    ```

    ---

1. 通过 RDP 连接到虚拟机，验证虚拟机是否已成功创建。

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择资源组名称。 应会看到，该资源组中总共有六个资源。
4. 在顶部菜单中选择“删除资源组”。

<!--Not Avaialble on ## Next steps-->
<!--Not Avaialble on > [Use deployment script](./template-tutorial-deployment-script.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->
