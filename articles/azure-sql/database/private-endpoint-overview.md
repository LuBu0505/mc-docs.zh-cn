---
title: Azure 专用链接
description: 专用终结点功能概述。
author: WenJason
ms.author: v-jay
titleSuffix: Azure SQL Database and Azure Synapse Analytics
ms.service: sql-database
ms.topic: overview
ms.custom: sqldbrb=1
ms.reviewer: vanto
origin.date: 03/09/2020
ms.date: 10/29/2020
ms.openlocfilehash: a0b1303cadc7aad03ec6f3aa5bfb8b783ab81678
ms.sourcegitcommit: cf3d8d87096ae96388fe273551216b1cb7bf92c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/31/2020
ms.locfileid: "97830269"
---
# <a name="azure-private-link-for-azure-sql-database-and-azure-synapse-analytics"></a>Azure SQL 数据库和 Azure Synapse Analytics 的 Azure 专用链接
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

使用专用链接可以通过 **专用终结点** 连接到 Azure 中的各种 PaaS 服务。 专用终结点是特定[ VNet ](../../virtual-network/virtual-networks-overview.md)和子网中的专用 IP 地址。

> [!IMPORTANT]
> 本文适用于 Azure SQL 数据库和 Azure Synapse Analytics（以前的 SQL 数据仓库）。 为简单起见，术语“数据库”是指 Azure SQL 数据库中的数据库和 Azure Synapse Analytic 中的数据库。 同样，无论何时提及“服务器”，都是指托管 Azure SQL 数据库和 Azure Synapse Analytics[ 的逻辑 SQL Server](logical-servers.md)。 本文不适用于 **Azure SQL 托管实例**。

## <a name="how-to-set-up-private-link-for-azure-sql-database"></a>如何设置 Azure SQL 数据库的专用链接 

### <a name="approval-process"></a>审批过程
网络管理员创建专用终结点 (PE) 后，SQL 管理员可以管理与 SQL 数据库建立的专用终结点连接 (PEC)。

1. 按照下面的屏幕截图中所示的步骤，导航到 Azure 门户中的服务器资源

    - (1) 在左窗格中选择“专用终结点连接”
    - (2) 显示所有专用终结点连接 (PEC) 的列表
    - (3) 创建的相应专用终结点 (PE) ![所有 PEC 的屏幕截图][3]

1. 在列表中选择单个 PEC。
![选定 PEC 的屏幕截图][6]

1. SQL 管理员可以选择批准或拒绝 PEC，并可以选择性地添加简短的文本回复。
![PEC 审批屏幕截图][4]

1. 批准或拒绝后，该列表将反映相应的状态以及回复文本。
![审批后的所有 PEC 的屏幕截图][5]

## <a name="on-premises-connectivity-over-private-peering"></a>通过专用对等互连建立本地连接

当客户从本地计算机连接到公共终结点时，需要使用[服务器级防火墙规则](firewall-create-server-level-portal-quickstart.md)将其 IP 地址添加到基于 IP 的防火墙。 尽管此模型非常适合用于允许对开发或测试工作负荷的单个计算机进行访问，但在生产环境中却难以管理。

借助专用链接，客户可以使用 [ExpressRoute](../../expressroute/expressroute-introduction.md)、专用对等互连或 VPN 隧道实现对专用终结点的跨界访问。 然后，客户可以通过公共终结点禁用所有访问，而无需使用基于 IP 的防火墙来允许任何 IP 地址。

## <a name="use-cases-of-private-link-for-azure-sql-database"></a>Azure SQL 数据库专用链接的用例 

客户端可以从同一虚拟网络、同一区域中的对等互联虚拟网络或通过跨区域的虚拟网络到虚拟网络连接连接到专用终结点。 此外，客户端可以使用 ExpressRoute、专用对等互连或 VPN 隧道从本地进行连接。 以下简化示意图显示了常见用例。

 ![连接选项示意图][1]

## <a name="test-connectivity-to-sql-database-from-an-azure-vm-in-same-virtual-network"></a>测试从同一虚拟网络中的 Azure VM 到 SQL 数据库的连接

此方案假设已创建一个运行 Windows Server 2016 的 Azure 虚拟机 (VM)。 

1. [启动远程桌面 (RDP) 会话并连接到虚拟机](../../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine)。 
1. 然后，可以使用以下工具执行一些基本的连接检查，以确保 VM 通过专用终结点连接到 SQL 数据库：
    1. Telnet
    1. Psping
    1. Nmap
    1. SQL Server Management Studio (SSMS)

### <a name="check-connectivity-using-telnet"></a>使用 Telnet 检查连接

[Telnet 客户端](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293%28v%3dws.10%29)是可用于测试连接的 Windows 功能。 根据 Windows OS 的版本，可能需要显式启用此功能。 

安装 Telnet 后，打开命令提示符窗口。 运行 Telnet 命令并指定 SQL 数据库中的数据库的 IP 地址和专用终结点。

```
>telnet 10.1.1.5 1433
```

当 Telnet 连接成功时，命令窗口中会显示下图所示的空白屏幕：

 ![Telnet 示意图][2]

### <a name="check-connectivity-using-psping"></a>使用 Psping 检查连接

