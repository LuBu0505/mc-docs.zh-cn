---
title: Azure VPN 网关：关于 P2S VPN 客户端配置文件
description: 这可帮助你使用客户端配置文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: article
origin.date: 05/13/2020
ms.date: 07/06/2020
ms.author: v-jay
ms.openlocfilehash: 8ec3a1bc5c7c333e1340439e7ad93882f3525a2e
ms.sourcegitcommit: 7ea2d04481512e185a60fa3b0f7b0761e3ed7b59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85845901"
---
# <a name="about-p2s-vpn-client-profiles"></a>关于 P2S VPN 客户端配置文件

已下载的配置文件包含配置 VPN 连接所需的信息。 本文将帮助你获取和了解 VPN 客户端配置文件所需的信息。

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* OpenVPN 文件夹包含 ovpn 配置文件，需要对该文件进行修改以包含密钥和证书。 有关详细信息，请参阅[为 Azure VPN 网关配置 OpenVPN 客户端](vpn-gateway-howto-openvpn-clients.md#windows)。 如果在 VPN 网关上选择 Azure AD 身份验证，则 zip 文件中不存在此文件夹。 而是应该导航到 AzureVPN 文件夹并找到 azurevpnconfig.xml。

## <a name="next-steps"></a>后续步骤

有关点到站点的详细信息，请参阅[关于点到站点](point-to-site-about.md)。
