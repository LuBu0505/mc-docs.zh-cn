---
title: 为可用性组配置 DNN 侦听器
description: 了解如何配置分布式网络名称 (DNN) 侦听器来替换虚拟网络名称 (VNN) 侦听器，并将流量路由到 Azure VM 的 SQL Server 上的 Always On 可用性组。
services: virtual-machines-windows
documentationcenter: na
author: WenJason
manager: digimobile
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 10/07/2020
ms.date: 01/04/2021
ms.author: v-jay
ms.reviewer: jroth
ms.openlocfilehash: 24047caf1e52cff0a94b154093bbe23342a6d992
ms.sourcegitcommit: cf3d8d87096ae96388fe273551216b1cb7bf92c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/31/2020
ms.locfileid: "97830398"
---
# <a name="configure-a-dnn-listener-for-an-availability-group"></a>为可用性组配置 DNN 侦听器
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

使用 Azure VM 上的 SQL Server 时，分布式网络名称 (DNN) 将流量路由到相应的群集化资源。 它提供了一种比虚拟网络名称 (VNN) 侦听器更简单的方法来连接到 Always On 可用性组 (AG)，不需要使用 Azure 负载均衡器。 

本文介绍了如何配置 DNN 侦听器来替换 VNN 侦听器，并使用 Azure VM 上的 SQL Server 将流量路由到你的可用性组，以实现高可用性和灾难恢复 (HADR)。 

DNN 侦听器功能目前仅适用于 Windows Server 2016 及更高版本上的 SQL Server 2019 CU8。 

对于备用连接选项，可考虑改用 [VNN 侦听器和 Azure 负载均衡器](availability-group-vnn-azure-load-balancer-configure.md)。 

## <a name="overview"></a>概述

当与 [SQL Server VM 上的 Always On 可用性组](availability-group-overview.md)一起使用时，分布式网络名称 (DNN) 侦听器会取代传统的虚拟网络名称 (VNN) 可用性组侦听器。 这样就不需通过 Azure 负载均衡器来路由流量，简化了部署、维护并改进了故障转移。 

请使用 DNN 侦听器来替代现有的 VNN 侦听器，或者将它与现有的 VNN 侦听器一起使用，使你的可用性组有两个不同的连接点 - 一个使用 VNN 侦听器名称（如果非默认，则还使用 VNN 侦听器端口），而另一个使用 DNN 侦听器名称和端口。 

## <a name="prerequisites"></a>先决条件

在完成本文中的步骤之前，应已做好以下准备：

