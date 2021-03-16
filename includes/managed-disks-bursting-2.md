---
title: include 文件
description: include 文件
services: virtual-machines
ms.service: virtual-machines
ms.topic: include
origin.date: 04/27/2020
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: d0b365f36298d24c116387cf04262881500b51aa
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055224"
---
<!--Verified successfully-->
<!--CONFIRM THE DEOPLOMENT REGIONS BEFORE RELEASEMENT-->
## <a name="common-scenarios"></a>常见场景
以下需求场景可显著受益于突发：
- **缩短启动时间** - 在使用突发的情况下，实例的启动速度将大大加快。 例如，启用高级层的 VM 的默认 OS 磁盘是 P4 磁盘，其预配性能最高可达 120 IOPS 和 25 MB/s。 在使用突发的情况下，P4 的性能最高可达 3500 IOPS 和 170 MB/s，可使启动速度加快 6 倍。
- **处理批处理作业** - 某些应用程序的工作负荷是周期性的，在大部分时间内需要基线性能，仅在很短的时间段内需要较高的性能。 这种情况的一个示例是一个会计程序，该程序每日处理需要少量磁盘流量的事务。 然后，在月末，该程序将运行需要大量磁盘流量的对帐报表。
- **为流量高峰做准备** - Web 服务器及其应用程序随时都可能遇到流量激增。 如果 Web 服务器由使用突发的 VM 或磁盘提供支持，那么这些服务器便可以更好地处理流量高峰。 

## <a name="bursting-flow"></a>突发流
突发额度系统以相同的方式应用于虚拟机级别和磁盘级别。 你的资源（VM 或磁盘）将从充分储备的额度开始。 有了这些额度，便能够以最大的突发速率进行 30 分钟的突发。 当资源在其性能磁盘存储限制下运行时，突发额度会进行累积。 如果资源使用的所有 IOPS 和 MB/s 低于性能限制，额度将开始累积。 如果资源已经积累了要用于突发的额度，并且你的工作负荷需要额外的性能，则资源可以使用这些额度超出你的性能限制，为工作负荷提供所需的磁盘 IO 性能以满足其需求。

![突发桶关系图](media/managed-disks-bursting/bucket-diagram.jpg)

这完全取决于你希望如何使用这 30 分钟的突发。 可以连续使用 30 分钟，也可以在一天内分散地使用。 部署产品后，该产品为满额度；当其额度耗尽时，可在一天内再次充满额度。 你可以自行决定如何累积和花费其突发信用额度，不一定要再次充满 30 分钟的 Bucket 才能突发。 有关突发累积，需要注意的一件事是，各个资源的突发累积都各不相同，因为它基于资源在其性能限制下运行时未使用的 IOPS 和 MB/s。 这意味着，较高基线性能的产品积累其突发额度的速度可能会快于较低基线性能的产品。 例如，处于空闲状态、没有任何活动的 P1 磁盘将每秒积累 120 IOPS，而 P20 磁盘在处于空闲状态、没有任何活动时将每秒积累 2300 IOPS。

## <a name="bursting-states"></a>突发状态
启用了突发功能时，资源可能处于以下三种状态之一：
- **正在积累** - 资源正在使用的 IO 流量低于性能目标。 为 IOPS 和 MB/s 累积额度是彼此分开执行的。 你的资源可能会积累 IOPS 额度并支出 MB/s 额度，也可能会支出 IOPS 额度并积累 MB/s 额度。
- **正在突发** - 资源正在使用的流量高于性能目标。 突发流量将独立消耗 IOPS 或带宽的额度。
- **恒定** - 资源的流量与性能目标完全相同。

## <a name="examples-of-bursting"></a>有关突发的示例

以下示例显示了在使用各种 VM 和磁盘组合时突发的工作情况。 为了使示例易于理解，我们将重点放在 MB/s 上，但相同的逻辑也独立适用于 IOPS。

### <a name="non-burstable-virtual-machine-with-burstable-disks"></a>具有可突发磁盘的非可突发虚拟机
**VM 和磁盘组合：** 
- Standard_D8s_v3
    
    <!--CORRECT ON Standard_D8s_v3-->
    
    - 未缓存的 MB/s：192
- P4 OS 磁盘
    - 预配的 MB/s：25
    - 最大突发 MB/s：170 
- 2 个 P10 数据磁盘 
    - 预配的 MB/s：100
    - 最大突发 MB/s：170

 当 VM 启动时，它从 OS 磁盘中检索数据。 由于 OS 磁盘是正在启动的 VM 的一部分，因此 OS 磁盘的突发额度将是充分的。 这些额度使 OS 磁盘以 170 MB/s 的速度突发启动。

![VM 将 192 MB/s 的吞吐量请求发送到 OS 磁盘，OS 磁盘以 170 MB/s 数据进行响应。](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-startup.jpg)

启动完成后，应用程序将在 VM 上运行，并且具有非关键工作负荷。 此工作负荷需要 15 MB/S（在所有磁盘上均匀分布）。

![应用程序向 VM 发送 15 MB/s 的吞吐量请求，VM 接收请求并向其每个磁盘发送 5 MB/s 的请求，每个磁盘返回 5 MB/s，VM 将 15 MB/s 返回到应用程序。](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-idling.jpg)

然后，应用程序需要处理一个批处理作业，该作业需要 192 MB/s。 2 MB/s 由 OS 磁盘使用，其余部分在数据磁盘之间平均划分。

![应用程序将 192 MB/s 的吞吐量请求发送到 VM，VM 接收请求，并将其大部分请求发送到数据磁盘（每个 95 MB/s）并将 2 MB/s 发送到 OS 磁盘，数据磁盘突发以满足需求，并且所有磁盘都将请求的吞吐量返回给 VM，后者将其返回给应用程序。](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-bursting.jpg)

<!--Not Avaialble on ### Burstable virtual machine with non-burstable disks-->
<!--Not Available on Standard_L8s_v2-->
<!--Not Avaialble on ### Burstable virtual machine with burstable Disks-->
<!--Not Available on Standard_L8s_v2-->
<!--Update_Description: update meta properties, wording update, update link-->