---
title: Azure 防火墙主动 FTP 支持
description: 默认情况下，Azure 防火墙上的主动 FTP 处于禁用状态。 可以使用 PowerShell、CLI 和 ARM 模板启用主动 FTP。
services: firewall
ms.service: firewall
ms.topic: conceptual
author: rockboyfor
origin.date: 01/22/2021
ms.date: 02/01/2021
ms.author: v-harrishe
ms.openlocfilehash: f1289478e87d8e55d034a326e39302ba4dfdeae0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061043"
---
<!--Verified Successfully-->
# <a name="azure-firewall-active-ftp-support"></a>Azure 防火墙主动 FTP 支持

借助主动 FTP，FTP 服务器可启动与指定 FTP 客户端数据端口的数据连接。 客户端网络上的防火墙通常会阻止与内部客户端端口的外部连接请求。 有关详细信息，请参阅[主动 FTP 和被动 FTP 的明确说明](https://slacksite.com/other/ftp.html)。

默认情况下，在 Azure 防火墙上禁用主动 FTP 支持，防范使用 FTP `PORT` 命令进行的 FTP 弹跳攻击。 但是，可以在使用 Azure PowerShell、Azure CLI 或 Azure ARM 模板进行部署时启用主动 FTP。

> [!NOTE]
> 目前，只有虚拟网络中部署的防火墙才支持主动 FTP。 后续将添加虚拟 WAN 支持。

## <a name="azure-powershell"></a>Azure PowerShell

若要使用 Azure PowerShell 进行部署，请使用 `AllowActiveFTP` 参数。 有关详细信息，请参阅[创建包含允许主动 FTP 的防火墙](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-5.4.0#16---create-a-firewall-with-allow-active-ftp-)。

## <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 进行部署，请使用 `--allow-active-ftp` 参数。 有关详细信息，请参阅 [az network firewall create](https://docs.azure.cn/cli/ext/azure-firewall/network/firewall#ext_azure_firewall_az_network_firewall_create_optional-parameters)。 

## <a name="azure-resource-manager-arm-template"></a>Azure 资源管理器 (ARM) 模板

若要使用 ARM 模板进行部署，请使用 `AdditionalProperties` 字段：

```json
"additionalProperties": {
            "Network.FTP.AllowActiveFTP": "True"
        },
```

<!--Not Available on For more information, see [Microsoft.Network azureFirewalls](https://docs.microsoft.com/azure/templates/microsoft.network/azurefirewalls)-->

## <a name="next-steps"></a>后续步骤

若要了解如何部署 Azure 防火墙，请参阅[使用 Azure PowerShell 部署和配置 Azure 防火墙](deploy-ps.md)。

<!-- Update_Description: new article about active ftp support -->
<!--NEW.date: 02/01/2021-->