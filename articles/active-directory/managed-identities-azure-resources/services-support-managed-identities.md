---
title: 支持托管标识的 Azure 服务 - Azure AD
description: 支持 Azure 资源托管标识和 Azure AD 身份验证的服务列表
services: active-directory
author: barclayn
ms.author: v-junlch
ms.date: 02/24/2021
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: daveba
ms.collection: M365-identity-device-management
ms.custom: references_regions
ms.openlocfilehash: c60f4a53a6b7d75cf66e63523900db9ec8e0b2a6
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697950"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的服务

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 使用托管标识，可对支持 Azure AD 身份验证的任何服务进行身份验证，而无需在代码中插入凭据。 我们正在跨 Azure 将 Azure 资源托管标识与 Azure AD 身份验证集成。 请经常再回来看看，确定这部分内容是否有更新。

> [!NOTE]
> Azure 资源托管标识是以前称为托管服务标识 (MSI) 的服务的新名称。

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的 Azure 服务

以下 Azure 服务支持 Azure 资源托管标识：


### <a name="azure-api-management"></a>Azure API 管理

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | 预览 | 预览 | 不可用 | 预览 |

请参阅以下列表来配置 Azure API 管理的托管标识（在可用的区域中）：

- [Azure Resource Manager 模板](../../api-management/api-management-howto-use-managed-service-identity.md)

### <a name="azure-app-configuration"></a>Azure 应用配置

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | ![可用][check] | ![可用][check]  | 不可用  | ![可用][check] |

请参阅以下列表来配置 Azure 应用配置的托管标识（在可用的区域中）：

- [Azure CLI](../../azure-app-configuration/overview-managed-identity.md)

### <a name="azure-app-service"></a>Azure 应用服务

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |
| 用户分配 | ![可用][check] | ![可用][check]  | ![可用][check]  | ![可用][check] |

请参阅以下列表来配置 Azure 应用服务的托管标识（在可用的区域中）：

