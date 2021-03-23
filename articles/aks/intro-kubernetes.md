---
title: Azure Kubernetes 服务简介
description: 了解 Azure Kubernetes 服务的功能和优势，以便在 Azure 中部署和管理基于容器的应用程序。
services: container-service
ms.topic: overview
origin.date: 02/09/2021
author: rockboyfor
ms.date: 03/22/2021
ms.testscope: no
ms.testdate: 05/25/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 67e5da26d3f98282888017977cc43ac35a3ddb1b
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766428"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS)

Azure Kubernetes Service (AKS) 通过将大量的复杂性和运营开销卸载到 Azure，简化了在 Azure 中部署托管 Kubernetes 群集的过程。 作为一项托管 Kubernetes 服务，Azure 可以为你处理运行状况监视和维护等关键任务。  

由于 Kubernetes 主节点由 Azure 管理，因此你只需要管理和维护代理节点。 因此，作为托管 Kubernetes 服务，AKS 是免费的；你只需支付群集中的代理节点费，无需支付主节点的费用。  

可以使用 Azure 门户、Azure CLI、Azure PowerShell 或使用模板驱动型部署选项（例如资源管理器模板和 Terraform）来创建 AKS 群集。 当你部署 AKS 群集时，系统会为你部署和配置 Kubernetes 主节点和所有节点。 另外，也可在部署过程中配置其他功能，例如高级网络、Azure Active Directory 集成、监视。 AKS 支持 Windows Server 容器。

有关 Kubernetes 基础知识的详细信息，请参阅 [AKS 的 Kubernetes 核心概念][concepts-clusters-workloads]。

若要开始，请[通过 Azure 门户][aks-portal]或者[通过 Azure CLI][aks-cli] 完成 AKS 快速入门。

<!--NOT AVAILABLE ON [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)-->

## <a name="access-security-and-monitoring"></a>访问权限、安全性和监视

为了增强安全性和管理，AKS 允许你集成 Azure Active Directory (Azure AD) 以及：
* 使用 Kubernetes 基于角色的访问控制 (Kubernetes RBAC)。 
* 监视群集和资源的运行状况。

### <a name="identity-and-security-management"></a>标识和安全管理

为了限制对群集资源的访问，AKS 支持 [Kubernetes RBAC][kubernetes-rbac]。 通过 Kubernetes RBAC，可以控制对 Kubernetes 资源和命名空间的访问，以及对这些资源的权限。  

还可将 AKS 群集配置为与 Azure AD 集成。 使用 Azure AD 集成时，可以将 Kubernetes 访问权限配置为基于现有标识和组成员身份。 可以为现有的 Azure AD 用户和组提供集成式登录体验，以及对 AKS 资源的访问权限。  

有关标识的详细信息，请参阅 [AKS 的访问权限和标识选项][concepts-identity]。

若要确保 AKS 群集的安全性，请参阅[将 Azure Active Directory 与 AKS 集成][aks-aad]。

### <a name="integrated-logging-and-monitoring"></a>集成式日志记录和监视

负责监视容器运行状况的 Azure Monitor 会从 AKS 群集和部署的应用程序中的容器、节点和控制器收集内存和处理器性能指标。 可以同时查看容器日志和 [Kubernetes 主日志][aks-master-logs]。 此监视数据存储在 Azure Log Analytics 工作区中，可以通过 Azure 门户、Azure CLI 或 REST 终结点获取。

有关详细信息，请参阅[监视 Azure Kubernetes 服务容器运行状况][container-health]。

## <a name="clusters-and-nodes"></a>群集和节点

AKS 节点在 Azure 虚拟机 (VM) 上运行。 通过 AKS 节点，可以将存储连接到节点和 Pod、升级群集组件以及使用 GPU。 AKS 支持运行多个节点池的 Kubernetes 群集，以支持混合操作系统和 Windows Server 容器。  

有关 Kubernetes 群集、节点和节点池功能的详细信息，请参阅 [AKS 的 Kubernetes 核心概念][concepts-clusters-workloads]。

### <a name="cluster-node-and-pod-scaling"></a>群集节点和 Pod 缩放

如果对资源的需求发生变化，用于运行服务的群集节点或 Pod 的数目就会自动增大或减小。 可以使用水平的 Pod 自动缩放程序或群集自动缩放程序。 这种缩放方法可以让 AKS 群集自动针对需求进行调整，只运行所需的资源。

有关详细信息，请参阅[缩放 Azure Kubernetes 服务 (AKS) 群集][aks-scale]。

### <a name="cluster-node-upgrades"></a>群集节点升级

AKS 提供多个 Kubernetes 版本。 新版本在 AKS 中可用以后，即可使用 Azure 门户或 Azure CLI 升级群集。 在升级过程中，节点会被仔细封锁和排除以尽量减少对正在运行的应用程序造成中断。  

若要详细了解生命周期版本，请参阅 [AKS 中支持的 Kubernetes 版本][aks-supported versions]。 有关升级步骤，请参阅[升级 Azure Kubernetes 服务 (AKS) 群集][aks-upgrade]。

### <a name="gpu-enabled-nodes"></a>启用了 GPU 的节点

