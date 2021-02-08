---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/19/2021
title: 在 Azure 虚拟网络中部署 Azure Databricks（VNet 注入）- Azure Databricks
description: 了解如何在 Azure 虚拟网络中部署 Azure Databricks（VNet 注入）。
ms.openlocfilehash: 3d28c1139f19cff9218afc4a5c8cd974ffd098a1
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058993"
---
# <a name="deploy-azure-databricks-in-your-azure-virtual-network-vnet-injection"></a>在 Azure 虚拟网络中部署 Azure Databricks（VNet 注入）

Azure Databricks 的默认部署是 Azure 上的完全托管服务：所有数据平面资源（包括与所有群集关联的[虚拟网络 (VNet)](/virtual-network/virtual-networks-overview)）都部署到锁定的资源组。 但如果需要自定义网络，则可以在自己的虚拟网络（有时称为 VNet 注入）中部署 Azure Databricks 数据平面资源，以便能够：

* 使用[服务终结点](/virtual-network/virtual-network-service-endpoints-overview)以更安全的方式将 Azure Databricks 连接到其他 Azure 服务（如 Azure 存储）。
* 连接到[本地数据源](on-prem-network.md)以与 Azure Databricks 配合使用，从而利于[用户定义的路由](udr.md)。
* 将 Azure Databricks 连接到[网络虚拟设备](on-prem-network.md#route-via-firewall)以检查所有出站流量并根据允许和拒绝规则执行操作，方法是使用[用户定义的路由](udr.md)。
* 将 Azure Databricks 配置为使用[自定义 DNS](on-prem-network.md#vnet-custom-dns)。
* 配置[网络安全组 (NSG) 规则](/virtual-network/manage-network-security-group)以指定出口流量限制。
* 在现有 VNet 中部署 Azure Databricks 群集。

通过将 Azure Databricks 数据平面资源部署到你自己的 VNet，还可以利用灵活的 CIDR 范围（VNet 的 CIDR 范围为 ``/16``-``/24``，子网的 CIDR 范围最大为 ``/26``）。

> [!IMPORTANT]
>
> 不能替换现有工作区的虚拟网络。 如果当前工作区无法容纳所需数量的活动群集节点，我们建议在较大的虚拟网络中另外创建一个工作区。 按照这些[详细的迁移步骤](/azure-databricks/howto-regional-disaster-recovery#detailed-migration-steps)将资源（笔记本、群集配置、作业）从旧工作区复制到新工作区。

## <a name="virtual-network-requirements"></a><a id="virtual-network-requirements"> </a><a id="vnet-inject-reqs"> </a> 虚拟网络要求

要将 Azure Databricks 工作区部署到的 VNet 必须满足以下要求：

* **区域：** 该 VNet 必须与 Azure Databricks 工作区位于同一区域。
* **订阅：** 该 VNet 必须与 Azure Databricks 工作区属于同一订阅。
* **地址空间：** VNet 的 CIDR 块范围为 ``/16`` 到 ``/24``，两个子网（容器子网和主机子网）的 CIDR 块最大可以为 ``/26``。 若要查看有关基于 VNet 及其子网的大小的最大群集节点数的指导，请参阅[地址空间和最大群集节点数](#max-nodes)。
* **子网：** VNet 必须包括两个专用于 Azure Databricks 工作区的子网：一个容器子网（有时称为专用子网）和一个主机子网（有时称为公共子网）。 但是，对于使用[安全群集连接](../../../security/secure-cluster-connectivity.md)的工作区，容器子网和主机子网都是专用的。 不支持跨工作区共享子网或在 Azure Databricks 工作区使用的子网上部署其他 Azure 资源。 若要查看有关基于 VNet 及其子网的大小的最大群集节点数的指导，请参阅[地址空间和最大群集节点数](#max-nodes)。
  * 如果[使用 Azure 门户创建 Azure Databricks 工作区](#vnet-inject-portal)，则可选择使用 VNet 中的现有子网，或者创建两个通过名称和 IP 范围指定的新子网。
  * 如果使用[全功能 ARM 模板](#arm-all-in-one)或[仅限 VNet 的 ARM 模板](#arm-vnet-only)，则模板会为你创建子网。 在这两种情况下，子网会在工作区部署之前委托给 ``Microsoft.Databricks/workspaces`` 资源提供程序，使 Azure Databricks 能够创建[网络安全组规则](#nsg)。 如果我们需要添加或更新 Azure Databricks 管理的 NSG 规则的范围，Azure Databricks 会提前通知你。
  * 如果使用[工作区 ARM 模板](#arm-workspace)或自定义 ARM 模板，请确保工作区的两个子网使用同一网络安全组并进行了适当的委托。 有关委托说明，请参阅[将“VNet 注入”预览版工作区升级到正式发行版](vnet-inject-upgrade.md#vnet-inject-upgrade)或[添加或删除子网委托](https://docs.microsoft.com/azure/virtual-network/manage-subnet-delegation)。

  > [!IMPORTANT]
  >
  > 这些子网和 Azure Databricks 工作区之间存在一对一关系。 不能在单个子网中共享多个工作区。 不支持跨工作区共享子网或在 Azure Databricks 工作区使用的子网上部署其他 Azure 资源。

若要详细了解用于配置 VNet 和部署工作区的模板，请参阅 [Azure Databricks 提供的 Azure 资源管理器模板](#vnet-inject-advanced)。

### <a name="address-space-and-maximum-cluster-nodes"></a><a id="address-space-and-maximum-cluster-nodes"> </a><a id="max-nodes"> </a>地址空间和最大群集节点数

具有较小虚拟网络的工作区比具有较大虚拟网络的工作区会更快地用完 IP 地址（网络空间）。 可使用的 VNet 的 CIDR 块范围为 ``/16`` 到 ``/24``，两个子网（容器子网和主机子网）的 CIDR 块最大可以为 ``/26``。

VNet 地址空间的 CIDR 范围会影响工作区可以使用的群集节点的最大数目：

* Azure Databricks 工作区需要 VNet 中的两个子网：容器子网（也称为专用子网）和主机子网（也称为公共子网）。 如果工作区使用[安全群集连接](../../../security/secure-cluster-connectivity.md)，则容器子网和主机子网都是专用的。
* Azure [在每个子网中预留 5 个 IP](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)。
* 在每个子网中，Azure Databricks 要求每个群集节点有 1 个 IP 地址。 每个群集节点总共有两个 IP：主机子网中的主机有一个 IP 地址，容器子网中的容器有一个 IP 地址。
* 你可能不希望使用 VNet 的所有地址空间。 例如，你可能想要在一个 VNet 中创建多个工作区。 由于不能跨工作区共享子网，因此你可能希望子网不要用完 VNet 的地址空间。
* 必须为 VNet 地址空间中的两个新子网分配地址空间，并且这个分配的地址空间不得与该 VNet 中当前子网或未来子网的地址空间重叠。

下表显示了基于网络大小的最大子网大小。 此表假定不存在占用地址空间的其他子网。 如果你有预先存在的子网或要为其他子网预留地址空间，请使用较小的子网：

| VNet 地址空间 (CIDR)                          | 最大 Azure Databricks 子网大小 (CIDR)，假定没有其他子网 |
|----------------------------------------------------|-----------------------------------------------------------------------|
| ``/16``                                            | ``/17``                                                               |
| ``/17``                                            | ``/18``                                                               |
| ``/18``                                            | ``/19``                                                               |
| ``/20``                                            | ``/21``                                                               |
| ``/21``                                            | ``/22``                                                               |
| ``/22``                                            | ``/23``                                                               |
| ``/23``                                            | ``/24``                                                               |
| ``/24``                                            | ``/25``                                                               |

若要根据子网大小查找最大群集节点数，请使用下表。 “每个子网的 IP 地址数”列包含 [Azure 预留的 5 个 IP 地址](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)。 最右边的列表示可以同时在已预配了该大小的子网的工作区中运行的群集节点的数目。

| 子网大小 (CIDR)                | 每个子网的 IP 地址数           | 最大 Azure Databricks 群集节点数 |
|-----------------------------------|-----------------------------------|----------------------------------------|
| ``/17``                           | 32768                             | 32763                                  |
| ``/18``                           | 16384                             | 16379                                  |
| ``/19``                           | 8192                              | 8187                                   |
| ``/20``                           | 4096                              | 4091                                   |
| ``/21``                           | 2048                              | 2043                                   |
| ``/22``                           | 1024                              | 1019                                   |
| ``/23``                           | 512                               | 507                                    |
| ``/24``                           | 256                               | 251                                    |
| ``/25``                           | 128                               | 123                                    |
| ``/26``                           | 64                                | 59                                     |

## <a name="create-an-azure-databricks-workspace-using-azure-portal"></a><a id="create-an-azure-databricks-workspace-using-azure-portal"> </a><a id="vnet-inject-portal"> </a>使用 Azure 门户创建 Azure Databricks 工作区

此部分介绍如何在 Azure 门户中创建 Azure Databricks 工作区并将其部署到你现有的 VNet 中。 Azure Databricks 会根据指定的 CIDR 范围，使用两个新的子网（如果尚不存在）来更新 VNet。 该服务还使用新的网络安全组来更新子网，配置入站和出站规则，最后将工作区部署到已更新的 VNet。 如果想要更多地控制 VNet 的配置，请使用 Azure Databricks 提供的 Azure 资源管理器 (ARM) 模板，而不使用门户 UI。 例如，使用现有的网络安全组或创建你自己的安全规则。 请参阅[使用 Azure 资源管理器模板的高级配置](#vnet-inject-advanced)。

> [!IMPORTANT]
>
> 必须为创建工作区的用户分配[“网络参与者”角色](/role-based-access-control/built-in-roles#network-contributor)或[自定义角色](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)（已为其分配 ``Microsoft.Network/virtualNetworks/subnets/join/action`` 操作）。

必须[配置一个 VNet](/virtual-network/)，以便将 Azure Databricks 工作区部署到其中。 可以使用现有的 VNet，也可以创建新的 VNet，但此 VNet 必须与你计划创建的 Azure Databricks 工作区位于同一区域和订阅中。 设置 VNet 大小时，CIDR 范围必须为 /16 到 /24。 有关详细信息，请参阅[虚拟网络要求](#vnet-inject-reqs)。

配置工作区时，可以使用现有子网，也可以使用新子网，但需指定名称和 IP 范围。

1. 在 Azure 门户中，选择“+ 创建资源”>“分析”>“Azure Databricks”或搜索 Azure Databricks，然后单击“创建”或“+ 添加”以启动“Azure Databricks 服务”对话框  。
2. 按照[在你自己的 VNet 中创建 Azure Databricks 工作区](/azure-databricks/quickstart-create-databricks-workspace-vnet-injection)快速入门中所述的配置步骤操作。
3. 在“网络”选项卡中，选择要在“虚拟网络”字段中使用的 VNet。

   > [!IMPORTANT]
   >
   > 如果未在选取器中看到网络名称，请确认为工作区指定的 Azure 区域是否与所需 VNet 的 Azure 区域匹配。

   > [!div class="mx-imgBorder"]
   > ![选择虚拟网络](../../../_static/images/vnet/vnet-injection-workspace.png)

4. 为子网命名，并在其大小最大为 ``/26`` 的块中提供 CIDR 范围。 若要查看有关基于 VNet 及其子网的大小的最大群集节点数的指导，请参阅[地址空间和最大群集节点数](#max-nodes)。
   * 若要指定现有子网，请指定现有子网的确切名称。 使用现有子网时，还应将工作区创建表单中的 IP 范围设置为与现有子网的 IP 范围完全匹配。
   * 若要创建新子网，请指定该 VNet 中尚不存在的子网名称。 将创建具有指定 IP 范围的子网。 你必须指定在 VNet 的 IP 范围内的 IP 范围，不得指定已分配给现有子网的 IP 范围。

   子网会获得关联的网络安全组规则，这些规则包括允许群集内部通信的规则。  Azure Databricks 将拥有通过 ``Microsoft.Databricks/workspaces`` 资源提供程序更新两个子网的委托权限。 这些权限仅适用于 Azure Databricks 所需的网络安全组规则，而不适用于你添加的其他网络安全组规则或所有网络安全组中所包含的默认网络安全组规则。

5. 单击“创建”将 Azure Databricks 工作区部署到 VNet。

   > [!NOTE]
   >
   > 当工作区部署失败时，仍然会创建工作区，但其状态为失败。 删除失败的工作区，并创建一个解决部署错误的新工作区。 删除失败的工作区时，托管资源组和任何成功部署的资源也将被删除。

## <a name="advanced-configuration-using-azure-resource-manager-templates"></a><a id="advanced-configuration-using-azure-resource-manager-templates"> </a><a id="vnet-inject-advanced"> </a>使用 Azure 资源管理器模板的高级配置

如果想要更多地控制对 VNet 的配置，则可以使用以下 Azure 资源管理器 (ARM) 模板，而不使用[基于门户 UI 的自动 VNet 配置和工作区部署](#vnet-inject-portal)。 例如，使用现有的子网、现有的网络安全组，或添加你自己的安全规则。

如果使用自定义 Azure 资源管理器模板或[用于 Azure Databricks VNet 注入的工作区模板](https://azure.microsoft.com/resources/templates/101-databricks-workspace-with-vnet-injection)将工作区部署到现有 VNet，则必须创建主机子网和容器子网，将网络安全组附加到每个子网，并在部署工作区之前将子网委托给 ``Microsoft.Databricks/workspaces`` 资源提供程序 。 部署的每个工作区必须拥有一对单独的子网。

### <a name="all-in-one-template"></a><a id="all-in-one-template"> </a><a id="arm-all-in-one"> </a>全功能模板

若要使用一个模板来创建 VNet 和 Azure Databricks 工作区，请使用[适用于 Azure Databricks VNet 注入工作区的全功能模板](https://azure.microsoft.com/resources/templates/101-databricks-all-in-one-template-for-vnet-injection)。

### <a name="virtual-network-template"></a><a id="arm-vnet-only"> </a><a id="virtual-network-template"> </a>虚拟网络模板

若要使用一个模板来创建具有适当子网的 VNet，请使用[适用于 Databricks VNet 注入的 VNet 模板](https://azure.microsoft.com/resources/templates/101-databricks-vnet-for-vnet-injection)。

### <a name="azure-databricks-workspace-template"></a><a id="arm-workspace"> </a><a id="azure-databricks-workspace-template"> </a>Azure Databricks 工作区模板

若要使用一个模板将 Azure Databricks 工作区部署到现有的 VNet，请使用[适用于 Azure Databricks VNet 注入的工作区模板](https://azure.microsoft.com/resources/templates/101-databricks-workspace-with-vnet-injection)。

使用工作区模板可以指定现有的 VNet 并使用现有的子网：

* 部署的每个工作区必须拥有一对单独的主机/容器子网。 不支持跨工作区共享子网或在 Azure Databricks 工作区使用的子网上部署其他 Azure 资源。
* 在使用此 Azure 资源管理器模板部署工作区之前，VNet 的主机子网和容器子网必须附加网络安全组，并且必须将其委托给 ``Microsoft.Databricks/workspaces`` 服务。
* 若要创建其子网已进行适当委托的 VNet，请使用[适用于 Databricks VNet 注入的 VNet 模板](https://azure.microsoft.com/resources/templates/101-databricks-vnet-for-vnet-injection)。
* 若要在尚未委托主机子网和容器子网的情况下使用现有的 VNet，请参阅[添加或删除子网委托](https://docs.microsoft.com/azure/virtual-network/manage-subnet-delegation)或[将 VNet 注入预览版工作区升级到正式发行版](vnet-inject-upgrade.md#vnet-inject-upgrade)。

## <a name="network-security-group-rules"></a><a id="network-security-group-rules"> </a><a id="nsg"> </a>网络安全组规则

下表显示了 Azure Databricks 使用的最新网络安全组规则。 如果 Azure Databricks 需要添加规则或更改此列表中现有规则的范围，你将收到预先通知。 每当发生此类修改时，本文和各表都会更新。

### <a name="in-this-section"></a>本节内容：

* [Azure Databricks 如何管理网络安全组规则](#how-azure-databricks-manages-network-security-group-rules)
* [2020 年 1 月 13 日之后创建的工作区的网络安全组规则](#network-security-group-rules-for-workspaces-created-after-january-13-2020)
* [2020 年 1 月 13 日之前创建的工作区的网络安全组规则](#network-security-group-rules-for-workspaces-created-before-january-13-2020)

### <a name="how-azure-databricks-manages-network-security-group-rules"></a>Azure Databricks 如何管理网络安全组规则

下面各部分列出的 NSG 规则表示 Azure Databricks 通过将 VNet 的主机子网和容器子网委托给 ``Microsoft.Databricks/workspaces`` 服务，在 NSG 中自动预配和管理的规则。 你无权更新或删除这些 NSG 规则；子网委托会阻止任何更新或删除操作。 Azure Databricks 必须拥有这些规则才能确保 Microsoft 能够在 VNet 中可靠地运行和支持 Azure Databricks 服务。

其中一些 NSG 规则将 VirtualNetwork 指定为源和目标。 在 Azure 中缺少子网级服务标记的情况下，这样做可以简化设计。 所有群集都在内部受到第二层网络策略的保护，这样群集 A 就无法连接到同一工作区中的群集 B。 如果工作区部署到同一个由客户管理的 VNet 中的一对不同的子网中，则这也适用于多个工作区。

> [!IMPORTANT]
>
> 如果工作区 VNet 与另一个由客户管理的网络对等互连，或者在其他子网中预配了非 Azure Databricks 资源，Databricks 建议你向附加到其他网络和子网的 NSG 添加“拒绝”入站规则，以阻止来自 Azure Databricks 群集的源流量。 你不需要为希望 Azure Databricks 群集连接到的资源添加此类规则。

### <a name="network-security-group-rules-for-workspaces-created-after-january-13-2020"></a>2020 年 1 月 13 日之后创建的工作区的网络安全组规则

下表仅适用于 2020 年 1 月 13 日之后创建的 Azure Databricks 工作区。 如果工作区是在 2020 年 1 月 13 日发布安全群集连接 (SCC) 之前创建的，请参阅下表。

> [!IMPORTANT]
>
> 下表包含两个仅在禁用[安全群集连接 (SCC)](../../../security/secure-cluster-connectivity.md) 的情况下包含的入站安全组规则。

| 方向     | 协议     | 源                                                       | Source Port     | 目标                      | Dest Port     | 已使用          |
|---------------|--------------|--------------------------------------------------------------|-----------------|----------------------------------|---------------|---------------|
| 入站       | 任意          | VirtualNetwork                                               | 任意             | VirtualNetwork                   | 任意           | 默认       |
| 入站       | TCP          | AzureDatabricks（服务标记）<br>仅当 SCC 已禁用 | 任意             | VirtualNetwork                   | 22            | 公共 IP     |
| 入站       | TCP          | AzureDatabricks（服务标记）<br>仅当 SCC 已禁用 | 任意             | VirtualNetwork                   | 5557          | 公共 IP     |
| 出站      | TCP          | VirtualNetwork                                               | 任意             | AzureDatabricks（服务标记）    | 443           | 默认       |
| 出站      | TCP          | VirtualNetwork                                               | 任意             | SQL                              | 3306          | 默认       |
| 出站      | TCP          | VirtualNetwork                                               | 任意             | 存储                          | 443           | 默认       |
| 出站      | 任意          | VirtualNetwork                                               | 任意             | VirtualNetwork                   | 任意           | 默认       |
| 出站      | TCP          | VirtualNetwork                                               | 任意             | EventHub                         | 9093          | 默认       |

### <a name="network-security-group-rules-for-workspaces-created-before-january-13-2020"></a>2020 年 1 月 13 日之前创建的工作区的网络安全组规则

下表仅适用于 2020 年 1 月 13 日之前创建的 Azure Databricks 工作区。 如果工作区是在 2020 年 1 月 13 日当天或之后创建的，请参阅上表。

| 方向     | 协议     | 源              | Source Port     | 目标        | Dest Port     | 已使用          |
|---------------|--------------|---------------------|-----------------|--------------------|---------------|---------------|
| 入站       | 任意          | VirtualNetwork      | 任意             | VirtualNetwork     | 任意           | 默认       |
| 入站       | TCP          | ControlPlane IP     | 任意             | VirtualNetwork     | 22            | 公共 IP     |
| 入站       | TCP          | ControlPlane IP     | 任意             | VirtualNetwork     | 5557          | 公共 IP     |
| 出站      | TCP          | VirtualNetwork      | 任意             | Webapp IP          | 443           | 默认       |
| 出站      | TCP          | VirtualNetwork      | 任意             | SQL                | 3306          | 默认       |
| 出站      | TCP          | VirtualNetwork      | 任意             | 存储            | 443           | 默认       |
| 出站      | 任意          | VirtualNetwork      | 任意             | VirtualNetwork     | 任意           | 默认       |
| 出站      | TCP          | VirtualNetwork      | 任意             | EventHub           | 9093          | 默认       |

> [!IMPORTANT]
>
> Azure Databricks 是部署在全局 Azure 公有云基础结构上的 Microsoft Azure 第一方服务。 服务组件之间的所有通信（包括控制平面和客户数据平面中的公共 IP 之间的通信）都留在 Microsoft Azure 网络主干内进行。 另请参阅 [Microsoft 全球网络](/networking/microsoft-global-network)。

## <a name="troubleshooting"></a><a id="troubleshooting"> </a><a id="vnet-inject-troubleshoot"> </a>故障排除

### <a name="workspace-creation-errors"></a>工作区创建错误

子网<subnet ID>需要以下任意委托 [Microsoft.Databricks/workspaces] 来引用服务关联链接

可能的原因：创建工作区时所在的 VNet 的主机子网和容器子网尚未委托给 ``Microsoft.Databricks/workspaces`` 服务。 每个子网必须附加并正确委托网络安全组。 有关详细信息，请参阅[虚拟网络要求](#vnet-inject-reqs)。

子网<subnet ID>已被工作区<workspace ID>占用

可能的原因：创建工作区时所在的 VNet 的主机子网和容器子网已经被现有 Azure Databricks 工作区占用。 不能在单个子网中共享多个工作区。 部署的每个工作区必须拥有一对新的主机子网和容器子网。

### <a name="troubleshooting"></a>故障排除

**实例不可访问：无法通过 SSH 访问资源。**

可能的原因：阻止了从控制平面到辅助角色的流量。 如果要部署到已连接到本地网络的现有 VNet，请根据[将 Azure Databricks 工作区连接到本地网络](on-prem-network.md)中提供的信息检查安装情况。

**启动意外失败：设置群集时遇到意外错误。请重试，如果问题仍然存在，请联系 Azure Databricks。内部错误消息：``Timeout while placing node``。**

可能的原因：从辅助角色到 Azure 存储终结点的流量被阻止。 如果使用自定义 DNS 服务器，请同时检查 VNet 中 DNS 服务器的状态。

**云提供程序启动失败：在设置群集时遇到云提供程序错误。请参阅 Azure Databricks 指南以获取详细信息。Azure 错误代码：``AuthorizationFailed/InvalidResourceReference.``**

可能的原因：VNet 或子网不再存在。 请确保 VNet 和子网存在。

**群集已终止。原因:Spark 启动失败：Spark 未能及时启动。此问题可能是由 Hive 元存储发生故障、Spark 配置无效或初始化脚本出现故障而导致的。请参阅 Spark 驱动程序日志以解决此问题，如果问题仍然存在，请联系 Databricks。内部错误消息：``Spark failed to start: Driver failed to start in time``。**

可能的原因：容器无法与托管实例或 DBFS 存储帐户通信。 解决方法是为 DBFS 存储帐户添加指向子网的自定义路由，并将下一个跃点设为 Internet。