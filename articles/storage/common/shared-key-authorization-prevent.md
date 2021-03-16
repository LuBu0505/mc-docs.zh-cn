---
title: 阻止通过共享密钥进行授权（预览）
titleSuffix: Azure Storage
description: 若要要求客户端使用 Azure AD 来对请求进行授权，你可以禁止使用共享密钥 对存储帐户进行授权的请求（预览）。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 01/21/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.reviewer: fryu
ms.openlocfilehash: 8ca6bffe936f8b17f7a97745d876c81b6ff5d154
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196432"
---
# <a name="prevent-shared-key-authorization-for-an-azure-storage-account-preview"></a>阻止对 Azure 存储帐户进行共享密钥授权（预览）

对 Azure 存储帐户的每个安全请求都必须经过授权。 默认情况下，可以使用 Azure Active Directory (Azure AD) 凭据对请求进行授权，或使用帐户访问密钥进行共享密钥授权。 在这两种类型的授权中，与共享密钥相比，Azure AD 提供更高级别的安全性和易用性，是 Azure 推荐的授权方法。 若要要求客户端使用 Azure AD 来对请求进行授权，你可以禁止使用共享密钥 对存储帐户进行授权的请求（预览）。

当禁止对某个存储帐户进行共享密钥授权时，Azure 存储将拒绝向该帐户发出的所有使用帐户访问密钥进行授权的后续请求。 只有通过 Azure AD 进行授权的安全请求才会成功。 有关使用 Azure AD 的详细信息，请参阅[使用 Azure Active Directory 授予对 blob 和队列的访问权限](storage-auth-aad.md)。

> [!WARNING]
> Azure 存储仅支持针对 Blob 和队列存储请求的 Azure AD 授权。 如果你不允许对存储帐户使用共享密钥授权，则使用共享密钥授权的 Azure 文件存储或表存储的请求将失败。 由于 Azure 门户始终使用共享密钥授权来访问文件和表数据，因此，如果你不允许对存储帐户使用共享密钥进行授权，将无法访问 Azure 门户中的文件或表数据。
>
> Azure 建议你在禁止通过共享密钥访问帐户之前，将任何 Azure 文件存储或表存储数据迁移到单独的存储帐户，或者不将此设置应用于支持 Azure 文件存储或表存储工作负载的存储帐户。
>
> 禁止对存储帐户进行共享密钥访问不会影响与 Azure 文件存储的 SMB 连接。

