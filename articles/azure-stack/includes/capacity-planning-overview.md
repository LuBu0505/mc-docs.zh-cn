---
author: WenJason
ms.topic: include
origin.date: 02/04/2021
ms.date: 03/01/2021
ms.author: v-jay
ms.openlocfilehash: 99c172394130c21ae88866c54bb0c604b6e12a9a
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697909"
---
评估 Azure Stack Hub 解决方案时，应考虑硬件配置选择，因为它直接影响到 Azure Stack Hub 云的总体容量。

例如，需要就 CPU、内存密度、存储配置和总体解决方案规模或服务器数目进行选择。 但是，确定可用容量将不同于传统的虚拟化解决方案，因为某些容量已在使用中。 构建 Azure Stack Hub 是为了在解决方案自身内部托管基础结构或管理组件。 另外，解决方案的某些容量保留用于支持复原。 复原的定义是：更新解决方案的软件时，必须将租户工作负载的中断降到最低程度。

> [!IMPORTANT]
> 可以从此容量规划信息和 [Azure Stack Hub Capacity Planner](https://download.microsoft.com/download/4/a/2/4a2bd10c-58a0-4ee7-8c3f-55b526ce7e75/AzureStackHubCapacityPlanner_v2005.01.xlsm) 着手，进行 Azure Stack Hub 规划和配置决策。 该信息不能取代你自己的调查和分析。 Azure 对于此处提供的信息不做任何明示或默示的声明或保证。

## <a name="hyperconvergence-and-the-scale-unit"></a>超融合性和缩放单元

Azure Stack Hub 解决方案是作为超融合群集构建的，包含计算和存储。 可以通过超融合性来共享群集中的硬件容量（称为“缩放单元”）。 在 Azure Stack Hub 中，缩放单元为资源提供可用性和可伸缩性。 缩放单元包含一组称为“主机”的 Azure Stack Hub 服务器。 基础结构软件托管在一组虚拟机 (VM) 中，与租户 VM 共享相同的物理服务器。 然后所有 Azure Stack Hub VM 可以通过缩放单元的 Windows Server 群集技术和独立的 Hyper-V 实例来管理。

缩放单元简化了 Azure Stack Hub 的获取和管理。 缩放单元还可以跨 Azure Stack Hub 移动和缩放所有服务（租户和体系结构）。