- [Azure 门户](../../app-service/overview-managed-identity.md#using-the-azure-portal)
- [Azure CLI](../../app-service/overview-managed-identity.md#using-the-azure-cli)
- [Azure PowerShell](../../app-service/overview-managed-identity.md#using-azure-powershell)
- [Azure Resource Manager 模板](../../app-service/overview-managed-identity.md#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure 蓝图

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | 不可用 |
| 用户分配 | ![可用][check] | ![可用][check] | 不可用 | 不可用 |

### <a name="azure-cognitive-search"></a>Azure 认知搜索

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

### <a name="azure-cognitive-services"></a>Azure 认知服务

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |


### <a name="azure-container-instances"></a>Azure 容器实例

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | Linux：预览<br>Windows:不可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | Linux：预览<br>Windows:不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 容器实例的托管标识（在可用的区域中）：

- [Azure CLI](/container-instances/container-instances-managed-identity)
- [Azure Resource Manager 模板](/container-instances/container-instances-managed-identity#enable-managed-identity-using-resource-manager-template)
- [YAML](/container-instances/container-instances-managed-identity#enable-managed-identity-using-yaml-file)


### <a name="azure-container-registry-tasks"></a>Azure 容器注册表任务

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | 不可用 | 不可用 | 不可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 容器注册表任务的托管标识（在可用的区域中）：

- [Azure CLI](/container-registry/container-registry-tasks-authentication-managed-identity)

### <a name="azure-data-explorer"></a>Azure 数据资源管理器

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

### <a name="azure-data-factory-v2"></a>Azure 数据工厂 V2

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 数据工厂 V2 的托管标识（在可用的区域中）：

- [Azure 门户](/data-factory/data-factory-service-identity#generate-managed-identity)
- [PowerShell](/data-factory/data-factory-service-identity#generate-managed-identity-using-powershell)
- [REST](/data-factory/data-factory-service-identity#generate-managed-identity-using-rest-api)
- [SDK](/data-factory/data-factory-service-identity#generate-managed-identity-using-sdk)



### <a name="azure-event-grid"></a>Azure 事件网格

托管标识类型 |所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | 预览 | 预览 | 不可用 | 预览 |
| 用户分配 | 不可用 | 不可用  | 不可用  | 不可用 |









### <a name="azure-functions"></a>Azure Functions

托管标识类型 |所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |
| 用户分配 | ![可用][check] | ![可用][check]  | ![可用][check]  | ![可用][check]  |

请参阅以下列表来配置 Azure Functions 的托管标识（在可用的区域中）：

- [Azure 门户](../../app-service/overview-managed-identity.md#using-the-azure-portal)
- [Azure CLI](../../app-service/overview-managed-identity.md#using-the-azure-cli)
- [Azure PowerShell](../../app-service/overview-managed-identity.md#using-azure-powershell)
- [Azure Resource Manager 模板](../../app-service/overview-managed-identity.md#using-an-azure-resource-manager-template)

### <a name="azure-iot-hub"></a>Azure IoT 中心

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |


### <a name="azure-importexport"></a>Azure 导入/导出

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 在提供 Azure 导入/导出服务的区域中可用 | 预览 | 可用 | 可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

### <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS)

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | 不可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |


有关详细信息，请参阅[在 Azure Kubernetes 服务中使用托管标识](../../aks/use-managed-identity.md)。


### <a name="azure-logic-apps"></a>Azure 逻辑应用

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |
| 用户分配 | ![可用][check] | ![可用][check] | 不可用 | ![可用][check] |


请参阅以下列表来配置 Azure 逻辑应用的托管标识（在可用的区域中）：

- [Azure 门户](../../logic-apps/create-managed-service-identity.md#enable-system-assigned-identity-in-azure-portal)
- [Azure Resource Manager 模板](../../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)

### <a name="azure-machine-learning"></a>Azure 机器学习

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | 预览 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |

有关详细信息，请参阅[将托管标识与 Azure 机器学习配合使用](../../machine-learning/how-to-use-managed-identities.md)。

### <a name="azure-policy"></a>Azure Policy

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure Policy 的托管标识（在可用的区域中）：

- [Azure 门户](../../governance/policy/tutorials/create-and-manage.md#assign-a-policy)
- [PowerShell](../../governance/policy/how-to/remediate-resources.md#create-managed-identity-with-powershell)
- [Azure CLI](/cli/policy/assignment#az-policy-assignment-create)
- [Azure Resource Manager 模板](https://docs.microsoft.com/azure/templates/microsoft.authorization/policyassignments)
- [REST](https://docs.microsoft.com/rest/api/resources/policyassignments/create)


### <a name="azure-service-fabric"></a>Azure Service Fabric

[Service Fabric 应用程序的托管标识](../../service-fabric/concepts-managed-identity.md)在所有区域中均可用。

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | 不可用 | 不可用 | 不可用 |
| 用户分配 | ![可用][check] | 不可用 | 不可用 |不可用 |

请参考下表为所有区域中的 Azure Service Fabric 应用程序配置托管标识：

- [Azure Resource Manager 模板](https://github.com/Azure-Samples/service-fabric-managed-identity/tree/anmenard-docs)

### <a name="azure-spring-cloud"></a>Azure Spring Cloud

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | 不可用 | 不可用 | ![可用][check] |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |


有关详细信息，请参阅[如何为 Azure Spring Cloud 应用程序启用系统分配的托管标识](../../spring-cloud/spring-cloud-howto-enable-system-assigned-managed-identity.md)。

### <a name="azure-stack-edge"></a>Azure Stack Edge

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 在提供 Azure Stack Edge 服务的区域中可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

### <a name="azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |
| 用户分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |

请参阅以下列表来配置 Azure 虚拟机规模集的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager 模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)



### <a name="azure-virtual-machines"></a>Azure 虚拟机

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |
| 用户分配 | ![可用][check] | ![可用][check] | ![可用][check] | ![可用][check] |

请参阅以下列表来配置 Azure 虚拟机的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager 模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)
- [Azure SDK](qs-configure-sdk-windows-vm.md)


### <a name="azure-vm-image-builder"></a>Azure VM 映像生成器

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | 不可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 在支持的区域中可用 | 不可用 | 不可用 | 不可用 |

### <a name="azure-signalr-service"></a>Azure SignalR 服务

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | 预览 | 预览 | 不可用 | 预览 |
| 用户分配 | 预览 | 预览 | 不可用 | 预览 |

请参阅以下列表来配置 Azure SignalR 服务的托管标识（在可用的区域中）：

- [Azure Resource Manager 模板](../../azure-signalr/howto-use-managed-identity.md)

### <a name="azure-resource-mover"></a>Azure 资源移动器

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure 德国 | Azure 中国世纪互联 |
| --- | :-: | :-: | :-: | :-: |
| 系统分配 | 在提供 Azure 资源移动服务的区域中可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下文档来使用 Azure 资源移动服务：

- [Azure 资源移动器](../../resource-mover/overview.md)

## <a name="azure-services-that-support-azure-ad-authentication"></a>支持 Azure AD 身份验证的 Azure 服务

以下服务支持 Azure AD 身份验证，已通过使用 Azure 资源托管标识的客户端服务进行测试。

### <a name="azure-resource-manager"></a>Azure 资源管理器

请参阅以下列表配置对 Azure 资源管理器的访问权限：

- [通过 Azure 门户分配访问权限](howto-assign-access-portal.md)
- [通过 PowerShell 分配访问权限](howto-assign-access-powershell.md)
- [通过 Azure CLI 分配访问权限](howto-assign-access-CLI.md)
- [通过 Azure 资源管理器模板分配访问权限](../../role-based-access-control/role-assignments-template.md)

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 | | ![可用][check] |
| Azure Government |  | ![可用][check] |
| Azure 德国 |  | ![可用][check] |
| Azure 中国世纪互联 | `https://management.chinacloudapi.cn` | ![可用][check] |

### <a name="azure-key-vault"></a>Azure Key Vault

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 || ![可用][check] |
| Azure Government | | ![可用][check] |
| Azure 德国 | | ![可用][check] |
| Azure 中国世纪互联 | `https://vault.azure.cn` | ![可用][check] |

### <a name="azure-data-lake"></a>Azure Data Lake

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 | | ![可用][check] |
| Azure Government |  | 不可用 |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

### <a name="azure-sql"></a>Azure SQL

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 | | ![可用][check] |
| Azure Government |  | ![可用][check] |
| Azure 德国 |  | ![可用][check] |
| Azure 中国世纪互联 | `https://database.chinacloudapi.cn/` | ![可用][check] |

### <a name="azure-data-explorer"></a>Azure 数据资源管理器

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 |  | ![可用][check] |
| Azure Government |   | ![可用][check] |
| Azure 德国 |  | ![可用][check] |
| Azure 中国世纪互联 | `https://<account>.<region>.kusto.chinacloudapi.cn` | ![可用][check] |

### <a name="azure-event-hubs"></a>Azure 事件中心

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 | | ![可用][check] |
| Azure Government |  | 不可用 |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

### <a name="azure-service-bus"></a>Azure 服务总线

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 |   | ![可用][check] |
| Azure Government |  | ![可用][check] |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |









### <a name="azure-storage-blobs-and-queues"></a>Azure 存储 blob 和队列

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 |  | ![可用][check] |
| Azure Government |  | ![可用][check] |
| Azure 德国 |  | ![可用][check] |
| Azure 中国世纪互联 | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.chinacloudapi.cn` <br /><br />`https://<account>.queue.core.chinacloudapi.cn` | ![可用][check] |

### <a name="azure-analysis-services"></a>Azure Analysis Services

| 云 | 资源 ID | 状态 |
|--------|------------|:-:|
| Azure 全球 | | ![可用][check] |
| Azure Government | | ![可用][check] |
| Azure 德国 |  | ![可用][check] |
| Azure 中国世纪互联 | `https://*.asazure.chinacloudapi.cn` | ![可用][check] |



[check]: ./media/services-support-managed-identities/check.png "可用"