本文介绍如何检测通过共享密钥授权发送的请求，以及如何修正存储帐户的共享密钥授权。 若要了解如何注册使用预览功能，请参阅[关于预览](#about-the-preview)。

## <a name="detect-the-type-of-authorization-used-by-client-applications"></a>检测客户端应用程序使用的授权类型

如果你不允许对存储帐户进行共享密钥授权，则从使用帐户访问密钥进行共享密钥授权的客户端发出的请求将失败。 若要了解在进行此更改之前禁用共享密钥授权可能会对客户端应用程序产生何种影响，请为存储帐户启用日志记录和指标。 然后，你可以分析一段时间内帐户的请求模式，以确定请求的授权方式。

使用指标来确定存储帐户收到的通过共享密钥或共享访问签名 (SAS) 授权的请求数。 使用日志来确定发送这些请求的客户端。

有关在预览过程中解释使用共享访问签名发出的请求的详细信息，请参阅[关于预览](#about-the-preview)。

### <a name="monitor-how-many-requests-are-authorized-with-shared-key"></a>监视通过共享密钥授权的请求数

若要跟踪对存储帐户请求的授权方式，请在 Azure 门户中使用 Azure 指标资源管理器。 若要详细了解 Azure 指标资源管理器，请参阅 [Azure 指标资源管理器入门](../../azure-monitor/platform/metrics-getting-started.md)。

按照以下步骤创建一个指标，该指标用于跟踪使用共享密钥或 SAS 发出的请求：

1. 导航到 Azure 门户中的存储帐户。 在“监视”部分下，选择“指标” 。
1. 选择“添加指标”。 在“指标”对话框中，指定以下值：
    1. 将“范围”字段设置为存储帐户的名称。
    1. 将“指标命名空间”设置为“帐户”。 此指标将报告对存储帐户的所有请求。
    1. 将“指标”字段设置为“事务”。
    1. 将“聚合”字段设置为“求和”。

    新指标会显示给定时间间隔内针对存储帐户的事务数之和。 生成的指标如下图所示：

    :::image type="content" source="media/shared-key-authorization-prevent/configure-metric-account-transactions.png" alt-text="显示如何将指标配置为对使用共享密钥或 SAS 进行的事务数求和的屏幕截图":::

1. 接下来，选择“添加筛选器”按钮，为授权类型指标创建筛选器。
1. 在“筛选器”对话框中，指定以下值：
    1. 将属性值设置为“身份验证”。
    1. 将“运算符”字段设置为等号 (=)。
    1. 在“值”字段中，选择“帐户密钥”和“SAS” 。
1. 在右上角，选择要查看指标的时间范围。 还可以通过指定从 1 分钟到 1 个月的时间间隔，来指示请求聚合粒度。 例如，将“时间范围”设置为30天，并将“时间粒度”设置为 1 天，以查看过去 30 天内按天聚合的请求 。

配置指标后，对存储帐户的请求将开始显示在图表上。 下图显示了使用共享密钥授权或使用 SAS 令牌发出的请求。 在过去的 30 天内，请求按天聚合。

:::image type="content" source="media/shared-key-authorization-prevent/metric-shared-key-requests.png" alt-text="显示通过共享密钥授权的聚合请求的屏幕截图":::

你还可以配置警报规则，让系统在对针对存储帐户发出的匿名请求达到一定数量时通知你。 有关详细信息，请参阅[使用 Azure Monitor 创建、查看和管理指标警报](../../azure-monitor/platform/alerts-metric.md)。

## <a name="remediate-authorization-via-shared-key"></a>修正通过共享密钥进行的授权

你可以采取措施来阻止通过共享密钥进行访问。 但首先，你需要将使用共享密钥授权的所有应用程序更新为改用 Azure AD。 可以按照[检测客户端应用程序使用的授权类型](#detect-the-type-of-authorization-used-by-client-applications)所述的方法监视日志和指标，以便对转换进行跟踪。 有关将 Azure AD 用于 blob 和队列数据的详细信息，请参阅[使用 Azure Active Directory 授予对 blob 和队列的访问权限](storage-auth-aad.md)。

确信可以安全地拒绝通过共享密钥授权的请求时，可以将存储帐户的“AllowSharedKeyAccess”属性设置为“false” 。

默认情况下，不会设置“AllowSharedKeyAccess”属性，并且在显式设置此属性之前，它不会返回值。 当属性值为“null”或“true”时，存储帐户允许通过共享密钥授权的请求 。

> [!WARNING]
> 如果任何客户端当前正在使用共享密钥访问存储帐户中的数据，则 Azure 建议将这些客户端迁移到 Azure AD，然后再禁止对存储帐户的共享密钥访问。

# <a name="azure-portal"></a>[Azure 门户](#tab/portal)

若要在 Azure 门户中禁止对存储帐户的共享密钥授权，请按照以下步骤操作：

1. 导航到 Azure 门户中的存储帐户。
1. 在“设置”下找到“配置”设置。 
1. 将“允许共享密钥访问”设置为“已禁用” 。

    :::image type="content" source="media/shared-key-authorization-prevent/shared-key-access-portal.png" alt-text="显示如何禁止对帐户的共享密钥访问的屏幕截图":::

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要通过 Azure CLI 禁止对存储帐户的共享密钥授权，请安装 Azure CLI 2.9.1 或更高版本。 有关详细信息，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 接下来，为新的或现有的存储帐户配置“allowSharedKeyAccess”属性。

下面的示例演示如何使用 Azure CLI 设置“allowSharedKeyAccess”属性。 请记得将括号中的占位符值替换为你自己的值：

```azurecli
$storage_account_id=$(az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query id \
    --output tsv)

az resource update \
    --ids $storage_account_id \
    --set properties.allowSharedKeyAccess=false

az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowSharedKeyAccess \
    --output tsv
```

---

禁用共享密钥授权后，使用共享密钥授权对存储帐户发出的请求将失败，错误代码为 403（禁止访问）。 Azure 存储会返回错误，指示不允许对存储帐户进行基于密钥的授权。

### <a name="verify-that-shared-key-access-is-not-allowed"></a>验证是否不允许使用共享密钥访问

若要验证是否不再允许使用共享密钥授权，可以尝试使用帐户访问密钥调用数据操作。 以下示例尝试使用访问密钥创建容器。 在不允许对存储帐户进行共享密钥授权的情况下，此调用将失败。 请记得将括号中的占位符值替换为你自己的值：

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name sample-container \
    --account-key <key>
    --auth-mode key
```

> [!NOTE]
> 匿名请求是未经授权的，如果已将存储帐户和容器配置为匿名公共读取访问，则该请求将继续。 有关详细信息，请参阅[配置对容器和 Blob 的匿名公共读取访问](../blobs/anonymous-read-access-configure.md)。

## <a name="permissions-for-allowing-or-disallowing-shared-key-access"></a>允许或禁止共享密钥访问的权限

若要为存储帐户设置“AllowSharedKeyAccess”属性，用户必须具有创建和管理存储帐户的权限。 提供这些权限的 Azure 基于角色的访问控制 (Azure RBAC) 角色包含 Microsoft.Storage/storageAccounts/write 或 Microsoft.Storage/storageAccounts/\* 操作 。 具有此操作的内置角色包括：

- Azure 资源管理器[所有者](../../role-based-access-control/built-in-roles.md#owner)角色
- Azure 资源管理器[参与者](../../role-based-access-control/built-in-roles.md#contributor)角色
- [存储帐户参与者](../../role-based-access-control/built-in-roles.md#storage-account-contributor)角色

这些角色不提供通过 Azure Active Directory (Azure AD) 对存储帐户中数据的访问权限。 但是，它们包含 Microsoft.Storage/storageAccounts/listkeys/action，可以授予对帐户访问密钥的访问权限。 借助此权限，用户可以使用帐户访问密钥访问存储帐户中的所有数据。

角色分配的范围必须设定为存储帐户级别或更高级别，以允许用户启用或禁用对存储帐户的共享密钥访问。 有关角色范围的详细信息，请参阅[了解 Azure RBAC 的范围](../../role-based-access-control/scope-overview.md)。

请注意，仅向需要能够创建存储帐户或更新其属性的用户分配这些角色。 使用最小特权原则确保用户拥有完成任务所需的最少权限。 有关使用 Azure RBAC 管理访问权限的详细信息，请参阅 [Azure RBAC 最佳做法](../../role-based-access-control/best-practices.md)。

> [!NOTE]
> 经典订阅管理员角色“服务管理员”和“共同管理员”具有 Azure 资源管理器[所有者](../../role-based-access-control/built-in-roles.md#owner)角色的等效权限。 所有者角色包括所有操作，因此具有这些管理角色之一的用户也可以创建和管理存储帐户。 有关详细信息，请参阅[经典订阅管理员角色、Azure 角色和 Azure AD 管理员角色](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles)。

## <a name="understand-how-disallowing-shared-key-affects-sas-tokens"></a>了解禁用共享密钥如何影响 SAS 令牌

当存储帐户不允许使用共享密钥访问时，Azure 存储会根据 SAS 的类型和请求的目标服务来处理 SAS 令牌。 下表显示了每种类型的 SAS 的授权方式，以及当存储帐户的“AllowSharedKeyAccess”属性为“false”时，Azure 存储将如何处理该 SAS 。

| SAS 类型 | 授权类型 | AllowSharedKeyAccess 为 false 时的行为 |
|-|-|-|
| 用户委托 SAS（仅限 Blob 存储） | Azure AD | 允许请求。 Azure 建议尽可能使用用户委托 SAS 以提高安全性。 |
| 服务 SAS | 共享密钥 | 拒绝对所有 Azure 存储服务的请求。 |
| 帐户 SAS | 共享密钥 | 拒绝对所有 Azure 存储服务的请求。 |

有关共享访问签名的详细信息，请参阅[使用共享访问签名 (SAS) 授予对 Azure 存储资源的有限访问权限](storage-sas-overview.md)。

## <a name="consider-compatibility-with-other-azure-tools-and-services"></a>考虑与其他 Azure 工具和服务的兼容性

许多 Azure 服务使用共享密钥授权来与 Azure 存储进行通信。 如果不允许对存储帐户进行共享密钥授权，这些服务将无法访问该帐户中的数据，并且应用程序可能会受到负面影响。

某些 Azure 工具提供了使用 Azure AD 授权来访问 Azure 存储的选项。 下表列出了一些常用的 Azure 工具，并说明了它们是否可以使用 Azure AD 来授权对 Azure 存储的请求。

| Azure 工具 | 对 Azure 存储的 Azure AD 授权 |
|-|-|
| Azure 门户 | 支持。 有关使用 Azure AD 帐户从 Azure 门户进行授权的信息，请参阅[选择如何授予对 Azure 门户中 blob 数据的访问权限](../blobs/authorize-data-operations-portal.md)。 |
| AzCopy | 支持用于 Blob 存储。 有关授权 AzCopy 操作的信息，请参阅 AzCopy 文档中的[选择如何提供授权凭据](storage-use-azcopy-v10.md#choose-how-youll-provide-authorization-credentials)。 |
| Azure 存储资源管理器 | 仅支持用于 Blob 存储和 Azure Data Lake Storage Gen2。 不支持对队列存储的 Azure AD 访问。 请确保选择正确的 Azure AD 租户。 有关详细信息，请参阅[存储资源管理器入门](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows#sign-in-to-azure) |
| Azure PowerShell | 支持。 有关如何使用 Azure AD 为 blob 或队列操作授权 PowerShell 命令的信息，请参阅[使用 Azure AD 凭据运行 PowerShell 命令以访问 blob 数据](../blobs/authorize-data-operations-powershell.md)或[使用 Azure AD 凭据运行 PowerShell 命令以访问队列数据](../queues/authorize-data-operations-powershell.md)。 |
| Azure CLI | 支持。 有关如何使用 Azure AD 授权 Azure CLI 命令来访问 blob 和队列数据的信息，请参阅[使用 Azure AD 凭据运行 Azure CLI 命令以访问 blob 或队列数据](../blobs/authorize-data-operations-cli.md)。 |

## <a name="about-the-preview"></a>关于此预览版

Azure 中提供禁止使用共享密钥授权的预览功能。 仅支持使用 Azure 资源管理器部署模型的存储帐户。 有关哪些存储帐户使用 Azure 资源管理器部署模型的信息，请参阅[存储帐户的类型](storage-account-overview.md#types-of-storage-accounts)。

> [!IMPORTANT]
> 此预览版仅用于非生产用途。

预览功能包括以下各节所述的限制。

### <a name="metrics-and-logging-report-all-requests-made-with-a-sas-regardless-of-how-they-are-authorized"></a>指标和日志记录报告使用 SAS 发出的所有请求，而不考虑它们的授权方式

Azure 指标和 Azure Monitor 中的日志记录不区分预览功能中不同类型的共享访问签名。 Azure 指标资源管理器中的 SAS 筛选器和 Azure Monitor 中 Azure 存储日志记录内的 SAS 字段都会报告通过任何类型的 SAS 授权的请求 。 但是，不同类型的共享访问签名以不同的方式获得授权，并且当不允许使用共享密钥访问时，行为会有所不同：

- 服务 SAS 令牌或帐户 SAS 令牌使用共享密钥授权，当 AllowSharedKeyAccess 属性设置为 false时，不允许在对 Blob 存储的请求中使用 。
- 用户委派 SAS 使用 Azure AD 授权，当 AllowSharedKeyAccess 属性设置为 false时，不允许在对 Blob 存储的请求中使用 。

评估通向存储帐户的流量时，请记住，[检测客户端应用程序使用的授权类型](#detect-the-type-of-authorization-used-by-client-applications)中所述的指标和日志可能包括使用用户委托 SAS 发出的请求。 有关在 AllowSharedKeyAccess 属性设置为 false 时 Azure 存储如何响应 SAS 的详细信息，请参阅[了解禁用共享密钥如何影响 SAS 令牌](#understand-how-disallowing-shared-key-affects-sas-tokens) 。

## <a name="next-steps"></a>后续步骤

- [授予访问 Azure 存储中的数据的权限](storage-auth.md)
- [使用 Azure Active Directory 授予对 Blob 和队列的访问权限](storage-auth-aad.md)
- [通过共享密钥进行授权](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key)