可按如下所示使用 [Psping](https://docs.microsoft.com/sysinternals/downloads/psping) 检查专用终结点连接 (PEC) 是否正在侦听端口 1433 上的连接。

按如下所示运行 Psping，并提供逻辑 SQL Server 的 FQDN 和端口 1433：

```
>psping.exe mysqldbsrvr.database.chinacloudapi.cn:1433

PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.6.1.4:1433:
5 iterations (warmup 1) ping test:
Connecting to 10.6.1.4:1433 (warmup): from 10.6.0.4:49953: 2.83ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49954: 1.26ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49955: 1.98ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49956: 1.43ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49958: 2.28ms
```

输出显示 Psping 可以 ping 通与 PEC 关联的专用 IP 地址。

### <a name="check-connectivity-using-nmap"></a>使用 Nmap 检查连接

Nmap（网络映射器）是一个用于网络发现和安全审核的免费开源工具。 有关详细信息和下载链接，请访问 https://nmap.org 。可以使用此工具来确保专用终结点侦听端口 1433 上的连接。

按如下所示运行 Nmap，并提供托管专用终结点的子网的地址范围。

```
>nmap -n -sP 10.1.1.0/24
...
...
Nmap scan report for 10.1.1.5
Host is up (0.00s latency).
Nmap done: 256 IP addresses (1 host up) scanned in 207.00 seconds
```

结果显示，一个对应于专用终结点 IP 地址的 IP 地址已启动。

### <a name="check-connectivity-using-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 检查连接
> [!NOTE]
> 在客户端的连接字符串中 (`<server>.database.chinacloudapi.cn`) 使用服务器的完全限定域名 (FQDN)。 直接登录 IP 地址的任何尝试或使用专用链接 FQDN (`<server>.privatelink.database.chinacloudapi.cn`) 都将失败。 此行为是设计使然，因为专用终结点会将流量路由到该区域中的 SQL 网关，并且需要指定正确的 FQDN 才能成功登录。

请按照此处的步骤使用 [SSMS 连接到 SQL 数据库](connect-query-ssms.md)。 使用 SSMS 连接到 SQL 数据库后，请运行以下查询，验证是否正在从 Azure VM 的专用 IP 地址进行连接：

````
select client_net_address from sys.dm_exec_connections 
where session_id=@@SPID
````

## <a name="data-exfiltration-prevention"></a>数据渗透防护

Azure SQL 数据库中的数据渗透是指已获授权的用户（例如数据库管理员）能够从一个系统提取数据，并将其移到组织外部的其他位置或系统。 例如，该用户将数据移到第三方拥有的存储帐户。

借助专用链接，客户现在可以设置 NSG 等网络访问控制来限制对专用终结点的访问。 然后，将单个 Azure PaaS 资源映射到特定的专用终结点。 恶意的预览体验成员只能访问映射的 PaaS 资源（例如 SQL 数据库中的数据库），而不能访问其他资源。 

## <a name="limitations"></a>限制 
与专用终结点的连接仅支持使用“代理”作为[连接策略](connectivity-architecture.md#connection-policy)


## <a name="connecting-from-an-azure-vm-in-peered-virtual-network"></a>从对等互联虚拟网络中的 Azure VM 进行连接 

配置[虚拟网络对等互联](../../virtual-network/tutorial-connect-virtual-networks-powershell.md)，以便从对等互联虚拟网络中的 Azure VM 建立与 SQL 数据库的连接。

## <a name="connecting-from-an-azure-vm-in-virtual-network-to-virtual-network-environment"></a>从虚拟网络中的 Azure VM 连接到虚拟网络环境

配置[虚拟网络到虚拟网络 VPN 网关连接](../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)，以便从另一区域或订阅中的 Azure VM 建立与 SQL 数据库中的数据库的连接。

## <a name="connecting-from-an-on-premises-environment-over-vpn"></a>通过 VPN 从本地环境进行连接

若要建立从本地环境到 SQL 数据库中的数据库的连接，请选择并实施以下选项之一：
- [点到站点连接](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
- [站点到站点 VPN 连接](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- [ExpressRoute 线路](../../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)


## <a name="connecting-from-azure-synapse-analytics-to-azure-storage-using-polybase-and-the-copy-statement"></a>使用 Polybase 和 COPY 语句从 Azure Synapse Analytics 连接到 Azure 存储

PolyBase 和 COPY 语句通常用于将数据从 Azure 存储帐户加载到 Azure Synapse Analytics 中。 如果要从中加载数据的 Azure 存储帐户仅允许通过专用终结点、服务终结点或基于 IP 的防火墙访问一组虚拟网络子网，则通过 PolyBase 和 COPY 语句与该帐户建立的连接将会断开。 对于连接到 Azure 存储（已通过安全方式连接到虚拟网络）的 Azure Synapse Analytics，若要启用导入和导出方案，请执行[此处](vnet-service-endpoint-rule-overview.md#impact-of-using-virtual-network-service-endpoints-with-azure-storage)提供的步骤。 

## <a name="next-steps"></a>后续步骤

- 有关 Azure SQL 数据库安全概述，请参阅[保护数据库](security-overview.md)
- 有关 Azure SQL 数据库连接的概述，请参阅 [Azure SQL 连接体系结构](connectivity-architecture.md)

<!--Image references-->
[1]: media/quickstart-create-single-database/pe-connect-overview.png
[2]: media/quickstart-create-single-database/telnet-result.png
[3]: media/quickstart-create-single-database/pec-list-before.png
[4]: media/quickstart-create-single-database/pec-approve.png
[5]: media/quickstart-create-single-database/pec-list-after.png
[6]: media/quickstart-create-single-database/pec-select.png
