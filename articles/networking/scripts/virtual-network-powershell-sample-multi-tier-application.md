---
title: Azure PowerShell 脚本示例 - 为多层应用程序创建网络
description: Azure PowerShell 脚本示例 - 为多层应用程序创建虚拟网络。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 02/01/2021
ms.author: v-tawe
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 32cdba67dbcb54f0e2260bd53df123daa14494ab
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060072"
---
# <a name="create-a-network-for-multi-tier-applications"></a>为多层应用程序创建网络

该脚本示例创建了包含前端和后端子网的虚拟网络。 传入前端子网的流量仅限 HTTP 和 SSH，而传入后端子网的流量限于 MySQL、端口 3306。 运行该脚本后，将具有两个虚拟机（在可向其中部署 Web 服务器和 MySQL 软件的每个子网中各具有一个虚拟机）。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
# Variables for common values
$rgName='MyResourceGroup'
$location='chinaeast'

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group.
New-AzResourceGroup -Name $rgName -Location $location

# Create a virtual network with a front-end subnet and back-end subnet.
$fesubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-FrontEnd' -AddressPrefix '10.0.1.0/24'
$besubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-BackEnd' -AddressPrefix '10.0.2.0/24'
$vnet = New-AzVirtualNetwork -ResourceGroupName $rgName -Name 'MyVnet' -AddressPrefix '10.0.0.0/16' `
  -Location $location -Subnet $fesubnet, $besubnet

# Create an NSG rule to allow HTTP traffic in from the Internet to the front-end subnet.
$rule1 = New-AzNetworkSecurityRuleConfig -Name 'Allow-HTTP-All' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 80

# Create an NSG rule to allow RDP traffic from the Internet to the front-end subnet.
$rule2 = New-AzNetworkSecurityRuleConfig -Name 'Allow-RDP-All' -Description "Allow RDP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 3389


# Create a network security group for the front-end subnet.
$nsgfe = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location `
  -Name 'MyNsg-FrontEnd' -SecurityRules $rule1,$rule2

# Associate the front-end NSG to the front-end subnet.
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' `
  -AddressPrefix '10.0.1.0/24' -NetworkSecurityGroup $nsgfe

# Create an NSG rule to allow SQL traffic from the front-end subnet to the back-end subnet.
$rule1 = New-AzNetworkSecurityRuleConfig -Name 'Allow-SQL-FrontEnd' -Description "Allow SQL" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
  -SourceAddressPrefix '10.0.1.0/24' -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 1433

# Create an NSG rule to allow RDP traffic from the Internet to the back-end subnet.
$rule2 = New-AzNetworkSecurityRuleConfig -Name 'Allow-RDP-All' -Description "Allow RDP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 3389

# Create a network security group for back-end subnet.
$nsgbe = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location `
  -Name "MyNsg-BackEnd" -SecurityRules $rule1,$rule2

# Associate the back-end NSG to the back-end subnet
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-BackEnd' `
  -AddressPrefix '10.0.2.0/24' -NetworkSecurityGroup $nsgbe

# Create a public IP address for the web server VM.
$publicipvm1 = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Web' `
  -location $location -AllocationMethod Dynamic

# Create a NIC for the web server VM.
$nicVMweb = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic-Web' -PublicIpAddress $publicipvm1 -NetworkSecurityGroup $nsgfe -Subnet $vnet.Subnets[0]

# Create a Web Server VM in the front-end subnet
$vmConfig = New-AzVMConfig -VMName 'MyVm-Web' -VMSize 'Standard_DS2' | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Web' -Credential $cred | `
  Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' `
  -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMweb.Id

$vmweb = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# Create a public IP address for the SQL VM.
$publicipvm2 = New-AzPublicIpAddress -ResourceGroupName $rgName -Name MyPublicIP-Sql `
  -location $location -AllocationMethod Dynamic

# Create a NIC for the SQL VM.
$nicVMsql = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name MyNic-Sql -PublicIpAddress $publicipvm2 -NetworkSecurityGroup $nsgbe -Subnet $vnet.Subnets[1] 

# Create a SQL VM in the back-end subnet.
$vmConfig = New-AzVMConfig -VMName 'MyVm-Sql' -VMSize 'Standard_DS2' | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Sql' -Credential $cred | `
  Set-AzureRmVMSourceImage -PublisherName 'MicrosoftSQLServer' -Offer 'SQL2016-WS2016' `
  -Skus 'Web' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMsql.Id

$vmsql = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# Create an NSG rule to block all outbound traffic from the back-end subnet to the Internet (must be done after VM creation)
$rule3 = New-AzNetworkSecurityRuleConfig -Name 'Deny-Internet-All' -Description "Deny Internet All" `
  -Access Deny -Protocol Tcp -Direction Outbound -Priority 300 `
  -SourceAddressPrefix * -SourcePortRange * `
  -DestinationAddressPrefix Internet -DestinationPortRange *

# Add NSG rule to Back-end NSG
$nsgbe.SecurityRules.add($rule3)

Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsgbe
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) | 创建 Azure 虚拟网络和前端子网。 |
| [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | 创建后端子网。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) | 创建虚拟网络接口，并将其附加到虚拟网络的前端和后端子网。 |
| [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) | 创建关联到前端和后端子网的网络安全组 (NSG)。 |
| [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig) |创建 NSG 规则，允许或阻止特定子网的特定端口。 |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | 创建虚拟机，并将 NIC 附加到每个 VM。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
