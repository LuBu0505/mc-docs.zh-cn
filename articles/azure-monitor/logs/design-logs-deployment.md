---
title: 设计 Azure Monitor 日志部署 | Microsoft Docs
description: 本文介绍有关客户在 Azure Monitor 中准备部署工作区时的注意事项和建议。
ms.subservice: ''
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 02/20/2021
origin.date: 09/20/2019
ms.openlocfilehash: 4454f5a7e647c28cddae71937a8e992fde76f015
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102205844"
---
# <a name="designing-your-azure-monitor-logs-deployment"></a>设计 Azure Monitor 日志部署

Azure Monitor 将[日志](data-platform-logs.md)数据存储在 Log Analytics 工作区中。该工作区是一个 Azure 资源，也是一个用于收集和聚合数据的容器，充当管理边界。 尽管可以在 Azure 订阅中部署一个或多个工作区，但为了确保初始部署遵循我们的指导原则来提供经济高效、易管理、可缩放且符合组织需求的部署，应考虑多种因素。

工作区中的数据组织成表，每个表存储不同类型的数据，根据生成数据的资源，它还具有自身独特的属性集。 大多数数据源将数据写入到其各自在 Log Analytics 工作区中的表内。

![工作区数据模型示例](./media/design-logs-deployment/logs-data-model-01.png)

Log Analytics 工作区可提供：

