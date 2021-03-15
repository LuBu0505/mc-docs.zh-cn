---
title: 查看 BGP 状态和指标
titleSuffix: Azure VPN Gateway
description: 查看与 BGP 相关的重要信息以进行故障排除。
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.devlang: powershell
ms.topic: sample
origin.date: 02/22/2021
ms.date: 03/15/2021
ms.author: v-jay
ms.openlocfilehash: 76f9ae24badd668bdaed2baac1527756ef2a96c4
ms.sourcegitcommit: ec127596b5c56f8ba4d452c39a7b44510b140ed4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103212769"
---
# <a name="view-bgp-metrics-and-status-using-powershell"></a>使用 PowerShell 查看 BGP 指标和状态

使用 Get-AzVirtualNetworkGatewayBGPPeerStatus 查看所有 BGP 对等体和状态

[!INCLUDE [VPN Gateway PowerShell instructions](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

```azurepowershell
Get-AzVirtualNetworkGatewayBgpPeerStatus -ResourceGroupName resourceGroup -VirtualNetworkGatewayName gatewayName

Asn               : 65515
ConnectedDuration : 9.01:04:53.5768637
LocalAddress      : 10.1.0.254
MessagesReceived  : 14893
MessagesSent      : 14900
Neighbor          : 10.0.0.254
RoutesReceived    : 1
State             : Connected
```

使用 Get-AzVirtualNetworkGatewayLearnedRoute 查看网关通过 BGP 获悉的所有路由。

```azurepowershell
Get-AzVirtualNetworkGatewayLearnedRoute -ResourceGroupName resourceGroup -VirtualNetworkGatewayname gatewayName

AsPath       :
LocalAddress : 10.1.0.254
Network      : 10.1.0.0/16
NextHop      :
Origin       : Network
SourcePeer   : 10.1.0.254
Weight       : 32768

AsPath       :
LocalAddress : 10.1.0.254
Network      : 10.0.0.254/32
NextHop      :
Origin       : Network
SourcePeer   : 10.1.0.254
Weight       : 32768

AsPath       : 65515
LocalAddress : 10.1.0.254
Network      : 10.0.0.0/16
NextHop      : 10.0.0.254
Origin       : EBgp
SourcePeer   : 10.0.0.254
Weight       : 32768
```

使用 Get-AzVirtualNetworkGatewayAdvertisedRoute 查看网关通过 BGP 播发到其对等体的所有路由。

```azurepowershell
Get-AzVirtualNetworkGatewayAdvertisedRoute -VirtualNetworkGatewayName gatewayName -ResourceGroupName resourceGroupName -Peer 10.0.0.254
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要所创建的资源，请使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令删除资源组。 该命令将删除资源组及其包含的所有资源。

```azurepowershell
Remove-AzResourceGroup -Name ResourceGroupName
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/)。
