---
title: Service Fabric 群集资源管理器 - 放置策略
description: 概述 Service Fabric 服务的其他放置策略和规则
ms.topic: conceptual
origin.date: 08/18/2017
author: rockboyfor
ms.date: 01/11/2021
ms.testscope: no
ms.testdate: 11/09/2020
ms.author: v-yeche
ms.custom: devx-track-csharp
ms.openlocfilehash: 61ab841019998eadbcc52745257fe7086a718828
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022724"
---
# <a name="placement-policies-for-service-fabric-services"></a>Service Fabric 服务的放置策略
放置策略是可用于在某些不常见的特定情况下控制服务位置的附加规则。 这些情况的一些示例包括：

- Service Fabric 群集会跨越地理距离（如多个本地数据中心）或跨 Azure 区域
- 环境跨多个地缘政治控制区域或法定控制区域，或一些其他情况：有要强制实施的政策边界
- 由于远距离或者使用速度较慢或可靠性较低的网络链接，需要考虑通信性能或延迟
- 在并置某些工作负载时，需要尽量使其与其他工作负载一起，或者靠近客户
- 在单个节点上需要一个分区的多个无状态实例

大多数这些要求与群集的物理布局一致，以集群的容错域表示。 

可以帮助处理这些方案的高级放置策略包括：

1. 无效域
2. 所需域
3. 首选域
4. 不允许副本打包
5. 允许节点上存在多个无状态实例

以下大多数控件都能通过节点属性和放置约束来配置，但有一些控件比较复杂。 为了使操作更简单，Service Fabric 群集 Resource Manager 提供了这些附加放置策略。 每个命名服务实例配置了放置策略。 还可以进行动态更新。

## <a name="specifying-invalid-domains"></a>指定无效域
凭借 InvalidDomain 放置策略，可以指定某个特定容错域对特定服务是无效的。 此策略可确保特定的服务绝对不会在特定的区域中运行（例如，出于地缘政治或公司政策的原因）。 可以通过单独的策略指定多个无效域。

<center>

![无效域示例][Image1]

</center>

代码：

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell：

```powershell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast")
```
## <a name="specifying-required-domains"></a>指定所需域
所需的域放置策略要求服务仅存在于指定域中。 可以通过独立的策略指定多个所需域。

<center>

![所需域示例][Image2]

</center>

代码：

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell：

```powershell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>指定有状态服务主要副本的首选域
首选主域指定放置主要副本的容错域。 如果一切运行正常，主副本最终会在此域中。 如果域或主要副本出现故障或关闭，则主要副本会移到其他位置，理想情况下为同一域中的其他位置。 如果这个新位置不在首选域中，群集 Resource Manager 将尽可能快地将它移回到首选域。 当然，此设置仅适用于有状态服务。 对于跨越 Azure 区域或多个数据中心的群集，如果该群集的服务希望放置在某个位置，此策略最有用。 使主要副本接近其用户或其他服务有助于降低延迟，尤其是对于默认由主要副本处理的读取操作。

<center>

![首选主域和故障转移][Image3]

</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/ChinaEast/";
serviceDescription.PlacementPolicies.Add(primaryDomain);
```

PowerShell：

