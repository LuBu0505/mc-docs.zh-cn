---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/19/2021
title: Azure Databricks 的企业安全性 - Azure Databricks
description: 了解典型的公司如何保护 Azure Databricks 工作区。
ms.openlocfilehash: 20041be8004ba6eb53fdd335e87ff709b116337a
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060676"
---
# <a name="enterprise-security-for-azure-databricks"></a>Azure Databricks 的企业安全性

本文概述了部署 Azure Databricks 最重要的安全相关控件和配置。

本文通过示例公司对某些场景进行介绍，以比较小型和大型组织处理部署的方式有何差异。 在本文中，LargeCorp 指代大型公司，SmallCorp 指代小型公司 。 请将这些示例用作一般性指南，但每个公司都是不同的。 如有疑问，请联系 Azure Databricks 代表。

有关特定安全功能的详细信息，请参阅[安全性和隐私](index.md)。 Azure Databricks 代表还可以提供其他安全性与合规性文档。

## <a name="account-plan"></a>帐户计划

与 Azure Databricks 代表联系讨论你需要的功能。 他们将帮助你选择适合你需求的[定价计划（层）](https://databricks.com/product/azure-pricing)。

以下功能通常适合注重安全的组织。 除 SSO 之外都需要高级计划。

* [单一登录 (SSO)](../administration-guide/users-groups/single-sign-on/index.md)：使用 Azure Active Directory 对用户进行身份验证。 该功能始终处于启用状态，并且是唯一的选项。 如果使用其他 IdP，请将 IdP 与 Azure Active Directory 联合。
* [基于角色的访问控制](access-control/index.md)：控制对群集、作业、数据表、API 和工作区资源（如笔记本、文件夹、作业和已注册的模型）的访问。
* [凭据直通身份验证](/security/credential-passthrough/adls-passthrough)：使用用户的 Azure Active Directory 凭据控制对 Azure Data Lake Storage 的访问。
* [VNet 注入](../administration-guide/cloud-configurations/azure/vnet-inject.md)：在你自己的 VNet 中部署在 Azure 订阅中管理的 Azure Databricks 工作区。 使你能够通过自定义网络安全组规则来实现自定义网络配置。
* [安全群集连接](secure-cluster-connectivity.md)：在工作区上启用安全群集连接，这意味着 VNet 的网络安全组 (NSG) 没有打开的入站端口，并且 Databricks Runtime 群集节点没有公共 IP 地址。 也称为“无公共 IP”。 可以在部署期间为工作区启用此功能。 此功能目前以[公共预览版](/release-notes/release-types)提供。

  > [!NOTE]
  >
  > 无论是否启用了安全群集连接，数据平面 VNet 与 Azure Databricks 控制平面之间的所有 Azure Databricks 网络流量都会经过 Microsoft 网络主干，而不是公共 Internet。

* [IP 访问列表](network/ip-access-list.md)：强制执行工作区用户的网络位置。
* [用于加密笔记本数据的客户管理的密钥](keys/customer-managed-key-notebook.md)：使用你管理的 Azure Key Vault 密钥加密笔记本数据。
* [用于加密 DBFS 根的客户管理的密钥](keys/customer-managed-keys-dbfs/index.md)：使用你管理的 Azure Key Vault 密钥加密 Databricks 文件存储 (DBFS) 根。 DBFS 是一个装载到 Azure Databricks 工作区的分布式文件系统，可以在 Azure Databricks 群集上使用。

你可能还会对以下内容感兴趣：

* [群集策略](/administration-guide/clusters/policies)：基于一组规则限制用户配置群集的能力。 利用群集策略可以强制执行特定群集设置（例如实例类型、附加库、计算成本），并为不同的用户级别显示不同的群集创建接口。

## <a name="workspaces"></a>工作区

Azure Databricks 工作区是用于访问 Azure Databricks 资产的环境。 工作区可将对象（[笔记本](../workspace/workspace-assets.md#ws-notebooks)、[库](../workspace/workspace-assets.md#ws-libraries)和[试验](../applications/mlflow/tracking.md#mlflow-experiments)）组织到[文件夹](../workspace/workspace-objects.md#folders)中。 工作区提供对[数据](../data/index.md#data)和计算资源（例如[群集](../workspace/workspace-assets.md#ws-clusters)和[作业](../workspace/workspace-assets.md#ws-jobs)）的访问权限。

确定你的组织需要多少个工作区、需要与哪些团队进行协作以及你对地理区域的要求。

像示例 SmallCorp 这样的小型组织可能只需要一个或少数几个工作区。 对于相对独立的大型公司的单个部门可能也是如此。 工作区管理员可以是工作区的普通用户。 在某些情况下，单独的部门 (IT/OpSec) 可能会承担工作区管理员的角色，以根据企业管理策略进行部署并管理权限、用户和组。

像示例 LargeCorp 这样的大型组织通常需要许多工作区。 LargeCorp 已有集中履行所有安全和管理职能的团队 (IT/OpSec)。 该集中化团队通常会设置新的工作区，并在整个公司内强制实施安全控制措施。

大型公司可能创建单独的工作区的常见原因如下：

* 团队处理不同级别的机密信息，其中可能包括个人身份信息。 通过分隔工作区，团队可以将不同级别的机密资产分开，而不会增加复杂性，例如设置访问控制列表。 例如，LargeCorp 财务团队可以轻松地将与财务相关的笔记本与其他部门的工作区分开存储。
* 简化将计入不同账单的 Databricks 使用情况 (DBU) 和云计算的计费。
* 团队或数据源的地理区域变化。 一个区域中的团队可能会倾向于使用不同区域的云资源来满足成本、网络延迟或法律合规性需求。 每个工作区都可以在不同的受支持区域中进行定义。

虽然通常使用工作区按团队、项目或地域隔离对资源的访问，但还有其他一些选择。 工作区管理员可以在工作区中使用[访问控制列表 (ACL)](#acl)，根据用户和组成员资格来限制对笔记本、文件夹、作业等资源的访问。 还可以使用[凭据直通身份验证](#passthrough)在一个工作区中控制对数据源的差异访问。

### <a name="plan-your-virtual-network-configuration"></a>规划虚拟网络配置

Azure Databricks 的默认部署会创建由 Microsoft 管理的新虚拟网络。 你可以改为在自己的客户管理的虚拟网络（也称为 VNet 注入）中创建新的工作区。 若要了解需要执行此操作的原因，请参阅[在 Azure 虚拟网络（VNet 注入）中部署 Azure Databricks](../administration-guide/cloud-configurations/azure/vnet-inject.md)。

小型公司可能会使用默认的 VNet，或者针对一个 Databricks 工作区只有一个客户管理的 VNet。 如果有两个或三个工作区，则具体取决于网络体系结构和区域，它们可能会与多个工作区共用一个客户管理的 VNet。

大型公司通常想要创建和指定一个客户管理的 VNet 供 Databricks 使用。 他们可以让某些工作区共用一个 VNet，以简化 Azure 资源分配。 一个组织可能在不同的 Azure 订阅和不同的区域中拥有多个 VNet，这可能会影响工作区数量的规划以及为支持这些工作区而规划的 VNet 数量。

> [!div class="mx-imgBorder"]
> ![规划工作区](../_static/images/security/security-overview-workspaces-azure.png)

对于每个工作区，必须提供两个子网：一个用于容器，另一个用于主机。 不能跨工作区共享这些子网（或其地址空间）。 如果在一个 VNet 中有多个工作区，则必须在该 VNet 中规划地址空间。 如需查看受支持的 VNet 和子网大小以及各种子网大小的最大 Azure Databricks 节点，请参阅[在 Azure 虚拟网络中部署 Azure Databricks（VNet 注入）](../administration-guide/cloud-configurations/azure/vnet-inject.md)。

### <a name="customer-managed-keys-for-root-azure-blob-storage-root-dbfs"></a><a id="cmk_dbfs"> </a><a id="customer-managed-keys-for-root-azure-blob-storage-root-dbfs"> </a>用于根 Azure Blob 存储（根 DBFS）的客户管理的密钥

Databricks 文件系统 (DBFS) 是一个装载到 Azure Databricks 工作区的分布式文件系统，可以在 Azure Databricks 群集上使用。 DBFS 在 Azure Databricks 工作区的受管理资源组中实现为存储帐户。 DBFS 中的默认存储位置称为 DBFS 根。 默认情况下，存储帐户使用 Microsoft 管理的密钥进行加密。 在你自己的订阅中，此根 Azure Blob 存储还存储其他托管数据，如群集或作业日志以及笔记本版本历史记录。

你也可以选择性地使用客户管理的密钥保护工作区的根 Azure Blob 存储。 有关详细信息，请参阅[为 DBFS 根配置客户管理的密钥](keys/customer-managed-keys-dbfs/index.md)

### <a name="customer-managed-keys-for-notebook-data-in-the-control-plane"></a><a id="cmk_notebook"> </a><a id="customer-managed-keys-for-notebook-data-in-the-control-plane"> </a>控制平面中用于笔记本数据的客户管理的密钥

Azure Databricks 工作区包含一个托管在 Azure Databricks 管理订阅中的控制平面，还包含一个部署在客户订阅中的虚拟网络中的数据平面 。 控制平面将笔记本源代码和某些笔记本结果存储在数据库中。 默认情况下，对于每个工作区，系统都使用不同的加密密钥对笔记本和某些结果进行静态加密。

如果安全性和合规性要求指定你必须拥有和管理用于加密笔记本和所有结果的密钥，那么你可以提供自己的密钥，用于加密存储在 Azure Databricks 控制平面中的笔记本数据。 有关详细信息，请参阅[为笔记本启用客户管理的密钥](keys/customer-managed-key-notebook.md)。

此功能不会对存储在控制平面之外的数据进行加密。 例如，它不会加密根 Azure Blob 存储中的数据，该存储区会存储作业结果和称为根 DBFS 的其他部分。 你可以用自己的客户管理的密钥来加密这些信息。 请参阅[用于根 Azure Blob 存储（根 DBFS）的客户管理的密钥](#cmk_dbfs)

## <a name="authentication-and-user-account-provisioning"></a><a id="auth"> </a><a id="authentication-and-user-account-provisioning"> </a>身份验证和用户帐户预配

单一登录 (SSO) 是指使用与工作区关联的 Azure Active Directory (Azure AD) 实例对用户进行身份验证的功能。 作为 Azure AD 内置支持的一部分，SSO 始终处于启用状态。 Azure Databricks 无权访问用户凭据。 Azure Databricks 通过 OpenID Connect 进行身份验证，OpenID Connect 是在 OAuth 2.0 的基础上构建的。 你可以选择性地使用 Azure AD 对多重身份验证 (MFA) 的支持。

使用 SSO 为 Azure Databricks 自动预配本地用户帐户的 Azure Databricks 行为取决于用户是否是管理员：

* **管理员用户**：如果 Azure AD 用户或服务主体在 Databricks 资源或子组上具有 ``Contributor`` 或 ``Owner`` 角色，则将在登录过程中预配 Azure Databricks 本地帐户。 这有时称为实时 (JIT) 预配。 用户将被添加到管理员的内置 Azure Databricks 组 (``admins``)。
* **非管理员用户**：Azure Databricks 工作区中不提供针对非管理员用户帐户的 JIT 预配。 若要自动预配非管理员用户，请使用[通过 Azure AD 进行跨域身份管理系统 (SCIM) 用户同步](../administration-guide/users-groups/scim/aad.md)。 Azure Databricks 和 Azure AD 都支持 SCIM 用户和组同步，包括预配和取消预配本地用户帐户。 尽管可以使用用户名来管理 Databricks 资源（如笔记本）的访问控制列表 (ACL)，但使用 Azure AD 进行 SCIM 组同步可以简化 Databricks ACL 的管理。 请参阅[访问控制列表 (ACL)](#acl)。

  > [!NOTE]
  >
  > Azure AD SCIM 用户和组同步需要高级版 Azure AD 帐户。 请参阅[为 Microsoft Azure Active Directory 配置 SCIM 预配](../administration-guide/users-groups/scim/aad.md)。 如果不想使用 Azure AD SCIM 同步，可以在管理员控制台中预配用户和组，或直接调用 Databricks [SCIM REST API](../dev-tools/api/latest/scim/scim-me.md) 来管理用户和组。 SCIM 提供[公共预览版](../release-notes/release-types.md)。

小型公司（例如示例 SmallCorp）可能会使用 Azure Databricks 管理员控制台通过电子邮件地址手动创建新用户，在这种情况下，用户还必须是与该工作区关联的 Azure AD 租户的成员。 但 Databricks 建议你使用 Azure AD 用户和组同步来自动执行用户预配。

大型公司（例如示例 LargeCorp）通常使用 Azure AD SCIM 用户和组同步来管理 Azure Databricks 用户和组。

## <a name="secure-api-access"></a><a id="rest"> </a><a id="secure-api-access"> </a>安全 API 访问

对于 REST API 身份验证，请使用内置的可撤销 Azure Databricks 个人访问令牌或使用可撤销的 Azure Active Directory 访问令牌。

你可以使用[令牌管理 API](../administration-guide/access-control/tokens.md) 查看当前的 Azure Databricks 个人访问令牌、删除令牌并设置新令牌的最长生存期。 使用相关的[权限 API](../administration-guide/access-control/tokens.md#manage-token-permissions-using-the-permissions-api) 设置令牌权限，这些权限可定义哪些用户可以创建和使用令牌来访问工作区 REST API。

使用令牌权限强制执行[最小特权原则](https://en.wikipedia.org/wiki/Principle_of_least_privilege)，使任何单个用户或组只有在具有合法需求时才有权访问 REST API。

对于在发布 Azure Databricks 平台 3.28 版（2020 年 9 月 9 日至 15 日）之后创建的工作区，默认情况下，只有管理员用户能够生成个人访问令牌。 管理员必须显式授予这些权限，无论是向整个 ``users`` 组授予还是按用户或按组授予。 在 3.28 之前创建的工作区保留在此更改之前已有的权限，但具有不同的默认值。 如果不确定工作区的创建时间，请检查工作区的令牌权限。

有关用于令牌的 API 和帐户控制台工具的完整列表，请参阅[管理个人访问令牌](../administration-guide/access-control/tokens.md)。

## <a name="ip-access-lists"></a>IP 访问列表

身份验证可证明用户身份，但对用户的网络位置并没有强制要求。 从不安全的网络访问云服务会带来安全风险，尤其是在用户可能已获得授权访问敏感数据或个人数据的情况下。 企业网络外围（例如防火墙、代理、DLP 和日志记录）会应用安全策略，并限制对外部服务的访问，因此超出这些控制的访问被视为是不可信的。

例如，如果员工从办公室前往了一家咖啡店，则即使该员工具有访问 Web 应用程序和 REST API 的正确凭据，公司仍可阻止与 Azure Databricks 工作区的连接。

指定允许访问的公用网络上的 IP 地址（或 CIDR 范围）。 这些 IP 地址可能属于出口网关或特定的用户环境。 你也可以指定要阻止的 IP 地址或子网，即使它们包含在允许列表中也是如此。 例如，允许的 IP 地址范围可能包括较小范围的基础结构 IP 地址（这些地址实际上超出了实际的安全网络外围）。

有关详细信息，请参阅 [IP 访问列表](network/ip-access-list.md)。

> [!div class="mx-imgBorder"]
> ![IP 访问列表概述关系图](../_static/images/security/network/ip-access-lists-azure.png)

## <a name="audit-logs"></a>审核日志

审核日志可自动用于 Azure Databricks 工作区。 必须将这些日志配置为流入 Azure 存储帐户、Azure Log Analytics 工作区或 Azure 事件中心。 在 Azure 门户中，用户界面将其称为诊断日志。 试用新工作区后，请尝试检查审核日志。 请参阅 [Azure Databricks 中的诊断日志记录](../administration-guide/account-settings/azure-diagnostic-logs.md)。

## <a name="cluster-policies"></a>群集策略

使用[群集策略](../administration-guide/clusters/policies.md)可强制执行特定群集设置（如实例类型、节点数、附加库和计算成本），并为不同的用户级别显示不同的群集创建接口。 使用策略管理群集配置有助于强制实施通用治理控制措施和管理计算基础结构的成本。

SmallCorp 等小型组织可能对所有群集使用一个群集策略。

LargeCorp 等大型组织可能具有更多复杂的策略，例如：

* 允许处理超大型数据集和复杂计算的客户数据分析人员具有多达数百个节点的群集。
* 允许金融团队数据分析人员使用多达十个节点的群集。
* 仅允许处理小型数据集和更简单的笔记本的人力资源部门使用四到八个节点的自动缩放群集。

## <a name="access-control-lists-acls"></a><a id="access-control-lists-acls"> </a><a id="acl"> </a>访问控制列表 (ACL)

许多 Azure Databricks 对象都具有访问控制列表 (ACL)，你可以使用这些列表来控制对特定用户和组的访问。 管理员可以使用[管理员控制台](access-control/index.md)或[权限 API](../dev-tools/api/latest/permissions.md) 来启用 ACL。

管理员还可以通过授予 ``Can manage`` 权限（例如，对群集具有 ``Can Manage`` 权限的任何用户都可以向其他用户授予对该群集执行附加、重启、重设大小和管理操作的权限）来将一些 ACL 配置委派给非管理员用户。

可以为用户、组和 Azure 服务主体设置以下 ACL。 除非另行指定，否则可以使用 Web 应用程序和 REST API 修改 ACL：

| 对象                                                                    | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [表](access-control/table-acls/index.md)                               | 强制访问数据表。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| [Cluster](access-control/cluster-acl.md)                                  | 管理哪些用户可以管理、重启或附加到群集。 在为群集配置数据源的直通身份验证时，对群集的访问会影响安全性，请参阅[使用 Azure Databricks 登录凭据访问数据源](#passthrough)。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| [池](access-control/pool-acl.md)                                        | 管理哪些用户可以管理或附加池。 某些 API 和文档将池称为实例池。 池通过维护一组空闲的随时可用的云实例来减少群集启动和自动缩放时间。 如果附加到池的群集需要一个实例，它首先会尝试分配池的一个空闲实例。 如果池中没有空闲实例，则该池会通过从实例提供程序分配新的实例进行扩展，以满足群集的请求。 当群集释放了某个实例时，该实例将返回到池中，以供其他群集使用。 只有附加到池的群集才能使用该池的空闲实例。 |
| [作业](access-control/jobs-acl.md)                                        | 管理哪些用户可以查看、管理、触发、取消或拥有作业。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| [笔记本](access-control/workspace-acl.md)                               | 管理哪些用户可以读取、运行、编辑或管理笔记本。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| [文件夹（目录](access-control/workspace-acl.md)                      | 管理哪些用户可以读取、运行、编辑或管理文件夹中的所有笔记本。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| [已注册 MLflow 的模型和试验](access-control/workspace-acl.md) | 管理哪些用户可以读取、编辑或管理已注册 MLflow 的模型和试验。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 令牌                  | 管理哪些用户可以创建或使用令牌。 另请参阅[安全 API 访问](#rest)。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

根据团队规模和信息敏感度，像 SmallCorp 这样的小型公司或 LargeCorp 中拥有自己工作区的小型团队可能会允许所有非管理员用户访问相同的对象，例如群集、作业、笔记本和目录。

具有高度敏感信息的大型团队或组织最好使用所有访问控制来实施[最小特权原则](https://en.wikipedia.org/wiki/Principle_of_least_privilege)，以便任何单个用户只能在具有合法需求时才有权访问相应资源。

例如，假设 LargeCorp 中有三名人员需要访问财务团队的专用工作区文件夹（其中包含笔记本和试验）。 LargeCorp 可以使用这些 API 仅向财务数据团队组授予目录访问权限。

## <a name="data-source-access-using-azure-databricks-login-credentials"></a><a id="data-source-access-using-azure-databricks-login-credentials"> </a><a id="passthrough"> </a>使用 Azure Databricks 登录凭据访问数据源

可以使用登录 Azure Databricks 时所用的相同 Azure Active Directory (Azure AD) 标识自动从 Azure Databricks 群集向 [Azure Data Lake Storage Gen1](../data/data-sources/azure/azure-datalake.md#adls-gen1) 和 [Azure Data Lake Storage Gen2](../data/data-sources/azure/azure-datalake-gen2.md#adls-gen2) 进行身份验证。 为群集启用 <Passthrough> 时，在该群集上运行的命令可以在 <ADLS> 中读取和写入数据，无需配置用于访问存储的服务主体凭据。

凭据直通身份验证仅适用于 <ADLS> Gen1 或 Gen2 存储帐户。 <ADLS> Gen2 存储帐户必须使用分层命名空间才能处理 <Passthrough>。 请参阅[创建 Azure Data Lake Storage Gen2 帐户并初始化文件系统](../data/data-sources/azure/azure-datalake-gen2.md#create-adls-account)。

有关详细信息，请参阅[使用 Azure Active Directory 凭据直通身份验证保护对 Azure Data Lake Storage 的访问](credential-passthrough/adls-passthrough.md)。

## <a name="secrets"></a>机密

另外，还建议你使用机密管理器来设置[你希望笔记本需要的机密](secrets/secrets.md)。 机密是一种键值对，用于存储外部数据源或其他计算的机密材料，它具有一个密钥名称，且在[机密范围](secrets/secret-scopes.md)中是唯一的。

你可以使用 REST API 或 CLI 创建机密，但必须使用笔记本或作业中的[机密实用程序](../dev-tools/databricks-utils.md#dbutils-secrets)来读取机密。

## <a name="automation-template-options"></a><a id="automation"> </a><a id="automation-template-options"> </a>自动化模板选项

使用 Databricks REST API 时，某些安全配置任务可能会自动执行。 为帮助 Databricks 客户使用典型的部署方案，你可以使用 [ARM 模板](https://azure.microsoft.com/resources/templates/?term=Azure+Databricks)来部署工作区。 尤其对于具有多个工作区的大型公司，使用模板可实现快速且一致的工作区自动化部署。