AKS 支持创建启用了 GPU 的节点池。 Azure 目前提供单个或多个启用了 GPU 的 VM。 启用了 GPU 的 VM 是针对计算密集型、图形密集型和可视化工作负荷设计的。

有关详细信息，请参阅[使用 AKS 上的 GPU][aks-gpu]。

<!--NOT AVAILABLE ON ### Confidential computing nodes (public preview)-->
<!--NOT AVAILABLE on FEATURE Intel SGX-->
<!--NOT AVAILABLE ON [Confidential computing nodes on AKS][conf-com-node]-->

### <a name="storage-volume-support"></a>存储卷支持

若要支持应用程序工作负荷，可以为持久保存的数据装载存储卷。 可以使用静态卷和动态卷。 根据要共享存储卷的已连接 Pod 的数目，可以使用 Azure 磁盘支持的存储进行单个 Pod 的访问，也可以使用 Azure 文件存储支持的存储进行多个并发 Pod 的访问。

有关详细信息，请参阅 [AKS 中应用程序的存储选项][concepts-storage]。

使用 [Azure 磁盘][azure-disk]或 [Azure 文件存储][azure-files]完成动态永久性卷的入门。

## <a name="virtual-networks-and-ingress"></a>虚拟网络和入口

AKS 群集可以部署到现有的虚拟网络中。 在此配置中，群集中的每个 pod 在虚拟网络中分配有一个 IP 地址，可以直接与群集中的其他 pod 以及虚拟网络中的其他节点通信。 Pod 还可以连接到对等互连虚拟网络中的其他服务，通过 ExpressRoute 或站点到站点 (S2S) VPN 连接连接到本地网络。  

有关详细信息，请参阅 [AKS 中应用程序的网络概念][aks-networking]。

<!--NOT AVAILABLE ON [HTTP application routing][aks-http-routing]-->

<!--Not Available on ### Ingress with HTTP application routing-->
<!--Not Available on [HTTP application routing][aks-http-routing]-->

## <a name="development-tooling-integration"></a>开发工具集成

Kubernetes 提供丰富的开发和管理工具生态系统，可与 AKS 无缝配合使用。 这些工具包括用于 Visual Studio Code 的 Helm 和 Kubernetes 扩展。 这些工具可以与 AKS 无缝地配合使用。  

<!--Not Available on Additionally, Azure Dev Spaces provides a rapid, iterative Kubernetes development experience for teams. With minimal configuration, you can run and debug containers directly in AKS. To get started, see [Azure Dev Spaces][azure-dev-spaces]-->
<!--Not Available on To get started, see [Azure Dev Spaces][azure-dev-spaces]-->

<!--Not Available on The Azure DevOps project provides a simple solution for bringing existing code and Git repository into Azure. The DevOps project automatically creates Azure resources such as AKS, a release pipeline in Azure DevOps Services that includes a build pipeline for CI, sets up a release pipeline for CD, and then creates an Azure Application Insights resource for monitoring.-->

<!--Not Available on For more information, see [DevOps Starter][azure-devops]-->

## <a name="docker-image-support-and-private-container-registry"></a>Docker 映像支持和专用容器注册表

AKS 支持 Docker 映像格式。 若要对 Docker 映像进行专用存储，可以将 AKS 与 Azure 容器注册表 (ACR) 集成。

要创建专用映像存储，请参阅 [Azure 容器注册表][acr-docs]。

## <a name="kubernetes-certification"></a>Kubernetes 认证

AKS 已被 CNCF 认证为符合 Kubernetes 规范。

## <a name="regulatory-compliance"></a>法规符合性

AKS 符合 SOC、ISO、PCI DSS 和 HIPAA 规范。 有关详细信息，请参阅 [Azure 合规性概述][compliance-doc]。

## <a name="next-steps"></a>后续步骤

学习 Azure CLI 快速入门，了解有关部署和管理 AKS 的详细信息。

> [!div class="nextstepaction"]
> [AKS 快速入门][aks-cli]

<!-- LINKS - external -->

[aks-engine]: https://github.com/Azure/aks-engine
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[compliance-doc]: https://www.trustcenter.cn/cloudservices/azure.html

<!-- LINKS - internal -->

[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./azure-ad-integration-cli.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md

<!--NOT AVAILABLE ON [aks-http-routing]: ./http-application-routing.md-->

[aks-networking]: ./concepts-network.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md

<!--NOT AVAILABLE ON [azure-dev-spaces]: ../dev-spaces/index.yml-->
<!--NOT AVAILABLE ON [azure-devops]: ../devops-project/overview.md-->

[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../azure-monitor/containers/container-insights-overview.md
[aks-master-logs]: view-master-logs.md
[aks-supported versions]: supported-kubernetes-versions.md
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[kubernetes-rbac]: concepts-identity.md#kubernetes-role-based-access-control-kubernetes-rbac
[concepts-identity]: concepts-identity.md
[concepts-storage]: concepts-storage.md

<!--NOT AVAILABLE ON [conf-com-node]: ../confidential-computing/confidential-nodes-aks-overview.md-->

<!--Update_Description: update meta properties, wording update, update link-->