* 数据存储的地理位置。
* 遵循建议的设计策略之一授予不同的用户访问权限，以实现数据隔离。
* 设置配置的范围，例如[定价层级](../platform/manage-cost-storage.md#changing-pricing-tier)、[保留期](../platform/manage-cost-storage.md#change-the-data-retention-period)和[数据上限](../platform/manage-cost-storage.md#manage-your-maximum-daily-data-volume)。

工作区托管在物理群集上。 默认情况下，系统会创建和管理这些群集。 每天引入的数据超过 4TB 的客户应创建其自己的专用于工作区的群集，这样可以更好地进行控制并提高引入速率。

本文提供设计和迁移注意事项的详细概述、访问控制概述，我们为 IT 组织推荐的设计实施方案的介绍。



## <a name="important-considerations-for-an-access-control-strategy"></a>访问控制策略的重要注意事项

以下一项或多项要求会影响所需工作区数量的选择：

* 贵公司是全球性公司，因数据所有权和合规性需要将日志数据存储于特定区域。
* 正在使用 Azure，并希望通过让工作区与它所管理的 Azure 资源位于同一区域，避免产生出站数据传输费用。
* 你要管理多个部门或业务组，并希望每个部门或业务组能够看到自己的数据，但看不到其他部门的数据。 此外，在整合部门或业务组视图方面没有业务要求。

当今的 IT 组织会遵循集中式、分散式或介于两者之间的混合结构建模。 因此，经常使用以下工作区部署模型映射到其中一个组织结构：

* **集中式**：所有日志存储在中心工作区并由单个团队进行管理，Azure Monitor 为每个团队提供差异访问权限。 在此方案中，可以轻松管理和搜索各个资源，以及交叉关联日志。 根据从订阅中的多个资源收集的数据量，工作区可能会明显增大，对不同的用户保持访问控制会增大管理开销。 此模型称为“中心和分支”。
* **分散式**：每个团队在其拥有和管理的资源组中创建自己的工作区，日志数据按资源隔离。 在此方案中，工作区可以保持安全性，访问控制与资源访问权限保持一致，但难以交叉关联日志。 需要大范围查看多种资源的用户无法以有效的方式分析数据。
* **混合**：安全审核合规性要求进一步使此方案变得复杂，因为许多组织同时实施这两种部署模型。 这通常会导致复杂、大开销且难以维护的配置，并使日志覆盖范围出现差异。

使用 Log Analytics 代理收集数据时，需要了解以下各项以规划代理部署：

* 若要从 Windows 代理收集数据，可[将每个代理配置为向一个或多个工作区报告](./../agents/agent-windows.md)。 Windows 代理最多可向四个工作区报告。
* Linux 代理不支持多宿主，只能向一个工作区报告。

<!-- Not available in MC: System Center Operations Manager -->
## <a name="access-control-overview"></a>访问控制概述

使用 Azure 基于角色的访问控制 (Azure RBAC)，可以仅向用户和组授予其在工作区中处理监视数据所需的访问级别。 这样，便可以使用单个工作区来存储对所有资源启用的收集数据，从而符合 IT 组织的操作模型。 例如，可向负责管理 Azure 虚拟机 (VM) 上托管的基础结构服务的团队授予访问权限，因此，他们只能访问这些 VM 生成的日志。 此方案遵循我们的新资源上下文日志模型。 此模型的基础适用于 Azure 资源发出的每条日志记录，自动与此资源相关联。 日志将根据资源转发到与范围和 Azure RBAC 相符的中心工作区。

用户有权访问的数据由下表中列出的因素组合决定。 后续部分会描述每种因素。

| 因子 | 说明 |
|:---|:---|
| [访问模式](#access-mode) | 用户访问工作区的方法。  定义可用数据的范围，以及应用的访问控制模式。 |
| [访问控制模式](#access-control-mode) | 工作区中的设置，用于定义是要在工作区级别还是资源级别应用权限。 |
| [权限](../platform/manage-access.md) | 应用到工作区或资源的个人用户或用户组的权限。 定义用户有权访问哪些数据。 |
| [表级别 Azure RBAC](../platform/manage-access.md#table-level-azure-rbac) | 应用到所有用户（无论他们使用的是访问模式还是访问控制模式）的可选精细权限。 定义用户可以访问哪些数据类型。 |

## <a name="access-mode"></a>访问模式

访问模式是指用户如何访问 Log Analytics 工作区，并定义他们有权访问的数据范围。 

用户可通过两个选项访问数据：

* **工作区上下文**：你可以查看你有权访问的工作区中的所有日志。 在此模式下，只能查询该工作区中所有表内的所有数据。 使用工作区作为范围来访问日志时（例如，在 Azure 门户上的“Azure Monitor”菜单中选择“日志”时），将使用此访问模式 。

    ![工作区中的 Log Analytics 上下文](./media/design-logs-deployment/query-from-workspace.png)

* **资源上下文**：访问特定资源、资源组或订阅的工作区时（例如，在 Azure 门户上的资源菜单中选择“日志”时），只能查看所有表中你有权访问的资源的日志。 在此模式下，只能查询与该资源关联的数据。 此模式还支持精细 Azure RBAC。

    ![资源中的 Log Analytics 上下文](./media/design-logs-deployment/query-from-resource.png)

    > [!NOTE]
    > 仅当日志已适当地关联到相关资源时，才能对日志进行资源上下文查询。 目前，以下资源存在限制：
    > - Azure 外部的计算机
    > - Service Fabric
    > - Application Insights
    >
    > 可以通过运行一个查询并检查所需的记录，来测试日志是否已适当关联到其资源。 如果 [_ResourceId](../platform/log-standard-columns.md#_resourceid) 属性中包含正确的资源 ID，则可以对数据进行以资源为中心的查询。

Azure Monitor 根据执行日志搜索时所在的上下文自动确定正确的模式。 范围始终显示在 Log Analytics 的左上部分。

### <a name="comparing-access-modes"></a>比较访问模式

下表汇总了访问模式：

| 问题 | 工作区上下文 | 资源上下文 |
|:---|:---|:---|
| 每种模式适合哪类用户？ | 集中管理。 需要配置数据收集的管理员，以及需要访问各种资源的用户。 此外，需要访问 Azure 外部资源的日志的用户目前也需要使用此模式。 | 应用程序团队。 受监视 Azure 资源的管理员。 |
| 用户需要哪些权限才能查看日志？ | 对工作区的权限。 请参阅 [使用工作区权限管理访问权限](../platform/manage-access.md#manage-access-using-workspace-permissions)中的 **工作区权限**。 | 对资源的读取访问权限。 请参阅 [使用 Azure 权限管理访问权限](../platform/manage-access.md#manage-access-using-azure-permissions)中的 **资源权限**。 权限可以继承（例如，从包含资源组继承），也可以直接分配给资源。 系统会自动分配对资源日志的权限。 |
| 权限范围是什么？ | 工作区。 有权访问工作区的用户可以通过他们有权访问的表查询该工作区中的所有日志。 请参阅[表访问控制](../platform/manage-access.md#table-level-azure-rbac) | Azure 资源。 用户可以通过任何工作区查询他们有权访问的资源、资源组或订阅的日志，但无法查询其他资源的日志。 |
| 用户如何访问日志？ | <ul><li>从“Azure Monitor”菜单启动“日志”。 </li></ul> <ul><li>从“Log Analytics 工作区”启动“日志”。 </li></ul> <ul><li>从 Azure Monitor [工作簿](../visualizations.md#workbooks)。</li></ul> | <ul><li>从 Azure 资源的菜单启动“日志”</li></ul> <ul><li>从“Azure Monitor”菜单启动“日志”。 </li></ul> <ul><li>从“Log Analytics 工作区”启动“日志”。 </li></ul> <ul><li>从 Azure Monitor [工作簿](../visualizations.md#workbooks)。</li></ul> |

## <a name="access-control-mode"></a>访问控制模式

访问控制模式是每个工作区中的一项设置，定义如何确定该工作区的权限。

* **需要工作区权限**：此控制模式不允许精细 Azure RBAC。 用户若要访问工作区，必须获得对该工作区或特定表的权限。

    如果用户遵循工作区上下文模式访问工作区，将可以访问他们有权访问的任何表中的所有数据。 如果用户遵循资源上下文模式访问工作区，则只能访问他们有权访问的任何表中该资源的数据。

    这是在 2019 年 3 月之前创建的所有工作区的默认设置。

* **使用资源或工作区权限**：此控制模式允许精细 Azure RBAC。 可以通过分配 Azure `read` 权限，仅向用户授予与他们可查看的资源相关联的数据的访问权限。 

    当用户以工作区上下文模式访问工作区时，将应用工作区权限。 当用户以资源上下文模式访问工作区时，只会验证资源权限，而会忽略工作区权限。 要为用户启用 Azure RBAC，可将其从工作区权限中删除，并允许识别其资源权限。

    这是在 2019 年 3 月之后创建的所有工作区的默认设置。

    > [!NOTE]
    > 如果用户只对工作区拥有资源权限，则他们只能使用资源上下文模式访问工作区（假设工作区访问模式设置为“使用资源或工作区权限”）。

若要了解如何使用门户、PowerShell 或资源管理器模板更改访问控制模式，请参阅[配置访问控制模式](manage-access.md#configure-access-control-mode)。

## <a name="scale-and-ingestion-volume-rate-limit"></a>规模和引入量速率限制

Azure Monitor 是一种大规模数据服务，每月为成千上万的客户发送数 PB 的数据，该数据量仍在不断增长。 工作区不受其存储空间的限制，可以增长到存储数 PB 的数据。 由于规模的原因，无需拆分工作区。

为了保护和隔离 Azure Monitor 客户及其后端基础结构，我们有一个默认的引入速率限制，该限制旨在防止出现峰值和泛洪情况。 速率限制默认值大约为“6 GB/分钟”，旨在实现正常引入。 有关引入量限制度量的更多详细信息，请参阅 [Azure Monitor 服务限制](../service-limits.md#data-ingestion-volume-rate)。

每天的引入量少于 4TB 的客户通常不会达到这些限制。 如果客户在正常操作中引入了更多的量或出现峰值，该客户应考虑转移到[专用群集](../log-query/logs-dedicated-clusters.md)，在那里可以提高引入速率限制。

当引入速率限制被激活或达到阈值的 80% 时，系统会向工作区中的“操作”表添加一个事件。 建议对其进行监视并创建警报。 有关更多详细信息，请参阅[数据引入量速率](../service-limits.md#data-ingestion-volume-rate)。


## <a name="recommendations"></a>建议

![资源上下文设计示例](./media/design-logs-deployment/workspace-design-resource-context-01.png)

此方案涉及到 IT 组织订阅中的单个工作区设计，该设计不受数据主权或合规性的约束，或者需要映射到部署资源的区域。 此方案可让组织的安全和 IT 管理团队利用与 Azure 访问管理的改进集成以及更安全的访问控制。

支持由不同团队维护的基础结构和应用程序的所有资源、监视解决方案和见解（例如 Application Insights 和用于 VM 的 Azure Monitor）将配置为向 IT 组织的集中式共享工作区转发收集的日志数据。 为每个团队的用户授予其已有权访问的资源的日志访问权限。

部署工作区体系结构后，可以使用 [Azure Policy](../../governance/policy/overview.md) 对 Azure 资源强制实施此方案。 此方案可让你定义策略并确保 Azure 资源合规，因此它们将其所有资源日志发送到特定的工作区。 例如，使用 Azure 虚拟机或虚拟机规模集时，可以使用现有的策略来评估工作区合规性和报告结果，或者自定义策略，以便在不合规的情况下予以补救。  

## <a name="workspace-consolidation-migration-strategy"></a>工作区整合迁移策略

对于已部署多个工作区并对整合到资源上下文访问模型感兴趣的客户，我们建议采用增量方法迁移到建议的访问模型，而不要尝试快速或以激进方式实现此目的。 根据合理的时间线以分阶段的方法进行规划、迁移、验证和停用，将有助于避免任何计划外事件或者对云操作造成意外影响。 如果出于合规或业务原因而没有制定数据保留策略，则在迁移过程中，需要评估在从中迁移的工作区中保留数据的适当时间长短。 将资源重新配置为向共享工作区报告时，仍可根据需要分析原始工作区中的数据。 完成迁移后，如果在保留期结束之前需要根据监管要求在原始工作区中保留数据，请不要删除该工作区。

规划迁移到此模型时，请注意以下事项：

* 了解必须遵守的有关数据保留的行业法规和内部政策。
* 确保应用程序团队可在现有的资源上下文功能范围内工作。
* 确定为应用程序团队授予的资源访问权限，并先在开发环境中进行测试，然后在生产环境中实施。
* 将工作区配置为启用“使用资源或工作区权限”。
* 删除应用程序团队的工作区读取和查询权限。
* 启用并配置原始工作区中部署的任何监视解决方案、见解（例如用于容器的 Azure Monitor 和/或用于 VM 的 Azure Monitor）、自动化帐户和管理解决方案（例如更新管理、启动/停止 VM 等）。

## <a name="next-steps"></a>后续步骤

若要实施本指南中建议的安全权限和控制措施，请查看[管理对日志的访问权限](../platform/manage-access.md)。
