---
title: 使用邻近放置组来降低 Azure Kubernetes 服务 (AKS) 群集的延迟
description: 了解如何使用邻近放置组来降低 AKS 群集工作负载的延迟。
services: container-service
manager: gwallace
ms.topic: article
origin.date: 10/19/2020
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: yes|no
ms.testdate: 01/11/2021null
ms.author: v-yeche
ms.openlocfilehash: 6405b94440f176f212472da0efc365302fc2a5fa
ms.sourcegitcommit: 1107b0d16ac8b1ad66365d504c925735eb079d93
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99063676"
---
<!--Verified successfully on 01/04/2021-->
# <a name="reduce-latency-with-proximity-placement-groups"></a>使用邻近放置组降低延迟

> [!Note]
> 在 AKS 上使用邻近放置组时，归置仅适用于代理节点。 节点到节点延迟以及相应的托管 Pod 到 Pod 延迟得到改善。 归置不会影响群集的控制平面的放置。

在 Azure 中部署应用程序时，跨区域分布虚拟机 (VM) 实例会造成网络延迟，这可能会影响应用程序的总体性能。 邻近放置组是一种逻辑分组，用于确保 Azure 计算资源的物理位置彼此接近。 有些应用程序（如游戏、工程模拟和高频交易 (HFT)）需要低延迟和快速完成的任务。 对于这样的高性能计算 (HPC) 场景，请考虑为群集的节点池使用[邻近放置组](../virtual-machines/linux/co-location.md#proximity-placement-groups) (PPG)。

<!--REMOVE or availability zones-->

## <a name="before-you-begin"></a>开始之前

本文要求运行 Azure CLI 版本 2.14 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][azure-cli-install]。

### <a name="limitations"></a>限制

<!--Not Available on * A proximity placement group can map to at most one availability zone.-->
<!--Not Available on FEATURE availability zone-->

* 节点池必须使用虚拟机规模集来关联邻近放置组。
* 节点池只能在节点池创建时关联邻近放置组。

## <a name="node-pools-and-proximity-placement-groups"></a>节点池和邻近放置组

使用邻近放置组部署的第一个资源会附加到特定的数据中心。 使用同一邻近放置组部署的其他资源归置在同一数据中心。 停止（解除分配）或删除使用邻近放置组的所有资源后，邻近放置组将不再附加。

* 多个节点池可以与单个邻近放置组相关联。
* 一个节点池只能与单个邻近放置组相关联。

<!--Not Available on ### Configure proximity placement groups with availability zones-->

## <a name="create-a-new-aks-cluster-with-a-proximity-placement-group"></a>使用邻近放置组创建新的 AKS 群集

以下示例使用 [az group create][az-group-create] 命令在 *chinaeast2* 区域中创建名为 *myResourceGroup* 的资源组。 然后使用 [az AKS create][az-aks-create] 命令创建名为 *myAKSCluster* 的 AKS 群集。

加速网络可极大地提高虚拟机的网络性能。 理想情况是将邻近放置组与加速网络一起使用。 默认情况下，AKS 在[支持的虚拟机实例](../virtual-network/create-vm-accelerated-networking-cli.md?toc=/virtual-machines/linux/toc.json#limitations-and-constraints)上使用加速网络，其中包括具有两个或多个 vCPU 的大多数 Azure 虚拟机。

创建一个新的 AKS 集群，其中的邻近放置组与第一个系统节点池相关联：

```azurecli
# Create an Azure resource group
az group create --name myResourceGroup --location chinaeast2
```
运行以下命令，并存储返回的 ID：

```azurecli
# Create proximity placement group
az ppg create -n myPPG -g myResourceGroup -l chinaeast2 -t standard
```

此命令生成输出，其中包括接下来的 CLI 命令所需的 id 值：

```output
{
  "availabilitySets": null,
  "colocationStatus": null,
  "id": "/subscriptions/yourSubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Compute/proximityPlacementGroups/myPPG",
  "location": "chinaeast2",
  "name": "myPPG",
  "proximityPlacementGroupType": "Standard",
  "resourceGroup": "myResourceGroup",
  "tags": {},
  "type": "Microsoft.Compute/proximityPlacementGroups",
  "virtualMachineScaleSets": null,
  "virtualMachines": null
}
```

在以下命令中将邻近放置组资源 ID 用作myPPGResourceID 值：

```azurecli
# Create an AKS cluster that uses a proximity placement group for the initial system node pool only. The PPG has no effect on the cluster control plane.
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --ppg myPPGResourceID
```

## <a name="add-a-proximity-placement-group-to-an-existing-cluster"></a>向现有群集添加邻近放置组

可通过创建一个新节点池来向现有群集添加邻近放置组。 然后，可选择将现有工作负载迁移到新节点池，然后删除原始节点池。

使用之前创建的同一邻近放置组，这将确保 AKS 群集的两个节点池中的代理节点物理上位于同一数据中心。

使用之前创建的邻近放置组中的资源 ID，并使用 [`az aks nodepool add`][az-aks-nodepool-add] 命令添加新的节点池：

```azurecli
# Add a new node pool that uses a proximity placement group, use a --node-count = 1 for testing
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 1 \
    --ppg myPPGResourceID
```

## <a name="clean-up"></a>清理

若要删除群集，请使用 [`az group delete`][az-group-delete] 命令删除 AKS 资源组：

```azurecli
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>后续步骤

* 详细了解[邻近放置组][proximity-placement-groups]。

<!-- LINKS - Internal -->

[azure-ad-rbac]: azure-ad-rbac.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: https://docs.azure.cn/cli/install-azure-cli
[az-aks-get-upgrades]: https://docs.azure.cn/cli/aks#az_aks_get_upgrades
[az-aks-upgrade]: https://docs.azure.cn/cli/aks#az_aks_upgrade
[az-aks-show]: https://docs.azure.cn/cli/aks#az_aks_show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[az-extension-add]: https://docs.azure.cn/cli/extension#az_extension_add
[az-extension-update]: https://docs.azure.cn/cli/extension#az_extension_update
[proximity-placement-groups]: ../virtual-machines/co-location.md#proximity-placement-groups
[az-aks-create]: https://docs.azure.cn/cli/aks#az_aks_create
[system-pool]: ./use-system-pools.md
[az-aks-nodepool-add]: https://docs.azure.cn/cli/aks/nodepool#az_aks_nodepool_add
[az-aks-create]: https://docs.azure.cn/cli/aks#az_aks_create
[az-group-create]: https://docs.azure.cn/cli/group#az_group_create
[az-group-delete]: https://docs.azure.cn/cli/group#az_group_delete

<!--Update_Description: update meta properties, wording update, update link-->