---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/10/2020
title: 安全群集连接 - Azure Databricks
description: 了解安全群集连接，该连接不在客户 VPC 上提供任何开放端口，其提供的 Databricks Runtime 工作器没有公共 IP 地址。
ms.openlocfilehash: fa32ac22b2317cde8911b564079d2b614719404e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060748"
---
# <a name="secure-cluster-connectivity"></a>安全群集连接

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../release-notes/release-types.md)提供。

启用安全群集连接后，客户虚拟网络没有开放的端口，并且 Databricks Runtime 群集节点没有公共 IP 地址。

* 在网络级别，每个群集在群集创建过程中会启动到控制平面安全群集连接中继（代理）的连接。 群集使用端口 443 (HTTPS) 和一个与 Web 应用程序和 REST API 所用 IP 地址不同的 IP 地址来建立此连接。
* 控制平面按逻辑启动的操作（例如启动新的 Databricks Runtime 作业或执行群集管理）将作为请求通过此反向隧道发送到群集。
* 数据平面 (VNet) 没有开放的端口，Databricks Runtime 群集节点没有公共 IP 地址。

> [!NOTE]
>
> 无论是否启用了安全群集连接，数据平面 VNet 与 Azure Databricks 控制平面之间的所有 Azure Databricks 网络流量均跨 Microsoft 网络主干而不是公共 Internet。

优点：

* 简单的网络管理 - 降低了复杂性，因为无需在安全组上进行端口配置，也无需配置网络对等互连。
* 更轻松的审批 - 归功于更好的安全性和更简单的网络管理，信息安全团队可以更轻松地将 Databricks 批准为 PaaS 提供程序。

> [!div class="mx-imgBorder"]
> ![安全群集连接](../_static/images/security/secure-cluster-connectivity-azure.png)

## <a name="using-secure-cluster-connectivity"></a>使用安全群集连接

若要将安全群集连接与 Azure Databricks 工作区结合使用，请在创建新工作区的 ARM 模板中为 ``Microsoft.Databricks/workspaces`` 资源添加值为 ``true`` 的布尔参数 ``enableNoPublicIp``。

> [!NOTE]
>
> 安全群集连接仅适用于新工作区。 如果你要迁移具有公共 IP 的工作区，则应创建允许进行安全群集连接的新工作区，并将资源迁移到新工作区。 有关详细信息，请联系你的 Microsoft 或 Databricks 帐户团队。

根据你是想让 Azure Databricks 为工作区创建默认虚拟网络，还是想要使用你自己的虚拟网络（也称为 [VNet 注入](../administration-guide/cloud-configurations/azure/vnet-inject.md)），将参数添加到以下模板之一。 VNet 注入是一项可选功能，允许你提供自己的 VNet 来承载新的 Azure Databricks 群集。

* [使用默认虚拟网络设置工作区的 ARM 模板](https://docs.microsoft.com/azure/databricks/scenarios/quickstart-create-databricks-workspace-resource-manager-template)。
* [使用 VNet 注入设置工作区的 ARM 模板](../administration-guide/cloud-configurations/azure/vnet-inject.md#advanced-configuration-using-azure-resource-manager-templates)。

### <a name="egress-from-workspace-subnets"></a>工作区子网的流出量

启用安全群集连接时，两个工作区子网都是专用子网，因为群集节点没有公共 IP 地址。

对于使用由 Azure Databricks 创建的默认虚拟网络的部署，发往公共网络的任何出站流量都使用 Azure 提供的默认源网络地址转换 (SNAT) 公共 IP。

如果你使用 [VNet 注入](../administration-guide/cloud-configurations/azure/vnet-inject.md)，则可能会应用相同的默认 SNAT 公共 IP。 但是，如果你使用 VNet 注入，则 Databricks 强烈建议你配置 [Azure NAT 网关](https://docs.microsoft.com/azure/virtual-network/nat-overview)、[Azure 防火墙](https://docs.microsoft.com/azure/firewall/overview)或你自己的防火墙设备。 这些解决方案确保你的工作区具有稳定的 SNAT 公共 IP。

如果你将 VNet 注入与 Azure NAT 网关配合使用，请在两个工作区子网中都配置该网关，以确保所有出站公共流量都通过该网关进行传输。

如果你将 VNet 注入与出口防火墙或其他自定义网络体系结构配合使用，则可以使用自定义路由，也称为用户定义的路由 (UDR)。 UDR 可确保正确路由你的工作区的网络流量。 如果你决定使用 UDR，则必须为安全群集连接中继添加 UDR。 若要了解你的部署区域中的安全群集连接中继，请参阅 [Azure Databricks 的用户定义的路由设置](../administration-guide/cloud-configurations/azure/udr.md)。