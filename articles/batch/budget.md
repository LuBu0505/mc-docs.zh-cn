---
title: 成本分析和预算
description: 了解如何针对那些用于运行 Batch 工作负荷的基础计算资源和软件许可证获取成本分析、设置预算和降低成本。
ms.topic: how-to
ms.service: batch
origin.date: 01/21/2021
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: no
ms.testdate: 04/26/2020
ms.author: v-yeche
ms.openlocfilehash: 6c430ec75a120203ee4fac3040b4211009422107
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059285"
---
# <a name="cost-analysis-and-budgets-for-azure-batch"></a>Azure Batch 成本分析和预算

使用 Azure Batch 本身不会产生任何费用，但是用于运行 Batch 工作负荷的底层计算资源和软件许可证可能会产生费用。 成本可能产生于池中的虚拟机 (VM)、从 VM 进行的数据传输，或云中存储的任何输入或输出数据。 本主题将帮助你了解成本来自何处、如何为 Batch 池或帐户设置预算，以及降低 Batch 工作负荷成本的方法。

## <a name="batch-resources"></a>Batch 资源

虚拟机是用于 Batch 处理的最重要资源。 Batch 的 VM 使用成本是根据类型、数量和使用持续时间计算的。 VM 计费选项包括[标准预付费套餐](https://wd.azure.cn/pricing/pia-waiting-list/)或 [MSDN 订阅 Azure 权益套餐](https://www.azure.cn/offers/ms-mc-arz-msdn/index.html)。 根据具体的计算工作负荷，这两个支付选项各有优势，将对帐单产生不同的影响。

<!--CORRECT ON [MSDN Subscriptions Azure Benefit Offer](https://www.azure.cn/offers/ms-mc-arz-msdn/index.html)-->
<!--Not Available on [reservation](../cost-management-billing/reservations/save-compute-costs-reservations.md)-->

使用[应用包](batch-application-packages.md)将应用部署到 Batch 节点 (VM) 时，还需要为应用包使用的 Azure 存储资源付费。 你还要为任何输入或输出文件（如资源文件和其他日志数据）的存储支付费用。 通常，与 Batch 关联的存储数据的成本要比计算资源的成本低得多。 使用[虚拟机配置](nodes-and-pools.md#virtual-machine-configuration)在池中创建的每个 VM 都有一个使用 Azure 托管磁盘的关联 OS 磁盘。 Azure 托管磁盘有额外的成本，其他磁盘性能层也有不同的成本。

Batch 池使用网络资源。 具体而言，对于 **VirtualMachineConfiguration** 池，将使用标准负载均衡器，这需要静态 IP 地址。 Batch 使用的负载均衡器对“用户订阅”帐户可见，但对“Batch 服务”帐户不可见。 传入和传出 Batch 池 VM 的所有数据都会让标准负载均衡器产生费用；选择从池节点检索数据的 Batch API（如“获取任务/节点文件”）、任务应用包、资源/输出文件和容器映像会产生费用。

### <a name="additional-services"></a>其他服务

在计算 Batch 帐户的成本时，可能需要考虑不包括 VM 和存储的服务这一因素。

通常用于 Batch 的其他服务可能包括：

- Application Insights
- 数据工厂
- Azure Monitor
- 虚拟网络
- 带有图形应用的 VM

根据要将哪些服务用于 Batch 解决方案，可能会产生额外的费用。 若要确定每个附加服务的成本，请参阅[定价计算器](https://www.azure.cn/pricing/calculator/)。

<!--Not Available on ## Cost analysis and budget for a pool-->

## <a name="minimize-cost"></a>最大限度地降低成本

长时间使用多个 VM 和 Azure 服务的成本可能会很高。 请考虑使用这些策略来最大程度地提高工作负荷效率并降低成本。

<!--Not Available on ### Low-priority virtual machines-->

### <a name="virtual-machine-os-disk-type"></a>虚拟机 OS 磁盘类型

Azure 提供了多个 [VM OS 磁盘类型](../virtual-machines/disks-types.md)。 大多数 VM 系列大小都支持高级和标准存储。 为池选择“s”VM 大小时，Batch 会配置高级 SSD OS 磁盘。 选择“非 s”VM 大小时，会使用更经济的标准 HDD 磁盘类型。 例如，将高级 SSD OS 磁盘用于 `Standard_D2s_v3`，将标准 HDD OS 磁盘用于 `Standard_D2_v3`。

高级 SSD OS 磁盘更昂贵，但性能更高。 采用高级磁盘的 VM 的启动速度可能比采用标准 HDD OS 磁盘的 VM 略快。 在 Batch 中，OS 磁盘通常不会频繁使用，因为应用程序和任务文件位于 VM 的临时 SSD 磁盘中。 因此，你通常可以选择“非 s”VM 大小，以避免为指定“s”VM 大小时预配的高级 SSD 支付增加的成本。

<!--Not Available on ### Reserved virtual machine instances-->
<!--Not Available on [Azure Reservations](../cost-management-billing/reservations/save-compute-costs-reservations.md)-->

### <a name="automatic-scaling"></a>自动缩放

[自动缩放](batch-automatic-scaling.md)根据当前作业的需求来动态缩放 Batch 池中的 VM 数。 自动缩放根据作业的生存期缩放池，确保仅在需要执行某个作业时才扩展和使用 VM。 如果该作业已完成或者没有任何作业，则 VM 会自动缩减以节省计算资源。 借助缩放，可以只使用所需的资源，从而降低 Batch 解决方案的总体成本。

## <a name="next-steps"></a>后续步骤

- 详细了解可用于生成和监视 Batch 解决方案的 [Batch API 和工具](batch-apis-tools.md)。  

<!--Not Available on - Learn about [low-priority VMs with Batch](batch-low-pri-vms.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->