- CU8 或更高版本上的 SQL Server 2019（在 Windows Server 2016 及更高版本上）
- 确定了分布式网络名称是适合[用于你的 HADR 解决方案的连接性选项](hadr-cluster-best-practices.md#connectivity)。
- 配置了你的 [Always On 可用性组](availability-group-overview.md)。 
- 安装了最新版本的 [PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。 


## <a name="create-script"></a>创建脚本

使用 PowerShell 创建分布式网络名称 (DNN) 资源，并将其与你的可用性组相关联。 

为此，请执行下列步骤： 

1. 打开文本编辑器，例如记事本。 
1. 复制并粘贴以下脚本： 

   ```powershell
   param (
      [Parameter(Mandatory=$true)][string]$Ag,
      [Parameter(Mandatory=$true)][string]$Dns,
      [Parameter(Mandatory=$true)][string]$Port
   )
   
   Write-Host "Add a DNN listener for availability group $Ag with DNS name $Dns and port $Port"
   
   $ErrorActionPreference = "Stop"
   
   # create the DNN resource with the port as the resource name
   Add-ClusterResource -Name $Port -ResourceType "Distributed Network Name" -Group $Ag 
   
   # set the DNS name of the DNN resource
   Get-ClusterResource -Name $Port | Set-ClusterParameter -Name DnsName -Value $Dns 
   
   # start the DNN resource
   Start-ClusterResource -Name $Port
   
   
   $Dep = Get-ClusterResourceDependency -Resource $Ag
   if ( $Dep.DependencyExpression -match '\s*\((.*)\)\s*' )
   {
   $DepStr = "$($Matches.1) or [$Port]"
   }
   else
   {
   $DepStr = "[$Port]"
   }
   
   Write-Host "$DepStr"
   
   # add the Dependency from availability group resource to the DNN resource
   Set-ClusterResourceDependency -Resource $Ag -Dependency "$DepStr"
   
   
   #bounce the AG resource
   Stop-ClusterResource -Name $Ag
   Start-ClusterResource -Name $Ag
   ```

1. 将脚本另存为 `.ps1` 文件，例如 `add_dnn_listener.ps1`。 


## <a name="execute-script"></a>执行脚本

若要创建 DNN 侦听器，请执行脚本并传入可用性组名称、侦听器名称和端口的参数。 

例如，假设可用性组名称为 `ag1`，侦听器名称为 `dnnlsnr`，侦听器端口为 `6789`，请执行以下步骤： 

1. 打开一个命令行界面工具，例如命令提示符或 PowerShell。 
1. 导航到保存 `.ps1` 脚本的位置，例如 c:\Documents。 
1. 执行脚本：```add_dnn_listener.ps1 <ag name> <listener-name> <listener port>```。 例如： 

   ```console
   c:\Documents> add_dnn_listener.ps1 ag1 dnnlsnr 6789
   ```

## <a name="verify-listener"></a>验证侦听器

使用 SQL Server Management Studio 或 Transact-SQL 确认是否已成功创建 DNN 侦听器。 

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

在 [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 中展开“可用性组侦听器”来查看你的 DNN 侦听器： 

:::image type="content" source="media/availability-group-distributed-network-name-dnn-listener-configure/dnn-listener-in-ssms.png" alt-text="在 SQL Server Management Studio (SSMS) 中的“可用性组侦听器”下查看 DNN 侦听器":::

### <a name="transact-sql"></a>Transact-SQL

使用 Transact-SQL 查看 DNN 侦听器的状态： 

```sql
SELECT * FROM SYS.AVAILABILITY_GROUP_LISTENERS
```

`is_distributed_network_name` 的值为 `1` 表示侦听器是分布式网络名称 (DNN) 侦听器： 

:::image type="content" source="media/availability-group-distributed-network-name-dnn-listener-configure/dnn-listener-tsql.png" alt-text="使用 sys.availability_group_listeners 来识别is_distributed_network_name 中的值为 1 的 DNN 侦听器":::


## <a name="update-connection-string"></a>更新连接字符串

更新应用程序的连接字符串，使其连接到 DNN 侦听器。 若要确保故障转移时的快速连接，请将 `MultiSubnetFailover=True` 添加到连接字符串（如果 SQL 客户端支持它）。 

## <a name="test-failover"></a>测试故障转移

测试可用性组的故障转移以确保其正常运行。 

若要测试故障转移，请执行以下步骤： 

1. 使用 [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 连接到 DNN 侦听器或副本之一。 
1. 在“对象资源管理器”中展开“Always On 可用性组”。  
1. 右键单击该可用性组，然后选择“故障转移”以打开“故障转移向导”。 
1. 按照提示选择故障转移目标，将可用性组故障转移到次要副本。 
1. 确认数据库在新的主要副本上处于已同步状态。 
1. （可选）故障回复到原始主要副本或另一个次要副本。 

## <a name="test-connectivity"></a>测试连接

通过以下步骤测试与 DNN 侦听器的连接：

1. 打开 SQL Server Management Studio  。
1. 连接到你的 DNN 侦听器。 
1. 打开一个新的查询窗口，通过运行 `SELECT @@SERVERNAME` 来检查你已连接到哪个副本。 
1. 将可用性组故障转移到另一个副本。
1. 经过一段合理的时间后，运行 `SELECT @@SERVERNAME` 来确认你的可用性组现在承载在另一个副本上。 


## <a name="limitations"></a>限制

- 目前，只有 Windows Server 2016 及更高版本上的 SQL Server 2019 CU8 及更高版本支持可用性组的 DNN 侦听器。 
- 将其他 SQL Server 功能与包含 DNN 的可用性组一起使用时，可能有额外的注意事项。 有关详细信息，请参阅[包含 DNN 的 AG 的互操作性](availability-group-dnn-interoperability.md)。 

## <a name="next-steps"></a>后续步骤

若要详细了解 Azure 中的 SQL Server HADR 功能，请参阅[可用性组](availability-group-overview.md)和[故障转移群集实例](failover-cluster-instance-overview.md)。 你还可以了解对环境进行配置以实现高可用性和灾难恢复的[最佳做法](hadr-cluster-best-practices.md)。 


