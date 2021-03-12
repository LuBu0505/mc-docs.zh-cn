---
title: 创建具有静态专用 IP 地址的 VM - Azure PowerShell
description: 了解如何使用 PowerShell 创建具有专用 IP 地址的虚拟机。
services: virtual-network
documentationcenter: na
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/07/2019
author: rockboyfor
ms.date: 02/22/2021
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: 9dfc9b4d112fa05ba07b5b1d713143408296e899
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102108767"
---
# <a name="create-a-virtual-machine-with-a-static-private-ip-address-using-powershell"></a>使用 PowerShell 创建具有静态专用 IP 地址的虚拟机

可以创建具有静态专用 IP 地址的虚拟机 (VM)。 若要从子网中选择分配给 VM 的具体地址，请分配静态专用 IP 地址而非动态地址。 详细了解[静态专用 IP 地址](./public-ip-addresses.md#allocation-method)。 若要将分配给现有 VM 的专用 IP 地址从动态更改为静态，或者要使用公共 IP 地址，请参阅[添加、更改或删除 IP 地址](virtual-network-network-interface-addresses.md)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-virtual-machine"></a>创建虚拟机

可以从本地计算机完成以下步骤。 若要使用本地计算机，请确保[安装了 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fvirtual-network%2ftoc.json)。 

1. 打开命令会话并使用 `Connect-AzAccount -Environment AzureChinaCloud` 登录到 Azure。
   
    <!--Not Available on If using the Cloud Shell, skip to step 2. -->
    
2. 使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令创建资源组。 以下示例在“中国东部”Azure 区域中创建一个资源组：

    ```powershell
    $RgName = "myResourceGroup"
    $Location = "chinaeast"
    New-AzResourceGroup -Name $RgName -Location $Location
    ```

3. 使用 [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) 和 [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) 命令创建子网配置和虚拟网络：

    ```powershell
    # Create a subnet configuration
    $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get the subnet object for use in a later step.
    $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

4. 使用 [New-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/powershell/module/Az.Network/New-AzNetworkInterfaceIpConfig) 和 [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) 命令在虚拟网络中创建一个网络接口，并将专用 IP 地址从子网分配给该网络接口：

    ```powershell
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzNetworkInterfaceIpConfig `
     -Name $IpConfigName1 `
     -Subnet $Subnet `
     -PrivateIpAddress 10.0.0.4 `
     -Primary

    $NIC = New-AzNetworkInterface `
     -Name MyNIC `
     -ResourceGroupName $RgName `
     -Location $Location `
     -IpConfiguration $IpConfig1
    ```

5. 使用 [New-AzVMConfig](https://docs.microsoft.com/powershell/module/Az.Compute/New-AzVMConfig) 创建 VM 配置，然后使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.Compute/New-azVM) 创建 VM。 出现提示时，请提供用作 VM 登录凭据的用户名和密码：

    ```powershell
    $VirtualMachine = New-AzVMConfig -VMName MyVM -VMSize "Standard_DS3"
    $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName MyServerVM -ProvisionVMAgent -EnableAutoUpdate
    $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
    $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest
    New-AzVM -ResourceGroupName $RgName -Location $Location -VM $VirtualMachine -Verbose
    ```

> [!WARNING]
> 虽然可以向操作系统添加专用 IP 地址设置，但建议不要在阅读[向操作系统添加专用 IP 地址](virtual-network-network-interface-addresses.md#private)之前这样做。
> 
> 
> <a name = "change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>
> 
> [!IMPORTANT]
> 若要从 Internet 访问 VM，必须为该 VM 分配公共 IP 地址。 也可将动态专用 IP 地址分配更改为静态分配。 有关详细信息，请参阅[添加或更改 IP 地址](virtual-network-network-interface-addresses.md)。 另外，建议将一个网络安全组关联到网络接口和/或创建网络接口时所在的子网，限制从网络到 VM 的流量。 有关详细信息，请参阅[管理网络安全组](manage-network-security-group.md)。

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组及其包含的所有资源，请使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 将其删除：

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>后续步骤

- 详细了解[专用 IP 地址](./private-ip-addresses.md)以及如何为 Azure 虚拟机分配[静态公共 IP 地址](virtual-network-network-interface-addresses.md#add-ip-addresses)。
- 详细了解如何创建 [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fvirtual-network%2ftoc.json) 和 [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fvirtual-network%2ftoc.json) 虚拟机。

<!--Update_Description: update meta properties, wording update, update link-->