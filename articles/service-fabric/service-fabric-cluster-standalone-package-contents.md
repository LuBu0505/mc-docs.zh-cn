---
title: 适用于 Windows Server 的 Azure Service Fabric 独立包
description: 适用于 Windows Server 的 Azure{1}{2}Service Fabric 独立包的说明和内容。
ms.topic: conceptual
origin.date: 08/10/2017
author: rockboyfor
ms.date: 09/14/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 53503b370bac4f4d35a701b6194f2a2d8306ec95
ms.sourcegitcommit: e1cd3a0b88d3ad962891cf90bac47fee04d5baf5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89655256"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>适用于 Windows Server 的 Service Fabric 独立包的内容
在[下载的](https://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric 独立包中，可找到以下文件：

| **文件名** | **简短说明** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |PowerShell 脚本，用于通过 ClusterConfig.json 中的设置创建群集。 |
| RemoveServiceFabricCluster.ps1 |PowerShell 脚本，用于通过 ClusterConfig.json 中的设置删除群集。 |
| AddNode.ps1 |PowerShell 脚本，用于在当前计算机上将节点添加到现有的部署群集。 |
| RemoveNode.ps1 |PowerShell 脚本，用于在当前计算机上将节点从现有的部署群集中删除。 |
| CleanFabric.ps1 |PowerShell 脚本，用于从当前计算机中清除独立 Service Fabric 安装。 应使用以前的 MSI 安装的自身关联卸载程序来删除以前的安装。 |
| TestConfiguration.ps1 |PowerShell 脚本，用于分析 Cluster.json 中指定的基础结构。 |
| DownloadServiceFabricRuntimePackage.ps1 |用于下载最新带外运行时包的 PowerShell 脚本，适用于部署计算机未连接到 Internet 的方案。 |
| DeploymentComponentsAutoextractor.exe |包含独立包脚本所用部署组件的自解压缩存档。 |
| EULA_ENU.txt |有关使用 Azure Service Fabric Windows Server 独立包的许可条款。 现在，可以[下载 EULA 的副本](https://go.microsoft.com/fwlink/?LinkID=733084)。 |
| Readme.txt |发行说明和基本安装说明的链接。 这是本文中说明的子集。 |
| ThirdPartyNotice.rtf |包中的第三方软件的通知。 |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |StandaloneLogCollector.exe 按需运行，收集跟踪日志并将其上传到 Azure 以提供支持。 |
| Tools\ServiceFabricUpdateService.zip |用于为不具有 Internet 访问权限的群集启用自动代码升级的工具。 在[此处](service-fabric-cluster-upgrade-windows-server.md)可以找到更多详细信息|

**模板** 

| **文件名** | **简短说明** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |群集配置示例文件，其中包含非安全型三节点式单计算机（或虚拟机）开发群集的设置，这些设置包括群集中每个节点的信息。 |
| ClusterConfig.Unsecure.MultiMachine.json |群集配置示例文件，其中包含非安全型多计算机（或虚拟机）群集的设置，这些设置包括群集中每个计算机的信息。 |
| ClusterConfig.Windows.DevCluster.json |群集配置示例文件，其中包含安全型三节点式单计算机（或虚拟机）开发群集的所有设置，这些设置包括群集中每个节点的信息。 此群集使用 [Windows 标识](https://docs.microsoft.com/previous-versions/msp-n-p/ff649396(v=pandp.10))进行保护。 |
| ClusterConfig.Windows.MultiMachine.json |群集配置示例文件，其中包含使用 Windows 安全性措施的安全型多计算机（或虚拟机）群集的所有设置，这些设置包括安全群集中每个计算机的信息。 此群集使用 [Windows 标识](https://docs.microsoft.com/previous-versions/msp-n-p/ff649396(v=pandp.10))进行保护。 |
| ClusterConfig.x509.DevCluster.json |群集配置示例文件，其中包含安全型三节点式单计算机（或虚拟机）开发群集的所有设置，这些设置包括群集中每个节点的信息。 此群集使用 x509 证书进行保护。 |
| ClusterConfig.x509.MultiMachine.json |群集配置示例文件，其中包含安全型多计算机（或虚拟机）群集的所有设置，这些设置包括安全群集中每个节点的信息。 此群集使用 x509 证书进行保护。 |
| ClusterConfig.gMSA.Windows.MultiMachine.json |群集配置示例文件，其中包含安全型多计算机（或虚拟机）群集的所有设置，这些设置包括安全群集中每个节点的信息。 使用[组托管服务帐户](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj128431(v=ws.11))保护该群集。 |

## <a name="cluster-configuration-samples"></a>群集配置示例
可在以下 GitHub 页面找到最新版本的群集配置模板：[独立群集配置示例](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)。

## <a name="independent-runtime-package"></a>独立运行时包
在从[下载链接 - Service Fabric 运行时 - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354) 部署群集的过程中，会自动下载最新的运行时包。

## <a name="related"></a>相关内容
* [创建独立 Azure Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)
* [Service Fabric 群集安全性方案](service-fabric-windows-cluster-windows-security.md)

<!-- Update_Description: update meta properties, wording update, update link -->