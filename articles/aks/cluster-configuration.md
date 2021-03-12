---
title: Azure Kubernetes 服务 (AKS) 中的群集配置
description: 了解如何在 Azure Kubernetes 服务 (AKS) 中配置群集
services: container-service
ms.topic: article
origin.date: 02/09/2020
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 01/25/2021
ms.author: v-yeche
author: rockboyfor
ms.openlocfilehash: 06ad05b2a35d7f6b1f8da2531518286ec173643a
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055309"
---
<!--Verified successfully-->
<!--MOONCAKE: AKS VERSION IS 1.18.14-->
# <a name="configure-an-aks-cluster"></a>配置 AKS 群集

在创建 AKS 群集的过程中，你可能需要自定义群集配置来满足你的需求。 本文介绍了几个用于自定义 AKS 群集的选项。

## <a name="os-configuration"></a>OS 配置

AKS 现在支持 Ubuntu 18.04 的正式发布 (GA) 版作为 kubernetes 版本高于 1.18 的群集的默认节点操作系统 (OS)，对于低于 1.18 的版本，AKS Ubuntu 16.04 仍是默认的基础映像。 从 Kubernetes v1.18 及更高版本开始，默认基础映像是 AKS Ubuntu 18.04。

> [!IMPORTANT]
> 在 Kubernetes v1.18 或更高版本上创建的节点池默认使用 `AKS Ubuntu 18.04` 节点映像。 低于 1.18 的受支持 Kubernetes 版本上的节点池会接收 `AKS Ubuntu 16.04` 作为节点映像，但在节点池 Kubernetes 版本更新到 v1.18 或更高版本后就会更新到 `AKS Ubuntu 18.04`。
> 
> 强烈建议在使用 1.18 或更高版本上创建的群集之前，在 AKS Ubuntu 18.04 节点池上测试工作负荷。

### <a name="use-aks-ubuntu-1804-ga-on-new-clusters"></a>在新群集上使用 AKS Ubuntu 18.04 (GA)

在 Kubernetes v1.18 或更高版本上创建的群集默认使用 `AKS Ubuntu 18.04` 节点映像。 低于 1.18 的受支持 Kubernetes 版本上创建的节点池仍会接收 `AKS Ubuntu 16.04` 作为节点映像，但在群集或节点池 Kubernetes 版本更新到 v1.18 或更高后就会更新到 `AKS Ubuntu 18.04`。

强烈建议在使用 1.18 或更高版本上创建的群集之前，在 AKS Ubuntu 18.04 节点池上测试工作负荷。

若要使用 `AKS Ubuntu 18.04` 节点映像创建群集，只需创建运行 Kubernetes v1.18 或更高版本的群集，如下所示

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

> [!NOTE]
> 可通过以下 CLI cmdlet 查找有效的 Kubernetes 版本。
> `az aks get-versions --location chinaeast2 --output table`

### <a name="use-aks-ubuntu-1804-ga-on-existing-clusters"></a>在现有的群集上使用 AKS Ubuntu 18.04 (GA)

在 Kubernetes v1.18 或更高版本上创建的群集默认使用 `AKS Ubuntu 18.04` 节点映像。 低于 1.18 的受支持 Kubernetes 版本上创建的节点池仍会接收 `AKS Ubuntu 16.04` 作为节点映像，但在群集或节点池 Kubernetes 版本更新到 v1.18 或更高后就会更新到 `AKS Ubuntu 18.04`。

强烈建议在使用 1.18 或更高版本上创建的群集之前，在 AKS Ubuntu 18.04 节点池上测试工作负荷。

如果群集或节点池已做好使用 `AKS Ubuntu 18.04` 节点映像的准备，则只需将其升级到 v1.18 或更高版本，如下所示。

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

如果只想升级一个节点池，请执行以下命令：

