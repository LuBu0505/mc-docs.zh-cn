---
title: Azure Stack Hub 集成系统的透明代理
description: 概述 Azure Stack Hub 集成系统中的透明属性。
author: WenJason
ms.topic: conceptual
origin.date: 01/25/2021
ms.date: 03/01/2021
ms.author: v-jay
ms.reviewer: sranthar
ms.lastreviewed: 01/25/2021
ms.openlocfilehash: b4bcb6f6595fe2dcd9769a7efe8f437fa33602c8
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751858"
---
# <a name="transparent-proxy-for-azure-stack-hub"></a>Azure Stack Hub 的透明代理

透明代理（也称为截获、内联或强制代理）会截获网络层的正常通信，无需特殊的客户端配置。 客户端不需要知道代理是否存在。

如果数据中心要求所有流量都使用代理，则请将透明代理配置为根据策略处理所有流量，方法是分离网络上不同区域之间的流量。

## <a name="traffic-types"></a>流量类型

Azure Stack Hub 的出站流量归类为租户流量或基础结构流量。

租户流量由租户通过虚拟机、负载均衡器、VPN 网关、应用服务等方式生成。

基础结构流量是从分配给基础结构服务（例如标识、修补升级、使用指标、市场联合、注册、日志收集、Windows Defender 等）的公共虚拟 IP 池的第一个 `/27` 范围生成的。来自这些服务的流量会路由到 [Azure 终结点](azure-stack-integrate-endpoints.md#ports-and-urls-outbound)。 Azure 不接受代理修改的流量或 TLS/SSL 截获流量。 这是 Azure Stack Hub 不支持原生代理配置的原因。

配置透明代理时，可以选择发送所有出站流量，或者只发送流经代理的基础结构流量。

## <a name="partner-integration"></a>合作伙伴集成

Microsoft 已与业界领先的代理供应商合作，目的是使用透明代理配置来验证 Azure Stack Hub 的用例方案。 下图是包含 HA 代理的 Azure Stack Hub 网络配置示例。 外部代理设备必须位于边界设备的“北方”（图中的上方）。

![网络图，其中显示代理置于边界设备之前](./media/azure-stack-transparent-proxy/proxy.svg)

 另外，必须将边界设备配置为通过以下方式之一从 Azure Stack Hub 路由流量：
- 将来自 Azure Stack Hub 的所有出站流量路由到代理设备
- 通过基于策略的路由将来自 Azure Stack Hub 虚拟 IP 池的第一个 `/27` 范围的所有出站流量路由到代理设备。  

有关边界配置示例，请参阅本文中的[示例边界配置](#example-border-configuration)部分。

请查看以下文档，了解 Azure Stack Hub 的已验证的透明代理配置： 

- [配置 Check Point 安全网关透明代理](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk171559)
- [配置 Sophos XG 防火墙透明代理](https://community.sophos.com/xg-firewall/f/recommended-reads/124106/xg-firewall-integration-with-azure-stack-hub)

在需要 Azure Stack Hub 出站流量流经显式代理的情况下，可以使用 Sophos 和 Checkpoint 设备提供的双模式功能。该功能允许特定范围的流量通过透明模式传递，而其他范围的流量则可以配置为通过显式模式传递。 使用此功能可以配置这些代理设备，以便使用透明代理时只发送基础结构流量，而所有租户通信则通过显式模式发送。

> [!IMPORTANT]
> SSL 流量拦截不受支持，并且在访问终结点时可能会导致服务故障。 与标识所需的终结点进行通信时，支持的最大超时值为 60 秒，并可以进行 3 次重试尝试。 有关详细信息，请参阅 [Azure Stack Hub 防火墙集成](azure-stack-firewall.md#ssl-interception)。

## <a name="example-border-configuration"></a>边界配置示例

此解决方案以基于策略的路由 (PBR) 为基础，而 PBR 则使用通过访问控制列表 (ACL) 实现的管理员定义的条件集。 ACL 会将特定流量分类，该流量定向到在路由-映射中实现的代理设备的下一跃点 IP，而不是进行仅基于目标 IP 地址的普通路由。 端口 80 和 443 的特定基础结构网络流量会从边界设备路由到透明代理部署。 透明代理会进行 URL 筛选，而“都不允许”流量则会被丢弃。

以下配置示例适用于思科 Nexus 9508 机箱。 

在此方案中，那些需要访问 Internet 的源基础结构网络如下所示：

- 公共 VIP - 第一个 /27
- 基础结构网络 - 最后一个 /27
- BMC 网络 - 最后一个 /27

以下子网在此方案中接受基于策略的路由 (PBR) 处理：

| 网络 | IP 范围 | 接受 PBR 处理的子网
|---------|----------|-------------------------------
| 公共虚拟 IP 池 | 172.21.107.0/27 的第一个 /27 | 172.21.107.0/27 = 172.21.107.1 到 172.21.107.30
| 基础结构网络 | 172.21.7.0/24 的最后一个 /27 | 172.21.7.224/27 = 172.21.7.225 到 172.21.7.254
| BMC 网络 | 10.60.32.128/26 的最后一个 /27 | 10.60.32.160/27 = 10.60.32.161 到 10.60.32.190

### <a name="configure-border-device"></a>配置边界设备

通过输入 `feature pbr` 命令启用 PBR。 

```
****************************************************************************
PBR Configuration for Cisco Nexus 9508 Chassis
PBR Enivronment configured to use VRF08
The test rack has is a 4-node Azure Stack stamp with 2x TOR switches and 1x BMC switch. Each TOR switch 
has a single uplink to the Nexus 9508 chassis using BGP for routing. In this example the test rack 
is in it's own VRF (VRF08)
****************************************************************************
!
feature pbr
!

<Create VLANs that the proxy devices will use for inside and outside connectivity>

!
VLAN 801
name PBR_Proxy_VRF08_Inside
VLAN 802
name PBR_Proxy_VRF08_Outside
!
interface vlan 801
description PBR_Proxy_VRF08_Inside
no shutdown
mtu 9216
vrf member VRF08
no ip redirects
ip address 10.60.3.1/29
!
interface vlan 802
description PBR_Proxy_VRF08_Outside
no shutdown
mtu 9216
vrf member VRF08
no ip redirects
ip address 10.60.3.33/28
!
!
ip access-list PERMITTED_TO_PROXY_ENV1
100 permit tcp 172.21.107.0/27 any eq www
110 permit tcp 172.21.107.0/27 any eq 443
120 permit tcp 172.21.7.224/27 any eq www
130 permit tcp 172.21.7.224/27 any eq 443
140 permit tcp 10.60.32.160/27 any eq www
150 permit tcp 10.60.32.160/27 any eq 443
!
!
route-map TRAFFIC_TO_PROXY_ENV1 pbr-statistics
route-map TRAFFIC_TO_PROXY_ENV1 permit 10
  match ip address PERMITTED_TO_PROXY_ENV1
  set ip next-hop 10.60.3.34 10.60.3.35
!
!
interface Ethernet1/1
  description DownLink to TOR-1:TeGig1/0/47
  mtu 9100
  logging event port link-status
  vrf member VRF08
  ip address 192.168.32.193/30
  ip policy route-map TRAFFIC_TO_PROXY_ENV1
  no shutdown
!
interface Ethernet2/1
  description DownLink to TOR-2:TeGig1/0/48
  mtu 9100
  logging event port link-status
  vrf member VRF08
  ip address 192.168.32.205/30
  ip policy route-map TRAFFIC_TO_PROXY_ENV1
  no shutdown
!

<Interface configuration for inside/outside connections to proxy devices. In this example there are 2 firewalls>

!
interface Ethernet1/41
  description management interface for Firewall-1
  switchport
  switchport access vlan 801
  no shutdown
!
interface Ethernet1/42
  description Proxy interface for Firewall-1
  switchport
  switchport access vlan 802
  no shutdown
!
interface Ethernet2/41
  description management interface for Firewall-2
  switchport
  switchport access vlan 801
  no shutdown
!
interface Ethernet2/42
  description Proxy interface for Firewall-2
  switchport
  switchport access vlan 802
  no shutdown
!

<BGP network statements for VLAN 801-802 subnets and neighbor statements for R023171A-TOR-1/R023171A-TOR-2> 

!
router bgp 65000
!
vrf VRF08
address-family ipv4 unicast
network 10.60.3.0/29
network 10.60.3.32/28
!
neighbor 192.168.32.194
  remote-as 65001
  description LinkTo 65001:R023171A-TOR-1:TeGig1/0/47
  address-family ipv4 unicast
    maximum-prefix 12000 warning-only
neighbor 192.168.32.206
  remote-as 65001
  description LinkTo 65001:R023171A-TOR-2:TeGig1/0/48
  address-family ipv4 unicast
    maximum-prefix 12000 warning-only
!
!
```

创建新的 ACL，该 ACL 将用于标识会获得 PBR 处理的流量。 该流量是测试机架中来自主机/子网的网络流量（HTTP 端口 80 和 HTTPS 端口 443），用于获取代理服务，如本示例所述。 例如，ACL 名称为 **PERMITTED_TO_PROXY_ENV1**。

```
ip access-list PERMITTED_TO_PROXY_ENV1
100 permit tcp 172.21.107.0/27 any eq www <<HTTP traffic from CL04 Public Admin VIPs leaving test rack>>
110 permit tcp 172.21.107.0/27 any eq 443 <<HTTPS traffic from CL04 Public Admin VIPs leaving test rack>>
120 permit tcp 172.21.7.224/27 any eq www <<HTTP traffic from CL04 INF-pub-adm leaving test rack>>
130 permit tcp 172.21.7.224/27 any eq 443 <<HTTPS traffic from CL04 INF-pub-adm leaving test rack>>
140 permit tcp 10.60.32.160/27 any eq www <<HTTP traffic from DVM and HLH leaving test rack>>
150 permit tcp 10.60.32.160/27 any eq 443 <<HTTPS traffic from DVM and HLH leaving test rack>>
```

PBR 功能的核心由 **TRAFFIC_TO_PROXY_ENV1** 路由-映射实现。 添加了 **pbr-statistics** 选项，这样就可以查看策略匹配统计信息，以验证获得和未获得 PBR 转发的数据包的数量。 路由-映射序列 10 允许对符合 ACL **PERMITTED_TO_PROXY_ENV1** 条件的流量进行 PBR 处理。 该流量会转发到下一跃点 IP 地址 `10.60.3.34` 和 `10.60.3.35`，这些 IP 地址是示例配置中的主要/辅助代理设备的 VIP

```
!
route-map TRAFFIC_TO_PROXY_ENV1 pbr-statistics
route-map TRAFFIC_TO_PROXY_ENV1 permit 10
  match ip address PERMITTED_TO_PROXY_ENV1
  set ip next-hop 10.60.3.34 10.60.3.35
```

ACL 用作 **TRAFFIC_TO_PROXY_ENV1** 路由-映射的匹配条件。 当流量与 **PERMITTED_TO_PROXY_ENV1** ACL 匹配时，PBR 会替代普通路由表，改将流量转发到列出的 IP 下一跃点。
 
**TRAFFIC_TO_PROXY_ENV1** PBR 策略适用于从 CL04 主机和公共 VIP 以及测试机架中的 HLH 和 DVM 进入边界设备的流量。

## <a name="next-steps"></a>后续步骤

有关防火墙集成的详细信息，请参阅 [Azure Stack Hub 防火墙集成](azure-stack-firewall.md)。
