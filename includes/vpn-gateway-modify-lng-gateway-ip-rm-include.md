---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 02/10/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 2005075677acbcd40889198ca47056420f920bf9
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101696636"
---
如果要连接的 VPN 设备已更改其公共 IP 地址，则需根据该更改修改本地网关。 修改此值时，还可同时修改地址前缀。 请务必使用本地网关的现有名称来覆盖当前设置。 如果使用其他名称，请创建一个新的本地网关，而不是覆盖现有的。

```azurepowershell
New-AzLocalNetworkGateway -Name Site1 `
-Location "China North" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```
