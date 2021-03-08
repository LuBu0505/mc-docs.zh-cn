---
title: 跨区域移动 Azure 资源的支持
description: 列出可跨 Azure 区域移动的 Azure 资源类型
ms.service: azure-resource-manager
ms.topic: reference
origin.date: 08/25/2020
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 08/24/2020
ms.author: v-yeche
ms.openlocfilehash: 978bc11c535a0ae9d7b196f36b28ae368b63f4bd
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052518"
---
<!--Verify Successfully-->
# <a name="support-for-moving-azure-resources-across-regions"></a>跨区域移动 Azure 资源的支持

本文确认了是否支持将某种 Azure 资源类型移到另一个 Azure 区域。 

跳转到资源提供程序命名空间：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClassicSubscription](#microsoftclassicsubscription)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Web](#microsoftweb)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | domainservices | 否 | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | diagnosticsettings | 否 |
> | diagnosticsettingscategories | 否 |
> | privatelinkforazuread | 否 |
> | tenants |  否 |


## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | 配置 | 否 | 
> | generaterecommendations | 否 |
> | metadata | 否 |
> | 建议 | 否 |
> | 禁止显示 | 否 | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | actionrules | 否 | 
> | alerts | 否 | 
> | alertslist | 否 | 
> | alertsmetadata | 否 | 
> | alertssummary | 否 | 
> | alertssummarylist | 否 | 
> | smartdetectoralertrules | 否 | 
> | smartgroups | 否 | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | servers | 否 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | reportfeedback | 否 |
> | 服务 |  是（使用模板） <br/><br/> [跨区域移动 API 管理](../../api-management/api-management-howto-migrate.md)。 | 

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | configurationstores | 否 | 
> | configurationstores / eventgridfilters | 否 |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | spring | 否 | 

<!--Not Available on ## Microsoft.AppService-->
<!--Not Available on ## Microsoft.Attestation-->

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | classicadministrators | 否 | 
> | dataaliases | 否 | 
> | denyassignments | 否 | 
> | elevateaccess | 否 | 
> | findorphanroleassignments | 否 | 
> | 锁定 | 否 | 
> | 权限 | 否 | 
> | policyassignments | 否 | 
> | policydefinitions | 否 | 
> | policysetdefinitions | 否 | 
> | privatelinkassociations | 否 | 
> | resourcemanagementprivatelinks | 否 | 
> | roleassignments | 否 | 
> | roleassignmentsusagemetrics | 否 | 
> | roledefinitions | 否 | 

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | automationaccounts | 是（使用模板） <br/><br/> [使用异地复制](../../automation/automation-managing-data.md#geo-replication-in-azure-automation) |  
> | automationaccounts/configurations | 否 | 
> | automationaccounts/runbooks | 否 | 

<!--Not Available on ## Microsoft.AVS-->

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | b2cdirectories | 否 | 
> | b2ctenants | 否 | 

<!--Not Available on ## Microsoft.AzureData-->

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | cloudmanifestfiles | 否 |
> | registrations | 否 | 

<!--Not Available on ## Microsoft.AzureStackHCI-->

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | batchaccounts |  Batch 帐户不能直接从一个区域移到另一个区域，但你可以使用模板来导出模板，对其进行修改，然后将模板部署到新区域。 <br/><br/> 了解如何[跨区域移动 Batch 帐户](../../batch/best-practices.md#moving-batch-accounts-across-regions) |

<!--Not Available on ## Microsoft.Billing-->
<!--Not Available on ## Microsoft.BingMaps-->
<!--Not Available on ## Microsoft.BizTalkServices-->
<!--Not Available on ## Microsoft.Blockchain-->
<!--Not Available on ## Microsoft.BlockchainTokens-->

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | blueprintassignments | 否 | 
> | blueprints | 否 |

<!--Not Available on ## Microsoft.BotService-->

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | redis | 否 | 
> | redisenterprise | 否 | 

<!--Not Available on ## Microsoft.Capacity-->

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | 否 |
> | edgenodes | 否
> | 配置文件 | 否 | 
> | profiles/endpoints | 否 | 

<!--Not Available on ## Microsoft.CertificateRegistration-->

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | capabilities | 否 | 
> | domainnames | 是 | 否 |
> | quotas | 否 | 
> | resourcetypes | 否 |
> | validatesubscriptionmoveavailability | 否 | 
> | virtualmachines | 否 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | classicinfrastructureresources | 否 | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | capabilities | 否 | 
> | expressroutecrossconnections | 否 | 
> | expressroutecrossconnections / peerings | 否 | 
> | gatewaysupporteddevices | 否 | 
> | networksecuritygroups | 否 |
> | quotas | 否 |
> | reservedips | 否 | 
> | virtualnetworks | 否 | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | disks | 否 | 
> | images | 否 | 
> | osimages | 否 | 
> | osplatformimages | 否 | 
> | publicimages | 否 | 
> | quotas | 否 | 
> | storageaccounts | 是 |  
> | vmimages | 否 |

## <a name="microsoftclassicsubscription"></a>Microsoft.ClassicSubscription

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | 操作 | 否 | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | accounts | 否 | 

<!--NOT AVAILABLE ON [moving your Azure Cognitive Search service to another region](../../search/search-howto-move-across-regions.md)-->

<!--NOT AVAILABLE ON ## Microsoft.Commerce-->

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | availabilitysets | 是 | 
> | diskaccesses | 否 |
> | diskencryptionsets | 否 | 
> | disks | 是 | 
> | galleries | 否 | 
> | galleries/images | 否 | 
> | galleries/images/versions | 否 | 
> | hostgroups | 否 | 
> | hostgroups/hosts | 否 | 
> | images | 否 | 
> | proximityplacementgroups | 否 | 
> | restorepointcollections | 否 | 
> | sharedvmimages | 否 | 
> | sharedvmimages/versions | 否 | 
> | snapshots | 否 | 
> | sshpublickeys | 否 |
> | virtualmachines | 是 | 
> | virtualmachines/extensions | 否 | 
> | virtualmachinescalesets | 否 | 

<!--NOT AVAILABLE ON ## Microsoft.Consumption-->
<!--NOT AVAILABLE ON ## Microsoft.Container-->

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | containergroups | 否 | 
> | serviceassociationlinks | 否 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | registries | 否 |  
> | registries/agentpools | 否 | 
> | registries/buildtasks | 否 |  
> | registries/replications | 否 | 
> | registries/tasks | 否 |  
> | registries/webhooks | 否 | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | containerservices | 否 |
> | managedclusters | 否 | 
> | openshiftmanagedclusters | 否 | 

<!--Not Available on ## Microsoft.ContentModerator-->
<!--Not Available on ## Microsoft.CortanaAnalytics-->
<!--Not Available on ## Microsoft.CostManagement-->
<!--Not Available on ## Microsoft.CustomerInsights-->
<!--Not Available on ## Microsoft.CustomerLockbox-->
<!--Not Available on ## Microsoft.CustomProviders-->

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | jobs | 否 | 

<!--Not Available on ## Microsoft.DataBoxEdge-->

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | workspaces | 否 | 

<!--Not Available on ## Microsoft.DataCatalog-->
<!--Not Available on ## Microsoft.DataConnect-->
<!--Not Available on ## Microsoft.DataExchange-->

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | datafactories | 否 | 
> | factories | 否 |  

<!--Not Available on ## Microsoft.DataLake-->
<!--Not Available on ## Microsoft.DataLakeAnalytics-->
<!--Not Available on ## Microsoft.DataLakeStore-->

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | services | 否 | 
> | services/projects | 否 | 
> | slots | 否 | 

<!--Not Available on ## Microsoft.DataProtection-->
<!--Not Available on ## Microsoft.DataShare-->

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | servers | 可以使用跨区域只读副本移动现有服务器。 [了解详细信息](../../postgresql/howto-move-regions-portal.md)。<br/><br/> 如果为服务预配了异地冗余备份存储，则可以使用异地还原在其他区域中进行还原。 [了解详细信息](../../mariadb/concepts-business-continuity.md#recover-from-an-azure-regional-data-center-outage)。

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | servers | 可以使用跨区域只读副本移动现有服务器。 [了解详细信息](../../mysql/howto-move-regions-portal.md)。

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | servergroups | 否 | 
> | servers | 可以使用跨区域只读副本移动现有服务器。 [了解详细信息](../../postgresql/howto-move-regions-portal.md)。
> | serversv2 | 否 | 

<!--Not Available on ## Microsoft.DeploymentManager-->
<!--Not Available on ## Microsoft.DesktopVirtualization-->

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | elasticpools | 否。 资源未公开。
> | elasticpools/iothubtenants | 否。 资源未公开。
> | iothubs | 是的。 [了解详细信息](../../iot-hub/iot-hub-how-to-clone.md)
> | provisioningservices | 否 | 

<!--Not Available on ## Microsoft.DevOps-->
<!--Not Available on ## Microsoft.DevSpaces-->
<!--Not Available on ## Microsoft.DevTestLab-->
<!--Not Available on ## Microsoft.DigitalTwins-->

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | databaseaccounts | 否 | 

<!--Not Available on ## Microsoft.DomainRegistration-->
<!--Not Available on ## Microsoft.EnterpriseKnowledgeGraph-->

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | domains | 否 | 
> | eventsubscriptions | 否 |
> | extensiontopics | 否 | 
> | partnernamespaces | 否 | 
> | partnerregistrations | 否 | 
> | partnertopics | 否 | 
> | systemtopics | 否 | 
> | topics | 否 | 
> | topictypes | 否 | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | clusters | 否 |  
> | namespaces | 是（带模板）<br/><br/> [将事件中心命名空间移到另一个区域](../../event-hubs/move-across-regions.md) | 
> | sku | 否 |  

<!--NOT AVAILABLE ON ## Microsoft.Experimentation-->
<!--NOT AVAILABLE ON ## Microsoft.Falcon-->

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | featureproviders | 否 | 
> | features | 否 | 
> | providers | 否 | 
> | subscriptionfeatureregistrations | 否 | 

<!--Not Available on ## Microsoft.Genomics-->

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | automanagedaccounts | 否 | 
> | automanagedvmconfigurationprofiles | 否 | 
> | guestconfigurationassignments | 否 | 
> | software | 否 | 
> | softwareupdateprofile | 否 | 
> | softwareupdates | 否 | 

<!--Not Available on ## Microsoft.HanaOnAzure-->
<!--Not Available on ## Microsoft.HardwareSecurityModules-->

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | clusters | 否 | 

<!--Not Available on ## Microsoft.HealthcareApis-->
<!--Not Available on ## Microsoft.HybridCompute-->
<!--Not Available on ## Microsoft.HybridData-->
<!--Not Available on ## Microsoft.HybridNetwork-->
<!--Not Available on ## Microsoft.Hydra-->

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | jobs |  否 | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | accounts | 不能。 [了解详细信息](../../azure-monitor/faq.md#how-do-i-move-an-application-insights-resource-to-a-new-region)。
> | actiongroups |  否 | 
> | activitylogalerts | 否 | 
> | alertrules |  否 | 
> | autoscalesettings |  否 | 
> | baseline | 否 |
> | components |  否 |  
> | datacollectionrules | 否 | 
> | diagnosticsettings | 否 | 
> | diagnosticsettingscategories | 否 | 
> | eventcategories | 否 | 
> | eventtypes | 否 | 
> | extendeddiagnosticsettings | 否 | |
> | guestdiagnosticsettings | 否 | 
> | listmigrationdate | 否 | 
> | logdefinitions | 否 | 
> | logprofiles | 否 | 
> | 日志 | 否 | 否 |
> | metricalerts | 否 | 
> | metricbaselines | 否 | 
> | metricbatch | 否 | 
> | metricdefinitions | 否 | 
> | metricnamespaces | 否 | 
> | 指标 | 否 | 
> | migratealertrules | 否 |
> | migratetonewpricingmodel | 否 | 
> | myworkbooks | 否 |
> | notificationgroups | 否 | 
> | privatelinkscopes | 否 |
> | rollbacktolegacypricingmodel | 否 |
> | scheduledqueryrules |  否 | 
> | 拓扑 | 否 |
> | 事务 | 否 |
> | vminsightsonboardingstatuses | 否 |
> | webtests |  否 | 
> | webtests/gettestresultfile | 否 |
> | workbooks |  否 |  
> | workbooktemplates | 否 |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | apptemplates | 否 | 
> | iotapps | 否 | 

<!--Not Available on ## Microsoft.IoTHub-->
<!--Not Available on ## Microsoft.IoTSpaces-->

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | deletedvaults | 否 |
> | hsmpools | 否 | 
> | vaults |  否 | 

<!--NOT AVAILABLE ON managedhsms-->

<!--NOT AVAILABLE ON ## Microsoft.Kubernetes-->
<!--NOT AVAILABLE ON## Microsoft.KubernetesConfiguration-->

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | clusters |  否 |  

<!--Not Available on ## Microsoft.LabServices-->
<!--Not Available on ## Microsoft.LocationBasedServices-->
<!--Not Available on ## Microsoft.LocationServices-->

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | hostingenvironments | 否 | 
> | integrationaccounts |  否 |  
> | integrationserviceenvironments | 否 | 
> | integrationserviceenvironments/managedapis | 否 |
> | isolatedenvironments | 否 | 
> | workflows |  否 |  

<!--Not Available on ## Microsoft.MachineLearning-->
<!--Not Available on ## Microsoft.MachineLearningCompute-->
<!--Not Available on ## Microsoft.MachineLearningExperimentation-->
<!--Not Available on ## Microsoft.MachineLearningModelManagement-->

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | workspaces | 否 | 

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | configurationassignments | 是的。 [了解详细信息](../../virtual-machines/move-region-maintenance-configuration.md) | 
> | maintenanceconfigurations | 是的。 [了解详细信息](../../virtual-machines/move-region-maintenance-configuration-resources.md) |
> | updates | 否 | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | identities | 否 | 
> | userassignedidentities | 否 | 

<!--Not Available on ## Microsoft.ManagedNetwork-->
<!--Not Available on ## Microsoft.ManagedServices-->

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | getentities | 否 | 
> | managementgroups | 否 | 
> | managementgroups / settings | 否 | 
> | resources | 否 | 
> | starttenantbackfill | 否 | 
> | tenantbackfillstatus | 否 | 

<!--Not Available on ## Microsoft.Maps-->
<!--Not Available on ## Microsoft.Marketplace-->
<!--Not Available on ## Microsoft.MarketplaceApps-->
<!--Not Available on ## Microsoft.MarketplaceOrdering-->

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | mediaservices |  否 | 
> | mediaservices/liveevents |  否 | 
> | mediaservices/streamingendpoints |  否 | 

<!--Not Available on ## Microsoft.Microservices4Spring-->

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | assessmentprojects | 否 | 
> | migrateprojects | 否 | 
> | movecollections | 否
> | projects | 否 | 

<!--Not Available on ## Microsoft.MixedReality-->
<!--Not Available on ## Microsoft.NetApp-->

<!--DDos, Front Door, and Private link service are not available on Mooncake-->
<!--MOONCAKE CUSTOMIZEIONT-->

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | applicationgateways | 否 |
> | applicationgatewaywebapplicationfirewallpolicies | 否 | 
> | applicationsecuritygroups |  否 |  
> | azurefirewalls |  否 |  
> | bastionhosts | 否 | 
> | bgpservicecommunities | 否 |
> | connections |  否 | 
> | dnszones |  否 | 
> | expressroutecircuits | 否 | 
> | expressroutegateways | 否 | 
> | expressrouteserviceproviders | 否 | 
> | firewallpolicies | 否 |
> | ipallocations | 否 |
> | ipgroups | 否 |
> | loadbalancers | 是 |
> | localnetworkgateways |  否 | 
> | natgateways |  否 | 
> | networkexperimentprofiles | 否 |
> | networkintentpolicies |  否 | 
> | networkinterfaces | 是 | 
> | networkprofiles | 否 | 
> | networksecuritygroups | 是 | 
> | networkwatchers |  否 |  
> | networkwatchers/connectionmonitors |  否 | 
> | networkwatchers/flowlogs |  否 | 
> | networkwatchers/pingmeshes |  否 | 
> | p2svpngateways | 否 | 
> | privatednszones |  否 |  
> | privatednszones/virtualnetworklinks | 否 |
> | privatednszonesinternal | 否 |
> | privateendpointredirectmaps | 否 |
> | privateendpoints | 否 | 
> | privatelinkservices | 否 | 
> | publicipaddresses | 是 |
> | publicipprefixes | 否 | 
> | routefilters | 否 | 
> | routetables |  否 | 
> | securitypartnerproviders | 否 |
> | serviceendpointpolicies |  否 | 
> | trafficmanagergeographichierarchies | 否 | 
> | trafficmanagerprofiles |  否 | 
> | trafficmanagerusermetricskeys | 否 |
> | virtualhubs | 否 | 
> | virtualnetworkgateways |  否 |  
> | virtualnetworks |  否 | 
> | virtualnetworktaps | 否 | 
> | virtualwans | 否 | 
> | vpngateways（虚拟 WAN） | 否 | 
> | vpnsites（虚拟 WAN） | 否 | 
> | vpnsites（虚拟 WAN） | 否 |

<!--DDos, Front Door, and Private link service are not available on Mooncake-->
<!--MOONCAKE CUSTOMIZEIONT-->

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | namespaces |  否 | 
> | namespaces/notificationhubs |  否 |  

<!--NOT AVAILABLE ON ## Microsoft.ObjectStore-->
<!--NOT AVAILABLE ON ## Microsoft.OffAzure-->

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | clusters | 否 | 
> | deletedworkspaces | 否 | 
> | linktargets | 否 | 
> | storageinsightconfigs | 否 |
> | workspaces | 否 |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | managementassociations | 否 |
> | managementconfigurations |  否 | 
> | solutions | 否 |
> | 视图 |  否 | 

<!--Not Available on ## Microsoft.Peering-->


## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | policyevents | 否 | 
> | policystates | 否 | 
> | policytrackedresources | 否 | 
> | remediations | 否 | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | consoles | 否 |
> | dashboards | 否 | 
> | usersettings | 否 | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | workspacecollections |  否 | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | capacities |  否 | 

<!--Not Available on ## Microsoft.Purview-->

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | availableaccounts | 否 | 
> | providerregistrations | 否 | 
> | rollouts | 否 | 

<!--Not Available on ## Microsoft.Quantum-->

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | replicationeligibilityresults | 否 |
> | vaults | 否。<br/><br/> 不支持跨 Azure 区域移动 Azure 备份的恢复服务保管库。<br/><br/>  |

<!--NOT AVAILABLE ON [disable and recreate the vault](../../site-recovery/move-vaults-across-regions.md)-->

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | namespaces |  否 | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | 查询 |  否 |  
> | resourcechangedetails | 否 | 
> | resourcechanges | 否 | 
> | resources | 否 | 
> | resourceshistory | 否 | 
> | subscriptionsstatus | 否 | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | childresources | 否 | 
> | emergingissues | 否 | 
> | events | 否 | 
> | metadata | 否 | 
> | 通知 | 否 | 

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 |
> | ------------- | ----------- |
> | templateSpecs |  是<br/><br/> |  

<!--Not Available on deploymentScripts-->

<!--Not Available on ## Microsoft.SaaS-->


## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | resourcehealthmetadata | 否 |
> | searchservices |  否 | 

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | adaptivenetworkhardenings | 否 | 
> | advancedthreatprotectionsettings | 否 | 
> | alerts | 否 | 
> | allowedconnections | 否 | 
> | applicationwhitelistings | 否 | 
> | assessmentmetadata | 否 | 
> | assessments | 否 | 
> | autodismissalertsrules | 否 | 
> | automations | 否 | 
> | autoprovisioningsettings | 否 |
> | complianceresults | 否 | 
> | compliances | 否 | 
> | datacollectionagents | 否 | 
> | devicesecuritygroups | 否 | 
> | discoveredsecuritysolutions | 否 | 
> | externalsecuritysolutions | 否 | 
> | informationprotectionpolicies | 否 | 
> | iotsecuritysolutions | 否 | 
> | iotsecuritysolutions / analyticsmodels | 否 | 
> | iotsecuritysolutions / analyticsmodels / aggregatedalerts | 否 | 
> | iotsecuritysolutions / analyticsmodels / aggregatedrecommendations | 否 | 
> | jitnetworkaccesspolicies | 否 | 
> | 策略 | 否 | 
> | pricings | 否 | 
> | regulatorycompliancestandards | 否 | 
> | regulatorycompliancestandards / regulatorycompliancecontrols | 否 | 
> | regulatorycompliancestandards / regulatorycompliancecontrols / regulatorycomplianceassessments | 否 | 
> | securitycontacts | 否 | 
> | securitysolutions | 否 | 
> | securitysolutionsreferencedata | 否 | 
> | securitystatuses | 否 | 
> | securitystatusessummaries | 否 | 
> | servervulnerabilityassessments | 否 | 
> | 设置 | 否 | 
> | subassessments | 否 |
> | 任务 | 否 | 
> | topologies | 否 | 
> | workspacesettings | 否 | 

<!--Not Available on ## Microsoft.SecurityInsights-->
<!--Not Available on ## Microsoft.SerialConsole-->
<!--Not Available on ## Microsoft.ServerManagement-->

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | namespaces |  否 | 
> | premiummessagingregions | 否 | 
> | sku | 否 | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | applications | 否 | 
> | clusters |  否 |  
> | containergroups | 否 | 
> | containergroupsets | 否 | 
> | edgeclusters | 否 | 
> | managedclusters | 否 |
> | networks | 否 | 
> | secretstores | 否 | 
> | volumes | 否 | 

<!--Not Available on ## Microsoft.ServiceFabricMesh-->
<!--Not Available on ## Microsoft.Services-->

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | signalr |  否 |  

<!--Not Available on ## Microsoft.SoftwarePlan-->

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | appliancedefinitions | 否 | 
> | appliances | 否 | 
> | jitrequests | 否 | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | instancepools | 否 | 
> | locations | 否 |
> | managedinstances | 是 <br/><br/> [详细了解](../../azure-sql/database/move-resources-across-regions.md)如何在区域之间移动托管实例。 | 
> | managedinstances/databases | 是 | 
> | servers | 是 | 
> | servers/databases | 是 <br/><br/> [详细了解](../../azure-sql/database/move-resources-across-regions.md)如何在区域之间移动数据库。<br/><br/>   | 
> | servers/elasticpools | 是 <br/><br/> [详细了解](../../azure-sql/database/move-resources-across-regions.md)如何在区域之间移动弹性池。<br/><br/>  | 
> | virtualclusters | 是 | 

<!--MOONCAKE: Not Available on [Learn more](../../resource-mover/tutorial-move-region-sql.md)-->

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  否 |  
> | sqlvirtualmachines |  否 |  

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | storageaccounts | 是<br/><br/> [将 Azure 存储帐户移到另一个区域](../../storage/common/storage-account-move.md) | 

<!--Not Available on ## Microsoft.StorageCache-->
## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | storagesyncservices |  否 | 

<!--Not Available on ## Microsoft.StorageSyncDev-->
<!--Not Available on ## Microsoft.StorageSyncInt-->
<!--Not Available on ## Microsoft.StorSimple-->

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | clusters | 否 |
> | streamingjobs |  否 |  

<!--Not Available on ## Microsoft.StreamAnalyticsExplorer-->
<!--Not Available on ## Microsoft.Subscription-->
<!--Not Available on ## Microsoft.support-->


## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- | 
> | workspaces | 否 | 
> | workspaces / bigdatapools | 否 | 
> | workspaces / sqlpools | 否 | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | environments |  否 | 
> | environments / eventsources |  否 |  
> | environments/referencedatasets |  否 | 

<!--Not Available on ## Microsoft.Token-->
<!--Not Available on ## Microsoft.VirtualMachineImages-->
<!--Not Available on ## microsoft.visualstudio-->
<!--Not Available on ## Microsoft.VMware-->
<!--Not Available on ## Microsoft.VMwareCloudSimple-->
<!--Not Available on ## Microsoft.VnfManager-->
<!--Not Available on ## Microsoft.VSOnline-->

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 资源类型 | 区域移动 | 
> | ------------- | ----------- |
> | availablestacks | 否 | 
> | billingmeters | 否 | 
> | certificates | 否 | 
> | connectiongateways |  否 |  
> | connections |  否 |  
> | customapis |  否 | 
> | deletedsites | 否 | 
> | deploymentlocations | 否 | 
> | georegions | 否 | 
> | hostingenvironments | 否 | 
> | kubeenvironments | 否 | 
> | publishingusers | 否 |
> | 建议 | 否 | 
> | resourcehealthmetadata | 否 | 
> | runtimes | 否 | 
> | serverfarms | 否 |  
> | serverfarms / eventgridfilters | N
> | sites |  否 | 
> | sites/premieraddons |  否 |  
> | sites/slots |  否 |  
> | sourcecontrols | 否 |
> | staticsites | 否 | 

<!--Not Available on ## Microsoft.WindowsESU-->
<!--Not Available on ## Microsoft.WindowsIoT-->
<!--Not Available on ## Microsoft.WorkloadBuilder-->
<!--Not Available on ## Microsoft.WorkloadMonitor-->

## <a name="third-party-services"></a>第三方服务

第三方服务目前不支持移动操作。

<!--NOT AVAILABLE ON [Learn more](../../resource-mover/overview.md)-->
<!--Update_Description: update meta properties, wording update, update link-->