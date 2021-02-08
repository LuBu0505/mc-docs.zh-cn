---
title: Azure 网络服务概述
description: 了解 Azure 中的网络服务，包括连接、应用程序保护、应用程序交付和网络监视服务。
services: networking
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
origin.date: 10/28/2020
ms.date: 02/01/2021
ms.author: v-tawe
ms.openlocfilehash: a71826748481433b2072c3527a3c47da6d7c263e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059750"
---
# <a name="azure-networking-services-overview"></a>Azure 网络服务概述

<!-- Peering service, Azure Bastion, Virtual network NAT Gateway， Private Link, DDoS protection, Azure Front Door Service, Internet Analyzer, VNet Terminal Access Point (TAP) -->

Azure 中的网络服务提供可以搭配使用或单独使用的各种网络功能。 请单击以下任一重要功能了解更多相关信息：
- [**连接服务**](#connect)：使用 Azure 中的以下任一网络服务或其组合连接 Azure 资源和本地资源 - 虚拟网络 (VNet)、虚拟 WAN、ExpressRoute、VPN 网关和 Azure DNS。
- [**应用程序保护服务**](#protect)：使用 Azure 中的以下任一网络服务或其组合来保护应用程序：防火墙、网络安全组、Web 应用程序防火墙和虚拟网络终结点。
- [**应用程序分发服务**](#deliver)：使用 Azure 中的以下任一网络服务或其组合在 Azure 网络中分发应用程序：内容分发网络 (CDN)、流量管理器、应用程序网关和负载均衡器。
- [**网络监视**](#monitor)：使用 Azure 中的以下任一网络服务或其组合来监视网络资源：网络观察程序、ExpressRoute Monitor 和 Azure Monitor。

## <a name="connectivity-services"></a><a name="connect"></a>连接服务
 
本部分介绍用于在 Azure 资源之间提供连接、建立从本地网络到 Azure 资源的连接，以及在 Azure 中建立分支到分支连接的服务 - 虚拟网络 (VNet)、虚拟 WAN、ExpressRoute、VPN 网关和 Azure DNS


### <a name="virtual-network"></a><a name="vnet"></a>虚拟网络

Azure 虚拟网络 (VNet) 是 Azure 中专用网络的基本构建块。 使用 VNet 可以：
- **在 Azure 资源之间通信**：可以将 VM 和多个其他类型的 Azure 资源部署到虚拟网络，如 Azure 应用服务环境、Azure Kubernetes 服务 (AKS) 和 Azure 虚拟机规模集。 若要查看可部署到虚拟网络的 Azure 资源的完整列表，请参阅[虚拟网络服务集成](../virtual-network/virtual-network-for-azure-services.md)。
- **相互通信**：可以互相连接虚拟网络，使虚拟网络中的资源能够通过虚拟网络对等互连相互进行通信。 连接的虚拟网络可以在相同或不同的 Azure 区域中。 有关详细信息，请参阅[虚拟网络对等互连](../virtual-network/virtual-network-peering-overview.md)。
- **与 Internet 通信**：默认情况下，VNet 中的所有资源都可以与 Internet 进行出站通信。 可以通过分配公共 IP 地址或公共负载均衡器来与资源进行入站通信。 还可以使用[公共 IP 地址](../virtual-network/virtual-network-public-ip-address.md)或公共[负载均衡器](../load-balancer/load-balancer-overview.md)来管理出站连接。
- **与本地网络通信**：可以使用 [VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)或 [ExpressRoute](../expressroute/expressroute-introduction.md) 将本地计算机和网络连接到虚拟网络。

有关详细信息，请参阅[什么是 Azure 虚拟网络？](../virtual-network/virtual-networks-overview.md)

### <a name="expressroute"></a><a name="expressroute"></a>ExpressRoute
使用 ExpressRoute 可通过连接服务提供商所提供的专用连接，将本地网络扩展到 Microsoft 云。 此连接是专用连接。 流量不经过 Internet。 使用 ExpressRoute 可与 Microsoft Azure、Office 365 和 Dynamics 365 等 Microsoft 云服务建立连接。  有关详细信息，请参阅[什么是 ExpressRoute？](../expressroute/expressroute-introduction.md)

:::image type="content" source="./media/networking-overview/expressroute-connection-overview.png" alt-text="Azure ExpressRoute" border="false":::

### <a name="vpn-gateway"></a><a name="vpngateway"></a>VPN 网关
VPN 网关可帮助你创建从本地位置到虚拟网络的加密跨界连接，或者在 VNet 之间创建加密连接。 VPN 网关连接可以使用不同的配置，例如站点到站点连接、点到站点连接，或 VNet 到 VNet 连接。
下图演示了与同一虚拟网络建立的多个站点到站点 VPN 连接。

:::image type="content" source="./media/networking-overview/vpngateway-multisite-connection-diagram.png" alt-text="站点到站点 Azure VPN 网关连接":::

有关不同类型的 VPN 连接的详细信息，请参阅 [VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)。

### <a name="virtual-wan"></a><a name="virtualwan"></a>虚拟 WAN
Azure Virtual WAN 是一种网络服务，提供到 Azure 并穿过该服务的经优化的自动分支连接。 Azure 区域充当可以选择将分支连接到的中心。 利用 Azure 主干网还可以连接分支并享用分支到 VNet 的连接。 Azure 虚拟 WAN 将许多 Azure 云连接服务（例如，站点到站点 VPN、ExpressRoute、点到站点用户 VPN）汇集到一个操作界面中。 通过使用虚拟网络连接建立与 Azure VNet 的连接。 有关详细信息，请参阅[什么是 Azure 虚拟 WAN？](../virtual-wan/virtual-wan-about.md)。

:::image type="content" source="./media/networking-overview/virtualwan1.png" alt-text="虚拟 WAN 示意图":::

### <a name="azure-dns"></a><a name="dns"></a>Azure DNS
Azure DNS 是 DNS 域的托管服务，它使用 Azure 基础结构提供名称解析。 通过在 Azure 中托管域，可以使用与其他 Azure 服务相同的凭据、API、工具和计费来管理 DNS 记录。 有关详细信息，请参阅[什么是 Azure DNS？](../dns/dns-overview.md)

<!--
### <a name="bastion"></a>Azure Bastion
The Azure Bastion service is a new fully platform-managed PaaS service that you provision inside your virtual network. It provides secure and seamless RDP/SSH connectivity to your virtual machines directly in the Azure portal over TLS. When you connect via Azure Bastion, your virtual machines do not need a public IP address. For more information, see [What is Azure Bastion?](../bastion/bastion-overview.md).

:::image type="content" source="./media/networking-overview/architecture.png" alt-text="Azure Bastion architecture":::

### <a name="nat"></a>Virtual network NAT Gateway
Virtual Network NAT (network address translation) simplifies outbound-only Internet connectivity for virtual networks. When configured on a subnet, all outbound connectivity uses your specified static public IP addresses. Outbound connectivity is possible without load balancer or public IP addresses directly attached to virtual machines. 
For more information, see [What is virtual network NAT gateway?](../virtual-network/nat-overview.md).

:::image type="content" source="./media/networking-overview/flow-map.png" alt-text="Virtual network NAT gateway":::

### <a name="azurepeeringservice"></a> Azure Peering Service
Azure Peering service enhances customer connectivity to Microsoft cloud services such as Office 365, Dynamics 365, software as a service (SaaS) services, Azure, or any Microsoft services accessible via the public internet. For more information, see [What is Azure Peering Service?](../peering-service/about.md).

### <a name="edge-zones"></a>Azure Edge Zones

Azure Edge Zone is a family of offerings from Microsoft Azure that enables data processing close to the user. You can deploy VMs, containers, and other selected Azure services into Edge Zones to address the low latency and high throughput requirements of applications. For more information, see [What is Azure Edge Zones?](edge-zones-overview.md).

### <a name="orbital"></a>Azure Orbital

Azure Orbital is a fully managed cloud-based ground station as a service that lets you communicate with your spacecraft or satellite constellations, downlink and uplink data, process your data in the cloud, chain services with Azure services in unique scenarios, and generate products for your customers. This system is built on top of the Azure global infrastructure and low-latency global fiber network. For more information, see [What is Azure Orbital?](azure-orbital-overview.md).

:::image type="content" source="./media/azure-orbital-overview/orbital-communications-use-flow.png" alt-text="Azure Orbital communications diagram":::
-->
## <a name="application-protection-services"></a><a name="protect"></a>应用程序保护服务

本部分介绍 Azure 中帮助保护网络资源的网络服务：使用 Azure 中以下网络服务的任意组合来保护应用程序 - 防火墙、网络安全组、Web 应用程序防火墙和虚拟网络终结点。

<!--### <a name="ddosprotection"></a>DDoS Protection -->

<!--### <a name="privatelink"></a>Azure Private Link-->

### <a name="azure-firewall"></a><a name="firewall"></a>Azure 防火墙
Azure 防火墙是托管的基于云的网络安全服务，可保护 Azure 虚拟网络资源。 使用 Azure 防火墙可以跨订阅和虚拟网络集中创建、实施和记录应用程序与网络连接策略。 Azure 防火墙对虚拟网络资源使用静态公共 IP 地址，使外部防火墙能够识别来自你的虚拟网络的流量。 

有关 Azure 防火墙的详细信息，请参阅 [Azure 防火墙文档](../firewall/overview.md)。

:::image type="content" source="./media/networking-overview/firewall-threat.png" alt-text="防火墙概述":::

<!--### <a name="waf"></a>Web Application Firewall-->

### <a name="network-security-groups"></a><a name="nsg"></a>网络安全组
可以使用网络安全组来筛选 Azure 虚拟网络中出入 Azure 资源的网络流量。 有关详细信息，请参阅[网络安全组](../virtual-network/network-security-groups-overview.md)。

### <a name="service-endpoints"></a><a name="serviceendpoints"></a>服务终结点
虚拟网络 (VNet) 服务终结点可通过直接连接将 VNet 的虚拟网络专用地址空间和标识扩展到 Azure 服务。 使用终结点可以保护关键的 Azure 服务资源，只允许在客户自己的虚拟网络中对其进行访问。 从 VNet 发往 Azure 服务的流量始终保留在 Azure 主干网络中。 有关详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)。

:::image type="content" source="./media/networking-overview/vnet-service-endpoints-overview.png" alt-text="虚拟网络服务终结点":::

## <a name="application-delivery-services"></a><a name="deliver"></a>应用程序分发服务

本部分介绍 Azure 中帮助分发应用程序的网络服务 - 网络观察程序、ExpressRoute Monitor 和 Azure Monitor。

<!--Not Available on see [Azure Content Delivery Network](../cdn/cdn-overview.md)-->

:::image type="content" source="./media/networking-overview/cdn-overview.png" alt-text="Azure CDN":::

<!-- ### <a name="frontdoor"></a>Azure Front Door service-->

### <a name="traffic-manager"></a><a name="trafficmanager"></a>流量管理器

Azure 流量管理器是一种基于 DNS 的流量负载均衡器，可以在 Azure 区域内以最佳方式向服务分发流量，同时提供高可用性和响应度。 流量管理器提供一系列流量路由方法来分发流量，例如优先级、加权、性能、地理位置、多值或子网路由方法。 有关流量路由方法的详细信息，请参阅[流量管理器路由方法](../traffic-manager/traffic-manager-routing-methods.md)。

下图演示了流量管理器的基于终结点优先级的路由方法：

:::image type="content" source="./media/networking-overview/priority.png" alt-text="Azure 流量管理器的“优先级”流量路由方法":::

有关流量管理器的详细信息，请参阅[什么是 Azure 流量管理器？](../traffic-manager/traffic-manager-overview.md)

### <a name="load-balancer"></a><a name="loadbalancer"></a>负载均衡器
Azure 负载均衡器为所有 UDP 和 TCP 协议提供高性能、低延迟的第 4 层负载均衡。 它管理入站和出站连接。 可以配置公共和内部负载均衡终结点。 可以定义规则，以便将入站连接映射到后端池目标，并在其中包含 TCP 和 HTTP 运行状况探测选项来管理服务的可用性。 若要了解有关负载均衡器的详细信息，请参阅[负载均衡器概述](../load-balancer/load-balancer-overview.md)一文。

下图显示了利用外部和内部负载均衡器的面向 Internet 的多层应用程序：

:::image type="content" source="./media/networking-overview/load-balancer.png" alt-text="Azure 负载均衡器示例":::

### <a name="application-gateway"></a><a name="applicationgateway"></a>应用程序网关
Azure 应用程序网关是一种 Web 流量负载均衡器，可用于管理 Web 应用程序的流量。 它是服务形式的应用程序传送控制器 (ADC)，借此为应用程序提供各种第 7 层负载均衡功能。 有关详细信息，请参阅[什么是 Azure 应用程序网关？](../application-gateway/overview.md)

下图演示了应用程序网关的基于 URL 路径的路由方法。

:::image type="content" source="./media/networking-overview/figure1-720.png" alt-text="应用程序网关示例":::

## <a name="network-monitoring-services"></a><a name="monitor"></a>网络监视服务
本部分介绍 Azure 中可帮助监视网络资源的网络服务 - 网络观察程序、ExpressRoute Monitor、Azure Monitor 和虚拟网络 TAP。

### <a name="network-watcher"></a><a name="networkwatcher"></a>网络观察程序
Azure 网络观察程序提供所需的工具用于监视、诊断 Azure 虚拟网络中的资源、查看其指标，以及为其启用或禁用日志。 有关详细信息，请参阅[什么是网络观察程序？](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fnetworking%2ftoc.json)

### <a name="azure-monitor-for-networks-preview"></a>Azure 网络监视器预览版
Azure 网络监视器为已部署的所有网络资源提供运行状况和指标的全面视图，并且无需任何配置。 它还提供对网络监视功能的访问，如[连接监视器](../network-watcher/connection-monitor-overview.md)、[网络安全组的流日志记录](../network-watcher/network-watcher-nsg-flow-logging-overview.md)和[流量分析](../network-watcher/traffic-analytics.md)。 有关详细信息，请参阅 [Azure 网络监视器预览版](../azure-monitor/insights/network-insights-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)。

### <a name="expressroute-monitor"></a><a name="expressroutemonitor"></a>ExpressRoute Monitor
若要了解如何查看 ExpressRoute 线路指标、资源日志和警报，请参阅 [ExpressRoute 监视、指标和警报](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fnetworking%2ftoc.json)。
### <a name="azure-monitor"></a><a name="azuremonitor"></a>Azure Monitor
Azure Monitor 提供用于收集、分析和处理来自云与本地环境的遥测数据的综合解决方案，可将应用程序的可用性和性能最大化。 它可以帮助你了解应用程序的性能，并主动识别影响应用程序及其所依赖资源的问题。 有关详细信息，请参阅 [Azure Monitor 概述](../azure-monitor/overview.md?toc=%2fnetworking%2ftoc.json)。

<!--
### <a name="vnettap"></a>Virtual Network TAP
Azure virtual network TAP (Terminal Access Point) allows you to continuously stream your virtual machine network traffic to a network packet collector or analytics tool. The collector or analytics tool is provided by a [network virtual appliance](https://azure.microsoft.com/solutions/network-appliances/) partner.

The following image shows how virtual network TAP works:

:::image type="content" source="./media/networking-overview/virtual-network-tap-architecture.png" alt-text="How virtual network TAP works":::

For more information, see [What is Virtual Network TAP](../virtual-network/virtual-network-tap-overview.md).
-->

## <a name="next-steps"></a>后续步骤

- 完成[创建首个虚拟网络](../virtual-network/quick-create-portal.md?toc=%2fnetworking%2ftoc.json)一文中的步骤，创建自己的首个虚拟网络，并将几个 VM 连接到此网络。
- 完成[配置点到站点连接](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fnetworking%2ftoc.json)一文中的步骤，将计算机连接到虚拟网络。
- 完成[创建面向 Internet 的负载均衡器](../load-balancer/quickstart-load-balancer-standard-public-portal.md?toc=%2fnetworking%2ftoc.json)一文中的步骤，对发往公共服务器的 Internet 流量进行负载均衡。
