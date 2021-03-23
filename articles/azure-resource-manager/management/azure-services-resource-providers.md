---
title: 按 Azure 服务列出的资源提供程序
description: 列出 Azure 资源管理器的所有资源提供程序命名空间，并显示该命名空间的 Azure 服务。
ms.topic: conceptual
origin.date: 12/01/2020
author: rockboyfor
ms.date: 03/22/2021
ms.testscope: yes
ms.testdate: 08/24/2020
ms.author: v-yeche
ms.openlocfilehash: e15a61d719471be78d48ba3d710cbb5013053397
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767088"
---
<!--Verify sucessfully on 2020/08/17 by harris-->
# <a name="resource-providers-for-azure-services"></a>Azure 服务的资源提供程序

本文介绍资源提供程序命名空间如何映射到 Azure 服务。

## <a name="match-resource-provider-to-service"></a>将资源提供程序匹配到服务

默认情况下，标记为“- 已注册”的资源提供程序已针对你的订阅注册。 有关详细信息，请参阅[注册](#registration)。

| 资源提供程序命名空间 | Azure 服务 |
| --------------------------- | ------------- |
| Microsoft.AAD | [Azure Active Directory 域服务](../../active-directory-domain-services/index.yml) |
| Microsoft.Advisor | [Azure 顾问](../../advisor/index.yml) |
| Microsoft.AlertsManagement | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](../../analysis-services/index.yml) |
| Microsoft.ApiManagement | [API 管理](../../api-management/index.yml) |
| Microsoft.AppConfiguration | [Azure 应用配置](../../azure-app-configuration/index.yml) |
| Microsoft.AppPlatform | [Azure Spring Cloud](../../spring-cloud/spring-cloud-overview.md) |
| Microsoft.Authorization - [已注册](#registration) | [Azure 资源管理器](../index.yml) |
| Microsoft.Automation | [自动化](../../automation/index.yml) |
| Microsoft.AutonomousSystems | [自治系统](https://www.microsoft.com/ai/autonomous-systems) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../../active-directory-b2c/index.yml) |
| Microsoft.AzureStack | core |
| Microsoft.Batch | [批处理](../../batch/index.yml) |
| Microsoft.Blueprint | Azure 蓝图 |
| Microsoft.Cache | [用于 Redis 的 Azure 缓存](../../azure-cache-for-redis/index.yml) |
| Microsoft.Cdn | [内容分发网络](https://docs.azure.cn/cdn/) |
| Microsoft.CertificateRegistration | [应用服务证书](../../app-service/configure-ssl-certificate.md#import-an-app-service-certificate) |
| Microsoft.ClassicCompute | 经典部署模型虚拟机 |
| Microsoft.ClassicInfrastructureMigrate | 经典部署模型迁移 |
| Microsoft.ClassicNetwork | 经典部署模型虚拟网络 |
| Microsoft.ClassicStorage | 经典部署模型存储 |
| Microsoft.ClassicSubscription - [已注册](#registration) | 经典部署模型 |
| Microsoft.CognitiveServices | [认知服务](../../cognitive-services/index.yml) |
| Microsoft.Compute | [虚拟机](../../virtual-machines/index.yml)<br />[虚拟机规模集](../../virtual-machine-scale-sets/index.yml) |
| Microsoft.ContainerInstance | [容器实例](../../container-instances/index.yml) |
| Microsoft.ContainerRegistry | [容器注册表](../../container-registry/index.yml) |
| Microsoft.ContainerService | [Azure Kubernetes 服务 (AKS)](../../aks/index.yml) |
| Microsoft.DataBox | [Azure Data Box](../../databox/index.yml) |
| Microsoft.Databricks | [Azure Databricks](https://docs.azure.cn/databricks/) |
| Microsoft.DataFactory | [数据工厂](../../data-factory/index.yml) |
| Microsoft.DataMigration | [Azure 数据库迁移服务](../../dms/index.yml) |
| Microsoft.DBforMariaDB | [Azure Database for MariaDB](../../mariadb/index.yml) |
| Microsoft.DBforMySQL | [Azure Database for MySQL](../../mysql/index.yml) |
| Microsoft.DBforPostgreSQL | [Azure Database for PostgreSQL](../../postgresql/index.yml) |
| Microsoft.Devices | [Azure IoT 中心](../../iot-hub/index.yml)<br />[Azure IoT 中心设备预配服务](../../iot-dps/index.yml) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../../cosmos-db/index.yml) |
| Microsoft.EventGrid | [事件网格](../../event-grid/index.yml) |
| Microsoft.EventHub | [事件中心](../../event-hubs/index.yml) |
| Microsoft.Features - [已注册](#registration) | [Azure 资源管理器](../index.yml) |
| Microsoft.GuestConfiguration | [Azure Policy](../../governance/policy/index.yml) |
| Microsoft.HDInsight | [HDInsight](../../hdinsight/index.yml) |
| Microsoft.ImportExport | [Azure 导入/导出](../../storage/common/storage-import-export-service.md) |
| microsoft.insights | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.IoTCentral | Azure IoT Central |
| Microsoft.KeyVault | [密钥保管库](../../key-vault/index.yml) |
| Microsoft.Kusto | [Azure 数据资源管理器](https://docs.azure.cn/data-explorer/) |
| Microsoft.Logic | [逻辑应用](../../logic-apps/index.yml) |
| Microsoft.MachineLearningServices | [Azure 机器学习](../../machine-learning/index.yml) |
| Microsoft.Maintenance | [Azure 维护](../../virtual-machines/maintenance-control-cli.md) |
| Microsoft.ManagedIdentity | [Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft.ManagedServices | Azure Lighthouse |
| Microsoft.Management | [管理组](../../governance/management-groups/index.yml) |
| Microsoft.Marketplace | core |
| Microsoft.MarketplaceApps | core |
| Microsoft.MarketplaceOrdering - [已注册](#registration) | core |
| Microsoft.Media | [媒体服务](../../media-services/index.yml) |
| Microsoft.Network | [应用程序网关](../../application-gateway/index.yml)<br />[Azure Bastion](../../bastion/index.yml)<br />[Azure DNS](../../dns/index.yml)<br />[Azure ExpressRoute](../../expressroute/index.yml)<br />[Azure 防火墙](../../firewall/index.yml)<br />[负载均衡器](../../load-balancer/index.yml)<br />[网络观察程序](../../network-watcher/index.yml)<br />[流量管理器](../../traffic-manager/index.yml)<br />[虚拟网络](../../virtual-network/index.yml)<br />[虚拟 WAN](../../virtual-wan/index.yml)<br />[VPN 网关](../../vpn-gateway/index.yml)<br /> |
| Microsoft.NotificationHubs | [通知中心](../../notification-hubs/index.yml) |
| Microsoft.OperationalInsights | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure Monitor](../../azure-monitor/index.yml) |
| Microsoft.PolicyInsights | [Azure Policy](../../governance/policy/index.yml) |
| Microsoft.Portal - [已注册](#registration) | [Azure 门户](../../azure-portal/index.yml) |
| Microsoft.PowerBI | [Power BI](https://docs.microsoft.com/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/power-bi-embedded/) |
| Microsoft.RecoveryServices | [Azure Site Recovery](../../site-recovery/index.yml) |
| Microsoft.Relay | [Azure 中继](../../azure-relay/relay-what-is-it.md) |
| Microsoft.ResourceGraph - [已注册](#registration) | [Azure Resource Graph](../../governance/resource-graph/index.yml) |
| Microsoft.ResourceHealth | [Azure 服务运行状况](../../service-health/index.yml) |
| Microsoft.Resources - [已注册](#registration) | [Azure Resource Manager](../index.yml) |
| Microsoft.Scheduler | [计划程序](../../scheduler/index.yml) |
| Microsoft.Search | [Azure 认知搜索](../../search/index.yml) |
| Microsoft.Security | [安全中心](../../security-center/index.yml) |
| Microsoft.ServiceBus | [服务总线](https://docs.azure.cn/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../../service-fabric/index.yml) |
| Microsoft.Services | core |
| Microsoft.SignalRService | [Azure SignalR 服务](../../azure-signalr/index.yml) |
| Microsoft.Solutions | [Azure 托管应用程序](../managed-applications/index.yml) |
| Microsoft.Sql | [Azure SQL 数据库](../../azure-sql/database/index.yml)<br /> [Azure SQL 托管实例](../../azure-sql/managed-instance/index.yml) <br />[Azure Synapse Analytics](https://docs.azure.cn/sql-data-warehouse/) |
| Microsoft.Storage | [存储](../../storage/index.yml) |
| Microsoft.StreamAnalytics | [Azure 流分析](../../stream-analytics/index.yml) |
| Microsoft.Synapse | [Azure Synapse Analytics](https://docs.azure.cn/sql-data-warehouse/) |
| Microsoft.TimeSeriesInsights | [Azure 时序见解](../../time-series-insights/index.yml) |
| Microsoft.Web | [应用服务](../../app-service/index.yml)<br />[Azure Functions](../../azure-functions/index.yml) |

<!--CORRECT ON | Microsoft.Cdn | [Content Delivery Network](https://docs.azure.cn/cdn/) |-->
<!--Table Content All Verified Successfully on 08/24/2020-->

## <a name="registration"></a><a name="registration"></a>注册

默认情况下，上面标记为“- 已注册”的资源提供程序已针对你的订阅注册。 若要使用其他资源提供程序，必须[进行注册](resource-providers-and-types.md)。 但是，当你执行某些操作时，系统会为你注册许多资源提供程序。 例如，如果你通过门户创建资源，门户会自动注册所需的尚未注册的资源提供程序。 通过 [Azure 资源管理器模板](../templates/overview.md)部署资源时，也会注册任何所需的资源提供程序。

> [!IMPORTANT]
> 仅在准备好使用资源提供程序时注册该程序。 注册步骤使你能够在订阅中保留最小特权。 恶意用户无法使用未注册的资源提供程序。

## <a name="next-steps"></a>后续步骤

有关资源提供程序的详细信息（包括如何注册资源提供程序），请参阅 [Azure 资源提供程序和类型](resource-providers-and-types.md)。

<!--Update_Description: update meta properties, wording update, update link-->