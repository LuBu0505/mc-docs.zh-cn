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
ms.openlocfilehash: da7b4255aa2ec84e52f4be395992ba7d70de4f3e
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101696637"
---
添加其他地址前缀：

1. 设置 LocalNetworkGateway 的变量。

   ```azurepowershell
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
1. 修改前缀。

   ```azurepowershell
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24','10.101.2.0/24')
   ```

删除地址前缀：

  省去你不再需要的前缀。 在此示例中，我们不再需要前缀 10.101.2.0/24（来自前面的示例），因此需更新本地网关，排除该前缀。

1. 设置 LocalNetworkGateway 的变量。

   ```azurepowershell
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
1. 使用更新的前缀设置网关。

   ```azurepowershell
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```
