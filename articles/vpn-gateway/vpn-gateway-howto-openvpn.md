---
title: 如何在 Azure VPN 网关上配置 OpenVPN
description: 了解如何使用 PowerShell 在点对站点环境中的 Azure VPN 网关上启用 OpenVPN 协议。
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: how-to
origin.date: 02/05/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.openlocfilehash: c0390e3b0ae3c3cc1bc0893c8c5c2a60f926a628
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697286"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>为 Azure 点到站点 VPN 网关配置 OpenVPN

本文可帮助你在 Azure VPN 网关上设置 **OpenVPN® 协议**。 可以使用门户或 PowerShell 说明。

## <a name="prerequisites"></a>先决条件

* 本文假设你已拥有一个可正常工作的点到站点环境。 如果没有，请使用下列方法之一创建一个。

  * [门户 - 创建点到站点](vpn-gateway-howto-point-to-site-resource-manager-portal.md)

  * [PowerShell - 创建点到站点](vpn-gateway-howto-point-to-site-rm-ps.md)

* 验证 VPN 网关不使用基本 SKU。 OpenVPN 不支持基本 SKU。

## <a name="portal"></a>门户

1. 在门户中，导航至“虚拟网络网关”->“点到站点配置”。
1. 对于“隧道类型”，请从下拉列表中选择“OpenVPN (SSL)” 。

   :::image type="content" source="./media/vpn-gateway-howto-openvpn/portal.png" alt-text="从下拉列表中选择“OpenVPN SSL”":::
1. 保存所做的更改，然后执行后续步骤。

## <a name="enable-openvpn-on-your-gateway-using-powershell"></a>使用 PowerShell 在网关上启用 OpenVPN。

1. 使用以下示例在网关上启用 OpenVPN：

   ```azurepowershell
   $gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
   Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
   ```
1. 继续执行后续步骤。

## <a name="next-steps"></a>后续步骤

若要为 OpenVPN 配置客户端，请参阅[配置 OpenVPN 客户端](vpn-gateway-howto-openvpn-clients.md)。

**“OpenVPN”是 OpenVPN Inc. 的商标。**
