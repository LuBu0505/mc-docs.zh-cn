---
title: Azure 中 VM 的生命周期和状态
description: 概述了 Azure 中的 VM 的生命周期，并介绍了 VM 在任意时刻可能处于的各种状态。
services: virtual-machines
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
origin.date: 08/09/2018
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 4638d9ea0f0a18a81dd8fcb6fbf1913c5b58fe66
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054373"
---
<!--Verified Successsfully-->
# <a name="virtual-machines-lifecycle-and-states"></a>虚拟机生命周期和状态

Azure 虚拟机 (VM) 经历的不同状态可以归类为“预配”状态和“电源”状态。 本文旨在介绍这些状态并专门突出显示了何时会对客户收取实例使用费用。 

## <a name="power-states"></a>电源状态

电源状态表示 VM 的上一个已知状态。

:::image type="content" source="./media/virtual-machines-common-states-lifecycle/vm-power-states.png" alt-text="VM 电源状态图":::

<br />
下表描述每个实例状态并指示是否会对其收取实例使用费用。

:::row:::
   :::column span="":::

   **State**

   :::column-end:::
   :::column span="":::

   **说明**

   :::column-end:::
   :::column span="":::

   **实例使用情况计费**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **正在启动**

   :::column-end:::
   :::column span="":::

   VM 正在启动。

   ```json
   "statuses": [
    {
    "code": "PowerState/starting",
    "level": "Info",
    "displayStatus": "VM starting"
    }
   ]
   ```
   :::column-end:::
   :::column span="":::

   **不计费**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **正在运行**

   :::column-end:::
   :::column span="":::

   VM 的正常工作状态

   ```json
   "statuses": [
    {
    "code": "PowerState/running",
    "level": "Info",
    "displayStatus": "VM running"
    }
  ]
  ```
   :::column-end:::
   :::column span="":::

   **计费**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **正在停止**

   :::column-end:::
   :::column span="":::

   这是一种过渡性状态。 完成后，会显示为“已停止”。

   ```json
   "statuses": [
    {
    "code": "PowerState/stopping",
    "level": "Info",
    "displayStatus": "VM stopping"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **计费**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **已停止**

   :::column-end:::
   :::column span="":::

   VM 已在来宾 OS 中关闭，或者已使用 PowerOff API 关闭。

   硬件仍然分配给 VM 并保留在主机上。

   ```json
   "statuses": [
    {
    "code": "PowerState/stopped",
    "level": "Info",
    "displayStatus": "VM stopped"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **已计费***

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **正在解除分配**

   :::column-end:::
   :::column span="":::

   过渡性状态。 完成后，VM 会显示为“已解除分配”。

   ```json
   "statuses": [
    {
    "code": "PowerState/deallocating",
    "level": "Info",
    "displayStatus": "VM deallocating"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **未计费***

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **已解除分配**

   :::column-end:::
   :::column span="":::

   VM 已成功停止并从主机中删除。

   ```json
   "statuses": [
    {
    "code": "PowerState/deallocated",
    "level": "Info",
    "displayStatus": "VM deallocated"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **不计费**

   :::column-end:::
:::row-end:::

&#42; 某些 Azure 资源（如磁盘和网络）会产生费用。 实例上的软件许可证不产生费用。

## <a name="provisioning-states"></a>预配状态

预配状态是用户在 VM 上启动的控制平面操作的状态。 以下状态独立于 VM 的电源状态。

- **创建** - 创建 VM。

- **更新** - 更新现有 VM 的模型。 某些针对 VM 的非模型更改（例如“启动/重启”）也属于更新。

- **删除** - 删除 VM。

- **解除分配** - 是指停止 VM 并将其从主机中删除。 可以将 VM 的解除分配视为一种更新，因此它会显示与更新相关的预配状态。

下面是在平台接受用户启动的操作之后的过渡操作状态：

:::row:::
   :::column span="":::

   **State**

   :::column-end:::
   :::column span="2":::

   **说明**

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Creating**

   :::column-end:::
   :::column span="2":::

  ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating",
    "level": "Info",
    "displayStatus": "Creating"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **更新**

   :::column-end:::
   :::column span="2":::

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/updating",
    "level": "Info",
    "displayStatus": "Updating"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **正在删除**

   :::column-end:::
   :::column span="2":::

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/deleting",
    "level": "Info",
    "displayStatus": "Deleting"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **OS 预配状态**

   :::column-end:::
   :::column span="2":::

   **说明**

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   :::column-end:::
   :::column span="2":::

   如果 VM 是使用 OS 映像而非专用映像创建的，可能会观察到以下子状态：

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **OSProvisioningInprogress**

   :::column-end:::
   :::column span="2":::

   VM 正在运行，来宾 OS 的安装正在进行。

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating/OSProvisioningInprogress",
    "level": "Info",
    "displayStatus": "OS Provisioning In progress"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **OSProvisioningComplete**

   :::column-end:::
   :::column span="2":::

   短时状态。 VM 会快速过渡到“成功”状态，除非需要安装扩展。 安装扩展可能需要一定的时间。

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating/OSProvisioningComplete",
    "level": "Info",
    "displayStatus": "OS Provisioning Complete"
    }
   [
   ```

   **注意**：如果存在 OS 故障或者 OS 没有及时安装，则 OS 预配可能会过渡到“失败”状态。 会根据部署在基础结构上的 VM 对客户收费。

   :::column-end:::

:::row-end:::

操作完成后，VM 会过渡到下述某个状态：

- **成功** - 用户启动的操作已完成。

    ```
  "statuses": [ 
  {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
  }
  ]
    ```

- **失败** - 表示操作失败。 若要获取详细信息和可能的解决方案，请查看错误代码。

    ```
  "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```

## <a name="vm-instance-view"></a>VM 实例视图

实例视图 API 提供 VM 运行状态信息。 有关详细信息，请参阅 [Virtual Machines - Instance View](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview)（虚拟机 - 实例视图）API 文档。

<!--NOT AVAILABLE ON [Resource Explorer](https://resources.azure.com/)-->

预配状态在 VM 属性和实例视图中可见。 电源状态在 VM 的实例视图中提供。

若要检索订阅中所有 VM 的电源状态，请使用[虚拟机 - 列出所有 API](https://docs.microsoft.com/rest/api/compute/virtualmachines/listall)，并将参数 statusOnly 设置为 true。

<!--NOT AVAILABLE ON ## Next steps-->
<!--NOT AVAILABLE ON [Monitor virtual machines in Azure](../azure-monitor/vm/monitor-vm-azure.md)-->
<!--Update_Description: update meta properties, wording update, update link-->