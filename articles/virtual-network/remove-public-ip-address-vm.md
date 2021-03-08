---
title: 将公共 IP 地址与 Azure VM 取消关联
titlesuffix: Azure Virtual Network
description: 了解如何将公共 IP 地址与 VM 取消关联
services: virtual-network
documentationcenter: ''
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/04/2019
author: rockboyfor
ms.date: 02/22/2021
ms.author: v-yeche
ms.openlocfilehash: fd3bb837d6a03d499e755cfc3a273a75321012e2
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102053973"
---
<!--Verified successfully on portal-->
# <a name="dissociate-a-public-ip-address-from-an-azure-vm"></a>将公共 IP 地址与 Azure VM 取消关联 

本文介绍如何将公共 IP 地址与 Azure 虚拟机 (VM) 取消关联。

可以使用 [Azure 门户](#azure-portal)、Azure [命令行接口](#azure-cli) (CLI) 或 [PowerShell](#powershell) 将公共 IP 地址与 VM 取消关联。

## <a name="azure-portal"></a>Azure 门户

1. 登录 [Azure 门户](https://portal.azure.cn)。
2. 浏览或搜索要将公共 IP 地址与之取消关联的虚拟机，然后将其选中。
3. 在 VM 页中选择“概述”，然后选择公共 IP 地址，如下图所示：

    :::image type="content" source="./media/remove-public-ip-address/remove-public-ip-address-2.png" alt-text="选择公共 IP":::

4. 在公共 IP 地址页中选择“概述”，然后选择“取消关联”，如下图所示：

    :::image type="content" source="./media/remove-public-ip-address/remove-public-ip-address-3.png" alt-text="取消关联公共 IP":::

5. 在“取消关联公共 IP 地址”中，选择“是”。

## <a name="azure-cli"></a>Azure CLI

安装 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?toc=%2fvirtual-network%2ftoc.json)

<!--Not Available on , or use the Azure Cloud Shell-->
<!--Not Available on  The Azure Cloud Shell is a free Bash shell that you can run directly within the Azure portal. It has the Azure CLI preinstalled and configured to use with your account. Select the **Try it** button in the CLI commands that follow. Selecting **Try it** invokes a Cloud Shell that you can sign in to your Azure account with.-->

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

1. 如果在本地使用 CLI，请使用 `az login` 登录到 Azure。

    <!--Not Available on in Bash-->
    
2. 公共 IP 地址将关联到 VM 上附加的网络接口的 IP 配置。 使用 [az network nic-ip-config update](https://docs.azure.cn/cli/network/nic/ip-config#az_network_nic_ip_config_update) 命令将公共 IP 地址与 IP 配置取消关联。 以下示例将名为 myVMPublicIP 的公共 IP 地址与名为 myVMVMNic 的现有网络接口（已连接到名为 myResourceGroup 的资源组中名为 myVM 的 VM）的名为 ipconfigmyVM 的 IP 配置取消关联。

    ```azurecli
    az network nic ip-config update \
    --name ipconfigmyVM \
    --resource-group myResourceGroup \
    --nic-name myVMVMNic \
    --remove PublicIpAddress
    ```

    如果你不知道附加到 VM 的网络接口的名称，请使用 [az vm nic list](https://docs.azure.cn/cli/vm/nic#az_vm_nic_list) 命令查看名称。 例如，以下命令会列出附加到资源组 *myResourceGroup* 中 VM *myVM* 的网络接口的名称：

    ```azurecli
    az vm nic list --vm-name myVM --resource-group myResourceGroup
    ```

    输出中包含类似于以下示例的一个或多个行：
    
    ```
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
    ```

    在以上示例中，*myVMVMNic* 是网络接口的名称。

    - 如果你不知道网络接口的 IP 配置的名称，请使用 [az network nic ip-config list](https://docs.azure.cn/cli/network/nic/ip-config#az_network_nic_ip_config_list) 命令检索名称。 例如，以下命令列出了名为 myResourceGroup 的资源组中名为 myVMVMNic 的网络接口的公共 IP 配置的名称：

        ```azurecli
        az network nic ip-config list --nic-name myVMVMNic --resource-group myResourceGroup --out table
        ```

    - 如果不知道网络接口的公共 IP 配置的名称，请使用 [az network nic ip-config show](https://docs.azure.cn/cli/network/nic/ip-config#az_network_nic_ip_config_show) 命令进行检索。 例如，以下命令列出了名为 myResourceGroup 的资源组中名为 myVMVMNic 的网络接口的公共 IP 配置的名称：

        ```azurecli
        az network nic ip-config show --name ipconfigmyVM --nic-name myVMVMNic --resource-group myResourceGroup --query publicIPAddress.id
        ```

## <a name="powershell"></a>PowerShell

安装 [PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。

<!--Not Available on , or use the Azure Cloud Shell-->
<!--Not Available on  The Azure Cloud Shell is a free Bash shell that you can run directly within the Azure portal. It has the Azure CLI preinstalled and configured to use with your account. Select the **Try it** button in the CLI commands that follow. Selecting **Try it** invokes a Cloud Shell that you can sign in to your Azure account with.-->

1. 如果在本地使用 PowerShell，请使用 `Connect-AzAccount -Environment AzureChinaCloud` 登录到 Azure。
2. 公共 IP 地址将关联到 VM 上附加的网络接口的 IP 配置。 使用 [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/Az.Network/Get-AzNetworkInterface) 命令获取网络接口。 将公共 IP 地址值设置为 null，然后使用 [Set-AzNetworkInterface](https://docs.microsoft.com/powershell/module/Az.Network/Set-AzNetworkInterface) 命令将新 IP 配置写入网络接口。

    以下示例将名为 myVMPublicIP 的公共 IP 地址与连接到名为 myVM 的 VM 的名为 myVMVMNic 的网络接口取消关联。 所有资源位于名为 *myResourceGroup* 的资源组中。

    ```azurepowershell
    $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroup myResourceGroup
    $nic.IpConfigurations.publicipaddress.id = $null
    Set-AzNetworkInterface -NetworkInterface $nic
    ```

    - 如果你不知道附加到 VM 的网络接口的名称，请使用 [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM) 命令查看名称。 例如，以下命令会列出附加到资源组 *myResourceGroup* 中 VM *myVM* 的网络接口的名称：

        ```azurepowershell
        $vm = Get-AzVM -name myVM -ResourceGroupName myResourceGroup
        $vm.NetworkProfile
        ```

         输出中包含类似于以下示例的一个或多个行。 在示例输出中，*myVMVMNic* 是网络接口的名称。

         ```
         "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
         ```

    - 如果你不知道网络接口的 IP 配置的名称，请使用 [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/Az.Network/Get-AzNetworkInterface) 命令检索名称。 例如，以下命令会列出资源组 *myResourceGroup* 中网络接口 *myVMVMNic* 的 IP 配置的名称：

        ```powershell
        $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
        $nic.IPConfigurations.id
        ```

        输出中包含类似于以下示例的一个或多个行。 在示例输出中，*ipconfigmyVM* 是 IP 配置的名称。

        ```
        "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM"
        ```

## <a name="next-steps"></a>后续步骤

- 了解如何[将公共 IP 地址关联到 VM](associate-public-ip-address-vm.md)。

<!--Update_Description: update meta properties, wording update, update link-->