```azurecli
az aks nodepool upgrade -name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

<a name="test-aks-ubuntu-1804-generally-available-on-existing-clusters"></a>
### <a name="test-aks-ubuntu-1804-ga-on-existing-clusters"></a>在现有的群集上测试 AKS Ubuntu 18.04 (GA)

在 Kubernetes v1.18 或更高版本上创建的节点池默认使用 `AKS Ubuntu 18.04` 节点映像。 低于 1.18 的受支持 Kubernetes 版本上创建的节点池仍会接收 `AKS Ubuntu 16.04` 作为节点映像，但在节点池 Kubernetes 版本更新到 v1.18 或更高后就会更新到 `AKS Ubuntu 18.04`。

强烈建议在升级生产节点池之前，在 AKS Ubuntu 18.04 节点池上测试工作负荷。

若要使用 `AKS Ubuntu 18.04` 节点映像创建节点池，只需创建运行 Kubernetes v1.18 或更高版本的节点池。 群集控制平面也至少需要位于 v1.18 或更高版本上，但其他节点池可以保留在较旧的 Kubernetes 版本上。
下面，我们将首先升级控制平面，然后使用将会接收新节点映像 OS 版本的 v1.18 来创建新节点池。

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14 --control-plane-only

az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.18.14
```

<!--Not Available on ### Use AKS Ubuntu 18.04 on new clusters (Preview)-->
<!--Not Available on ### Use AKS Ubuntu 18.04 existing clusters (Preview)-->

<!--Not Available on TILL 01/28/2021 ## Container runtime configuration-->

<!--Not Available on ### Use `containerd` as your container runtime (preview)-->
<!--Not Available on ### Use `containerd` on new clusters (preview)-->

<!--Not Available on till 01/28/2021 ### `Containerd` limitations/differences-->

<!--Not Available on ## Generation 2 virtual machines (Preview)-->
<!--Not Avaialble on ### Use Gen2 VMs on new clusters (Preview)-->

## <a name="ephemeral-os"></a>临时 OS

默认情况下，Azure 会自动将虚拟机的操作系统磁盘复制到 Azure 存储，以避免在 VM 需要重定位到另一台主机时丢失数据。 但是，由于容器并未设计为保留本地状态，因此该行为提供的价值有限且存在一些缺点，其中包括节点预配速度较慢、读/写延迟较高。

相比而言，临时 OS 磁盘只存储在主机上，就像临时磁盘一样。 这样的读/写延迟较低，且节点缩放和群集升级速度较快。

与临时磁盘类似，临时 OS 磁盘包含在虚拟机的价格中，因此不会产生额外的存储成本。

> [!IMPORTANT]
>如果用户未显式请求用于 OS 的托管磁盘，则在可能的情况下，AKS 会针对给定的 nodepool 配置默认使用临时 OS。

使用临时 OS 时，OS 磁盘必须适合 VM 缓存。 VM 缓存的大小在 [Azure 文档](../virtual-machines/dv3-dsv3-series.md)中以括号的形式提供，位于 IO 吞吐量旁边（“以 GiB 为单位的缓存大小”）。

以 AKS 默认 VM 大小 Standard_DS2_v2 和默认 OS 磁盘大小 100GB 为例，此 VM 大小支持临时 OS，但只有 86GB 的缓存大小。 如果用户未进行显式指定，则此配置默认为托管磁盘。 如果用户显式请求了临时 OS，则用户会收到验证错误。

如果用户请求 OS 磁盘大小为 60GB 的同一 Standard_DS2_v2，则此配置将默认为临时 OS：请求的 60GB 大小小于最大缓存大小 86GB。

将 Standard_D8s_v3 与 100GB OS 磁盘配合使用时，此 VM 大小支持临时 OS，有 200GB 的缓存空间。 如果用户未指定 OS 磁盘类型，则默认情况下，nodepool 会收到临时 OS。 

临时 OS 至少需要 2.15.0 版的 Azure CLI。

### <a name="use-ephemeral-os-on-new-clusters"></a>在新群集上使用临时 OS