```powershell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/ChinaEast")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>需要副本分发并禁止封装
群集正常运行时，副本通常分布在容错域和升级域中。 但是，存在给定分区的多个副本最终可能会暂时打包到单个域中的情况。 例如，假设群集有九个节点在三个容错域（fd:/0、fd:/1 和 fd:/2）中。 再假设服务具有三个副本。 假设 fd:/1 和 fd:/2 中用于这些副本的节点发生故障。 群集资源管理器通常会首选这些相同容错域中的其他节点。 在此例中，假设由于容量问题这些域中的其他节点都无效。 如果群集 Resource Manager 生成这些副本的替换位置，它只能选择 fd:/0 中的节点。 但是，执行该操作就造成了违反容错域约束的情况。 打包副本会增加整个副本集发生故障或丢失的可能性。 

> [!NOTE]
> 有关约束和约束优先级的其他一般信息，请参阅[此主题](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities)。
>

如果曾经看到过类似“`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`”的运行状况消息，则可能已遇到这种情况或类似情况。 通常只会暂时将一个或两个副本打包在一起。 只要少于给定域中的大多数副本就是安全的。 打包的情况很少发生，但它可能发生，而且通常这些情况是暂时性的，因为这些节点会恢复正常。 如果这些节点确实一直处于关闭状态，并且群集 Resource Manager 需要生成替换位置，通常最适合的容错域中有其他可用节点。

某些工作负荷会愿意始终具有目标副本数，即使将它们打包到更少的域，也是如此。 这些工作负荷押注不会全部同时出现永久性域故障，并且通常可以恢复本地状态。 其他工作负荷则宁可早停机也不愿冒数据不正确或数据丢失的风险。 大多数生产工作负荷运行超过三个副本、超过三个容错域，以及每个容错域的多个有效节点。 因此，默认情况下默认行为允许域打包。 默认行为允许常规均衡和故障转移处理这些极端情况，即使这意味着要进行临时域打包。

如果要对给定工作负荷禁用此类打包，可以在服务中指定 `RequireDomainDistribution` 策略。 设置此策略后，群集资源管理器可确保同一容错域或升级域中不存在同一分区的两个副本。

代码：

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell：

```powershell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

现在，是否可针对未跨越地理区域的群集中的服务使用这些配置？ 可以使用，但也没有充分的理由。 应避免使用必需域、无效域和首选域配置，除非方案需要。 尝试强制给定工作负荷在单个机架中运行，或优先选择本地群集的某段没有任何意义。 应在容错域之间分布不同的硬件配置，并通过标准放置约束和节点属性处理这些配置。

## <a name="placement-of-multiple-stateless-instances-of-a-partition-on-single-node"></a>在单个节点上放置一个分区的多个无状态实例
AllowMultipleStatelessInstancesOnNode 放置策略允许在单个节点上放置一个分区的多个无状态实例。 默认情况下，不能在一个节点上放置单个分区的多个实例。 即使使用 -1 服务，对于给定的命名服务，也不可能将实例数扩展到群集中的节点数以上。 此放置策略将删除此限制，并允许将 InstanceCount 指定为大于节点数的值。

如果曾经看到过类似“`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: ReplicaExclusion`”的运行状况消息，则可能已遇到这种情况或类似情况。 

通过在服务上指定 `AllowMultipleStatelessInstancesOnNode` 策略，可以将 InstanceCount 设置为大于群集中的节点数的值。

代码：

```csharp
ServicePlacementAllowMultipleStatelessInstancesOnNodePolicyDescription allowMultipleInstances = new ServicePlacementAllowMultipleStatelessInstancesOnNodePolicyDescription();
serviceDescription.PlacementPolicies.Add(allowMultipleInstances);
```

PowerShell：

```powershell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -PlacementPolicy @("AllowMultipleStatelessInstancesOnNode") -InstanceCount 10 -ServicePackageActivationMode ExclusiveProcess 
```

> [!NOTE]
> 放置策略当前为预览版，位于 `EnableUnsupportedPreviewFeatures` 群集设置后。 由于这目前是预览功能，因此设置预览配置会阻止到/从该群集的升级。 换句话说，你需要创建一个新群集来试用该功能。
>

> [!NOTE]
> 目前，仅处于 ExclusiveProcess [服务包激活模式](https://docs.azure.cn/dotnet/api/system.fabric.description.servicepackageactivationmode)下的无状态服务支持该策略。
>

> [!WARNING]
> 与静态端口终结点一起使用时，不支持该策略。 将两者结合使用可能导致群集运行不正常，因为同一节点上的多个实例尝试绑定到同一端口，但无法启动。 
>

<!--Not Available on Using a high value of [MinInstanceCount](https://docs.azure.cn/dotnet/api/system.fabric.description.statelessservicedescription.mininstancecount)-->

## <a name="next-steps"></a>后续步骤
- 有关配置服务的详细信息，请参阅[了解如何配置服务](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png

<!-- Update_Description: update meta properties, wording update, update link -->