---
title: 使用 Azure CLI 的 Azure 流量管理器子网替代 | Azure
description: 本文帮助你了解如何使用流量管理器子网替代来替代流量管理器配置文件的路由方法，以便通过预定义的 IP 范围到终结点的映射，基于最终用户 IP 地址将流量定向到某个终结点。
services: traffic-manager
documentationcenter: ''
ms.topic: how-to
ms.service: traffic-manager
origin.date: 09/18/2019
author: rockboyfor
ms.date: 03/29/2021
ms.testscope: yes
ms.testdate: 09/28/2020
ms.author: v-yeche
ms.openlocfilehash: 49b3443b794e324dc577795724c0b442ce3b24c8
ms.sourcegitcommit: 1a64114f25dd71acba843bd7f1cd00c4df737ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105603356"
---
<!--Verified successfully-->
# <a name="traffic-manager-subnet-override-using-azure-cli"></a>使用 Azure CLI 的流量管理器子网替代

使用流量管理器子网替代可以更改配置文件的路由方法。  添加替代后，会使用预定义的 IP 范围到终结点的映射，基于最终用户的 IP 地址来定向流量。 

## <a name="how-subnet-override-works"></a>子网替代的工作原理

将子网替代添加到流量管理器配置文件后，流量管理器会先检查最终用户的 IP 地址是否存在子网替代。 如果找到了一个替代，用户的 DNS 查询将定向到相应的终结点。  如果找不到映射，流量管理器将回退到配置文件的原始路由方法。 

可将 IP 地址范围指定为 CIDR 范围（例如 1.2.3.0/24）或地址范围（例如 1.2.3.4-5.6.7.8）。 与每个终结点关联的 IP 范围对于该终结点必须是唯一的。 不同终结点之间的 IP 范围出现任何重叠会导致流量管理器拒绝配置文件。

有两种类型的路由配置文件支持子网替代：

* **地理** - 如果流量管理器找到了 DNS 查询的 IP 地址的子网替代，它会将该查询路由到终结点，而不管该终结点的运行状况如何。
* **性能** - 如果流量管理器找到了 DNS 查询的 IP 地址的子网替代，它只会将流量路由到正常的终结点。  如果子网替代终结点不正常，流量管理器将回退到性能路由试探法。

## <a name="create-a-traffic-manager-subnet-override"></a>创建流量管理器子网替代

若要创建流量管理器子网替代，可以使用 Azure CLI 将替代子网添加到流量管理器终结点。

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- 本文需要 Azure CLI 2.0.28 或更高版本。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="update-the-traffic-manager-endpoint-with-subnet-override"></a>使用子网替代更新流量管理器终结点。
使用 Azure CLI 通过 [az network traffic-manager endpoint update](https://docs.azure.cn/cli/network/traffic-manager/endpoint#az_network_traffic_manager_endpoint_update) 更新终结点。

```azurecli
### Add a range of IPs ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 1.2.3.4-5.6.7.8 \
    --type AzureEndpoints

### Add a subnet ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 9.10.11.0:24 \
    --type AzureEndpoints
```

可以在运行 [az network traffic-manager endpoint update](https://docs.azure.cn/cli/network/traffic-manager/endpoint#az_network_traffic_manager_endpoint_update) 时使用 **--remove** 选项，以便删除 IP 地址范围。

```azurecli
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --remove subnets \
    --type AzureEndpoints
```

## <a name="next-steps"></a>后续步骤

详细了解流量管理器[流量路由方法](traffic-manager-routing-methods.md)。

了解[子网流量路由方法](./traffic-manager-routing-methods.md#subnet-traffic-routing-method)

<!--Update_Description: update meta properties, wording update, update link-->