配置群集，以便在创建群集时使用临时 OS 磁盘。 使用 `--node-osdisk-type` 标志将临时 OS 设置为新群集的 OS 磁盘类型。

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS3_v2 --node-osdisk-type Ephemeral
```

若要使用通过网络附加的 OS 磁盘来创建常规群集，可以指定 `--node-osdisk-type=Managed`。 还可以选择添加更多的临时 OS 节点池，如下所示。

### <a name="use-ephemeral-os-on-existing-clusters"></a>在现有群集上使用临时 OS
配置新节点池，以使用临时 OS 磁盘。 使用 `--node-osdisk-type` 标志将 OS 磁盘类型设置为该节点池的 OS 磁盘类型。

```azurecli
az aks nodepool add --name ephemeral --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS3_v2 --node-osdisk-type Ephemeral
```

> [!IMPORTANT]
> 如果使用临时 OS，可以部署不超过 VM 缓存大小的 VM 和实例映像。 在使用 AKS 的情况下，默认节点 OS 磁盘配置使用 128GB，这意味着所需 VM 大小的缓存大于 128GB。 默认 Standard_DS2_v2 的缓存大小为 86GB，不够大。 Standard_DS3_v2 的缓存大小为 172GB，足够大。 还可以通过使用 `--node-osdisk-size` 来减小 OS 磁盘的默认大小。 AKS 映像的最小大小为 30GB。 

若要使用通过网络附加的 OS 磁盘来创建节点池，可以指定 `--node-osdisk-type Managed`。

## <a name="custom-resource-group-name"></a>自定义资源组名称

在 Azure 中部署 Azure Kubernetes 服务群集时，会为工作器节点创建第二个资源组。 默认情况下，AKS 会将节点资源组命名为 `MC_resourcegroupname_clustername_location`，但你也可以提供自己的名称。

若要指定自己的资源组名称，请安装 aks-preview Azure CLI 扩展版本 0.3.2 或更高版本。 使用 Azure CLI 时，通过 `az aks create` 命令的 `--node-resource-group` 参数为资源组指定自定义名称。 如果使用 Azure 资源管理器模板部署 AKS 群集，可以使用 `nodeResourceGroup` 属性定义资源组名称。

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

第二个资源组由订阅中的 Azure 资源提供程序自动创建。 只有创建了群集，才能指定自定义资源组名称。 

请注意，对于节点资源组，不能执行以下操作：

- 不能为节点资源组指定现有资源组。
- 为节点资源组指定不同的订阅。
- 创建群集后更改节点资源组名称。
- 不能为节点资源组内的受管理资源指定名称。
- 不能修改或删除节点资源组内受管理资源中由 Azure 创建的标记。

## <a name="next-steps"></a>后续步骤

- 了解如何在群集中[升级节点映像](node-image-upgrade.md)。
- 若要了解如何将群集升级到最高版本的 Kubernetes，请参阅[升级 Azure Kubernetes 服务 (AKS) 群集](upgrade-cluster.md)。

    <!--Not Available on till 01/28/2021 - Read more about [`containerd` and Kubernetes](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/)-->
    
- 若要查找有关一些常用 AKS 问题的答案，请参阅 [AKS 常见问题解答](faq.md)。
- 详细了解[临时 OS 磁盘](../virtual-machines/ephemeral-os-disks.md)。

<!-- LINKS - internal -->

[azure-cli-install]: https://docs.azure.cn/cli/install-azure-cli
[az-feature-register]: https://docs.azure.cn/cli/feature#az_feature_register
[az-feature-list]: https://docs.azure.cn/cli/feature#az_feature_list
[az-provider-register]: https://docs.azure.cn/cli/provider#az_provider_register
[az-extension-add]: https://docs.azure.cn/cli/extension#az_extension_add
[az-extension-update]: https://docs.azure.cn/cli/extension#az_extension_update
[az-feature-register]: https://docs.azure.cn/cli/feature#az_feature_register
[az-feature-list]: https://docs.azure.cn/cli/feature#az_feature_list
[az-provider-register]: https://docs.azure.cn/cli/provider#az_provider_register

<!--Update_Description: update meta properties, wording update, update link-->