---
title: 支持的 Azure 资源管理器资源类型
description: 提供 Azure Resource Graph 和更改历史记录支持的 Azure 资源管理器资源类型的列表。
origin.date: 02/04/2021
author: rockboyfor
ms.date: 03/22/2021
ms.author: v-yeche
ms.topic: reference
ms.custom: generated
ms.openlocfilehash: a27e46d177b360a5999cc80d86f0754b86a651d4
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766897"
---
# <a name="azure-resource-graph-table-and-resource-type-reference"></a>Azure Resource Graph 表格和资源类型参考

Azure Resource Graph 支持 [Azure 资源管理器](../../../azure-resource-manager/management/overview.md)的以下资源类型。 每项资源都是 Resource Graph 中表格的一部分 。

## <a name="advisorresources"></a>advisorresources

- microsoft.advisor/configurations
- microsoft.advisor/recommendations
- microsoft.advisor/recommendations/suppressions
- microsoft.advisor/suppressions

## <a name="alertsmanagementresources"></a>alertsmanagementresources

- microsoft.alertsmanagement/alerts

## <a name="guestconfigurationresources"></a>guestconfigurationresources

- microsoft.guestconfiguration/guestconfigurationassignments

## <a name="maintenanceresources"></a>maintenanceresources

- microsoft.maintenance/configurationassignments
- microsoft.maintenance/updates

## <a name="patchassessmentresources"></a>patchassessmentresources

- microsoft.compute/virtualmachines/patchassessmentresults
- microsoft.compute/virtualmachines/patchassessmentresults/softwarepatches

<!--NOT AVAILBLE ON - microsoft.hybridcompute/machines/patchassessmentresults-->
<!--NOT AVAILBLE ON - microsoft.hybridcompute/machines/patchassessmentresults/softwarepatches-->

## <a name="patchinstallationresources"></a>patchinstallationresources

- microsoft.compute/virtualmachines/patchinstallationresults
- microsoft.compute/virtualmachines/patchinstallationresults/softwarepatches

<!--NOT AVAILBLE ON - microsoft.hybridcompute/machines/patchinstallationresults-->
<!--NOT AVAILBLE ON - microsoft.hybridcompute/machines/patchinstallationresults/softwarepatches-->

## <a name="policyresources"></a>policyresources

- microsoft.policyinsights/policystates

## <a name="recoveryservicesresources"></a>recoveryservicesresources

<!--NOT AVAILBLE ON - microsoft.dataprotection/backupvaults/backupinstances-->
<!--NOT AVAILBLE ON - microsoft.dataprotection/backupvaults/backupjobs-->
<!--NOT AVAILBLE ON - microsoft.dataprotection/backupvaults/backuppolicies-->

- microsoft.recoveryservices/vaults/alerts
- Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems（备份项）
- microsoft.recoveryservices/vaults/backupjobs
- microsoft.recoveryservices/vaults/backuppolicies

## <a name="resourcecontainers"></a>resourcecontainers

- microsoft.resources/subscriptions（订阅）
- Microsoft.Resources/subscriptions/resourceGroups（资源组）

## <a name="resources"></a>resources

<!--NOT AVAILBLE ON - 84codes.CloudAMQP/servers (CloudAMQP)-->
<!--NOT AVAILBLE ON - Citrix.Services/XenAppEssentials (Citrix Virtual Apps Essentials)-->
<!--NOT AVAILBLE ON - Citrix.Services/XenDesktopEssentials (Citrix Virtual Desktops Essentials)-->
<!--NOT AVAILBLE ON - Conexlink.MyCloudIt/accounts (MyCloudIT - Azure Desktop Hosting)-->
<!--NOT AVAILBLE ON - Crypteron.DataSecurity/apps (Crypteron)-->
<!--NOT AVAILBLE ON - gridpro.evops/accounts-->
<!--NOT AVAILBLE ON - gridpro.evops/accounts/eventrules-->
<!--NOT AVAILBLE ON - gridpro.evops/accounts/requesttemplates-->
<!--NOT AVAILBLE ON - gridpro.evops/accounts/views-->
<!--NOT AVAILBLE ON - Hive.Streaming/services (Hive Streaming)-->
<!--NOT AVAILBLE ON - incapsula.waf/accounts-->
<!--NOT AVAILBLE ON - LiveArena.Broadcast/services-->
<!--NOT AVAILBLE ON - Mailjet.Email/services-->

- Microsoft.AAD/domainServices（Azure AD 域服务）
- microsoft.aadiam/azureadmetrics
    
    <!--NOT AVAILBLE ON - microsoft.aadiam/privateLinkForAzureAD (Private Link for Azure AD)-->
    
- microsoft.aadiam/tenants

    <!--NOT AVAILBLE ON - microsoft.agfoodplatform/farmbeats-->
    <!--NOT AVAILBLE ON - microsoft.aisupercomputer/accounts-->
    <!--NOT AVAILBLE ON - microsoft.aisupercomputer/accounts/jobgroups-->
    <!--NOT AVAILBLE ON - microsoft.aisupercomputer/accounts/jobgroups/jobs-->
    
- microsoft.alertsmanagement/actionrules
- microsoft.alertsmanagement/resourcehealthalertrules
- microsoft.alertsmanagement/smartdetectoralertrules
- Microsoft.AnalysisServices/servers (Analysis Services)

    <!--NOT AVAILBLE ON - microsoft.anybuild/clusters-->
    
- Microsoft.ApiManagement/service（API 管理服务）
    
    <!--NOT AVAILBLE ON - microsoft.appassessment/migrateprojects-->

- Microsoft.AppConfiguration/configurationStores（应用配置）
- Microsoft.AppPlatform/Spring (Azure Spring Cloud)
    
    <!--NOT AVAILBLE ON - microsoft.archive/collections-->
    <!--NOT AVAILBLE ON - Microsoft.Attestation/attestationProviders (Attestation providers)-->
    
- Microsoft.Authorization/resourceManagementPrivateLinks（资源管理专用链接）
    
    <!--NOT AVAILBLE ON - microsoft.automanage/accounts-->
    <!--NOT AVAILBLE ON - microsoft.automanage/configurationprofilepreferences-->

- Microsoft.Automation/AutomationAccounts（自动化帐户）
- microsoft.automation/automationaccounts/configurations
- Microsoft.Automation/automationAccounts/runbooks (Runbook)

    <!--NOT AVAILBLE ON - microsoft.autonomousdevelopmentplatform/accounts-->
    <!--NOT AVAILBLE ON - Microsoft.AutonomousSystems/workspaces (Bonsai)-->
    <!--NOT AVAILBLE ON - Microsoft.AVS/privateClouds (AVS Private clouds)-->
    <!--NOT AVAILBLE ON - microsoft.azconfig/configurationstores-->
    
- Microsoft.AzureActiveDirectory/b2cDirectories（B2C 租户）
- Microsoft.AzureActiveDirectory/guestUsages（来宾使用情况）

    <!--NOT AVAILBLE ON - Microsoft.AzureArcData/dataControllers (Azure Arc data controllers)-->
    <!--NOT AVAILBLE ON - Microsoft.AzureArcData/postgresInstances (Azure Database for PostgreSQL server groups - Azure Arc)-->
    <!--NOT AVAILBLE ON - Microsoft.AzureArcData/sqlManagedInstances (SQL managed instances - Azure Arc)-->
    <!--NOT AVAILBLE ON - Microsoft.AzureArcData/sqlServerInstances (SQL Server - Azure Arc)-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/datacontrollers-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/hybriddatamanagers-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/postgresinstances-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/sqlbigdataclusters-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/sqlinstances-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/sqlmanagedinstances-->
    <!--NOT AVAILBLE ON - microsoft.azuredata/sqlserverinstances-->
    <!--NOT AVAILBLE ON - Microsoft.AzureData/sqlServerRegistrations (SQL Server registries)-->
    
- microsoft.azurestack/edgesubscriptions
- microsoft.azurestack/linkedsubscriptions
- Microsoft.Azurestack/registrations (Azure Stack Hub)
- Microsoft.AzureStackHCI/clusters (Azure Stack HCI)
- microsoft.azurestackhci/galleryimages
- microsoft.azurestackhci/networkinterfaces
- microsoft.azurestackhci/virtualnetworks

    <!--NOT AVAILBLE ON - microsoft.baremetal/consoleconnections-->
    <!--NOT AVAILBLE ON - Microsoft.BareMetal/crayServers (Cray Servers)-->
    <!--NOT AVAILBLE ON - Microsoft.BareMetal/monitoringServers (Monitoring Servers)-->
    <!--NOT AVAILBLE ON - Microsoft.BareMetalInfrastructure/bareMetalInstances (BareMetal Instances)-->
    
- Microsoft.Batch/batchAccounts（Batch 帐户）
    
    <!--NOT AVAILBLE ON - microsoft.batchai/clusters-->
    <!--NOT AVAILBLE ON - microsoft.batchai/fileservers-->
    <!--NOT AVAILBLE ON - microsoft.batchai/jobs-->
    <!--NOT AVAILBLE ON - microsoft.batchai/workspaces-->
    <!--NOT AVAILBLE ON - Microsoft.Bing/accounts (Bing Resources)-->
    <!--NOT AVAILBLE ON - Microsoft.BingMaps/mapApis (Bing Maps API for Enterprise)-->
    <!--NOT AVAILBLE ON - microsoft.biztalkservices/biztalk-->
    <!--NOT AVAILBLE ON - Microsoft.Blockchain/blockchainMembers (Azure Blockchain Service)-->
    <!--NOT AVAILBLE ON - Microsoft.Blockchain/cordaMembers (Corda)-->
    <!--NOT AVAILBLE ON - Microsoft.Blockchain/watchers (Blockchain Data Manager)-->
    <!--NOT AVAILBLE ON - Microsoft.BotService/botServices (Bot Services)-->
    
- Microsoft.Cache/Redis (Azure Cache for Redis)
- Microsoft.Cache/RedisEnterprise (Redis Enterprise)
- Microsoft.Cdn/CdnWebApplicationFirewallPolicies（Web 应用程序防火墙策略 (WAF)）
- microsoft.cdn/profiles（CDN 配置文件）
- microsoft.cdn/profiles/afdendpoints
- microsoft.cdn/profiles/endpoints（终结点）
    
    <!--NOT AVAILBLE ON - Microsoft.CertificateRegistration/certificateOrders (App Service Certificates)-->
    <!--NOT AVAILBLE ON - microsoft.chaos/chaosexperiments-->
    
- microsoft.classicCompute/domainNames（云服务[经典]）
- Microsoft.ClassicCompute/VirtualMachines（虚拟机[经典]）
- Microsoft.ClassicNetwork/networkSecurityGroups（网络安全组[经典]）
- Microsoft.ClassicNetwork/reservedIps（保留 IP 地址[经典]）
- Microsoft.ClassicNetwork/virtualNetworks（虚拟网络[经典]）
- Microsoft.ClassicStorage/StorageAccounts（存储帐户[经典]）
- microsoft.cloudes/accounts
- microsoft.cloudsearch/indexes

    <!--NOT AVAILBLE ON - Microsoft.CloudTest/accounts (CloudTest Accounts)-->
    <!--NOT AVAILBLE ON - Microsoft.CloudTest/hostedpools (1ES Hosted Pools)-->
    <!--NOT AVAILBLE ON - Microsoft.CloudTest/images (CloudTest Images)-->
    <!--NOT AVAILBLE ON - Microsoft.CloudTest/pools (CloudTest Pools)-->
    <!--NOT AVAILBLE ON - microsoft.codespaces/plans-->
    <!--NOT AVAILBLE ON - Microsoft.Cognition/syntheticsAccounts (Synthetics Accounts)-->
    
- Microsoft.CognitiveServices/accounts（认知服务）
- Microsoft.Compute/availabilitySets （可用性集）
- microsoft.compute/capacityreservationgroups
- microsoft.compute/capacityreservationgroups/capacityreservations
- microsoft.compute/capacityreservations
- Microsoft.Compute/cloudServices（云服务[外延支持]）
- Microsoft.Compute/diskAccesses（磁盘访问）
- Microsoft.Compute/diskEncryptionSets（磁盘加密集）
- Microsoft.Compute/disks（磁盘）
- Microsoft.Compute/galleries（共享映像库）
- microsoft.compute/galleries/applications
- microsoft.compute/galleries/applications/versions
- Microsoft.Compute/galleries/images（映像定义）
- Microsoft.Compute/galleries/images/versions（映像版本）
- Microsoft.Compute/hostgroups（主机组）
- Microsoft.Compute/hostgroups/hosts（主机）
- Microsoft.Compute/images（映像）
- Microsoft.Compute/ProximityPlacementGroups（邻近放置组）
- microsoft.compute/restorepointcollections
- microsoft.compute/sharedvmextensions
- microsoft.compute/sharedvmextensions/versions
- microsoft.compute/sharedvmimages
- microsoft.compute/sharedvmimages/versions
- Microsoft.Compute/snapshots（快照）
- Microsoft.Compute/sshPublicKeys（SSH 密钥）
- microsoft.compute/swiftlets
- Microsoft.Compute/VirtualMachines（虚拟机）
- microsoft.compute/virtualmachines/extensions
- microsoft.compute/virtualmachines/runcommands
- Microsoft.Compute/virtualMachineScaleSets（虚拟机规模集）

    <!--NOT AVAILBLE ON - Microsoft.Confluent/organizations (Confluent Organizations)-->
    <!--NOT AVAILBLE ON - Microsoft.ConnectedCache/cacheNodes (Connected Cache Resources)-->
    <!--NOT AVAILBLE ON - microsoft.connectedvehicle/platformaccounts-->
    
- Microsoft.ContainerInstance/containerGroups（容器实例）
- Microsoft.ContainerRegistry/registries（容器注册表）
- microsoft.containerregistry/registries/agentpools
- microsoft.containerregistry/registries/buildtasks
- Microsoft.ContainerRegistry/registries/replications（容器注册表复制）
- microsoft.containerregistry/registries/taskruns
- microsoft.containerregistry/registries/tasks
- Microsoft.ContainerRegistry/registries/webhooks（容器注册表 Webhook）
- Microsoft.ContainerService/containerServices（容器服务[已弃用]）
- Microsoft.ContainerService/managedClusters（Kubernetes 服务）
- microsoft.containerservice/openshiftmanagedclusters
- microsoft.contoso/clusters
- microsoft.contoso/employees
- microsoft.contoso/towers
- microsoft.costmanagement/connectors
- microsoft.customproviders/resourceproviders
- microsoft.d365customerinsights/instances
- Microsoft.DataBox/jobs (Data Box)
    
    <!--NOT AVAILBLE ON - Microsoft.DataBoxEdge/dataBoxEdgeDevices (Azure Stack Edge / Data Box Gateway)-->
    
- Microsoft.Databricks/workspaces（Azure Databricks 服务）

    <!--NOT AVAILBLE ON - Microsoft.DataCatalog/catalogs (Data Catalog)-->
    <!--NOT AVAILBLE ON - microsoft.datacatalog/datacatalogs-->
    <!--NOT AVAILBLE ON - Microsoft.DataCollaboration/workspaces (Data Collaborations)-->
    <!--NOT AVAILBLE ON - Microsoft.Datadog/monitors (Datadog)-->
    
- Microsoft.DataFactory/dataFactories（数据工厂）
- Microsoft.DataFactory/factories（数据工厂 (V2)）
- Microsoft.DataLakeAnalytics/accounts (Data Lake Analytics)
- Microsoft.DataLakeStore/accounts (Data Lake Storage Gen1)
- microsoft.datamigration/controllers
- Microsoft.DataMigration/services（Azure 数据库迁移服务）
- Microsoft.DataMigration/services/projects（Azure 数据库迁移项目）
- microsoft.datamigration/slots

    <!--NOT AVAILBLE ON - Microsoft.DataProtection/BackupVaults (Backup vaults)-->
    <!--NOT AVAILBLE ON - microsoft.dataprotection/resourceoperationgatekeepers-->
    <!--NOT AVAILBLE ON - Microsoft.DataShare/accounts (Data Shares)-->
    
- Microsoft.DBforMariaDB/servers（Azure Database for MariaDB 服务器）
- Microsoft.DBforMySQL/flexibleServers（Azure Database for MySQL 灵活服务器）
- Microsoft.DBforMySQL/servers（Azure Database for MySQL 服务器）
- Microsoft.DBforPostgreSQL/flexibleServers（Azure Database for PostgreSQL 灵活服务器）
- Microsoft.DBforPostgreSQL/serverGroups（Azure Database for PostgreSQL 服务器组）
- Microsoft.DBforPostgreSQL/servers（Azure Database for PostgreSQL 服务器）
- Microsoft.DBforPostgreSQL/serversv2（Azure Database for PostgreSQL 服务器 v2）
- microsoft.dbforpostgresql/singleservers

    <!--NOT AVAILBLE ON - microsoft.delegatednetwork/controller-->
    <!--NOT AVAILBLE ON - microsoft.delegatednetwork/delegatedsubnets-->
    <!--NOT AVAILBLE ON - microsoft.delegatednetwork/orchestratorinstances-->
    <!--NOT AVAILBLE ON - microsoft.deploymentmanager/artifactsources-->
    <!--NOT AVAILBLE ON - Microsoft.DeploymentManager/Rollouts (Rollouts)-->
    <!--NOT AVAILBLE ON - microsoft.deploymentmanager/servicetopologies-->
    <!--NOT AVAILBLE ON - microsoft.deploymentmanager/servicetopologies/services-->
    <!--NOT AVAILBLE ON - microsoft.deploymentmanager/servicetopologies/services/serviceunits-->
    <!--NOT AVAILBLE ON - microsoft.deploymentmanager/steps-->
    <!--NOT AVAILBLE ON - Microsoft.DesktopVirtualization/ApplicationGroups (Application groups)-->
    <!--NOT AVAILBLE ON - Microsoft.DesktopVirtualization/HostPools (Host pools)-->
    <!--NOT AVAILBLE ON - microsoft.desktopvirtualization/scalingplans-->
    <!--NOT AVAILBLE ON - Microsoft.DesktopVirtualization/Workspaces (Workspaces)-->
    
- microsoft.devices/elasticpools
- microsoft.devices/elasticpools/iothubtenants
- Microsoft.Devices/IotHubs（IoT 中心）
- Microsoft.Devices/ProvisioningServices（设备预配服务）

    <!--NOT AVAILBLE ON - Microsoft.DeviceUpdate/Accounts (Device Update for IoT Hubs)-->
    <!--NOT AVAILBLE ON - microsoft.deviceupdate/accounts/instances-->
    <!--NOT AVAILBLE ON - microsoft.devops/pipelines (DevOps Starter)-->
    <!--NOT AVAILBLE ON - microsoft.devspaces/controllers-->
    <!--NOT AVAILBLE ON - microsoft.devtestlab/labcenters-->
    <!--NOT AVAILBLE ON - Microsoft.DevTestLab/labs (DevTest Labs)-->
    <!--NOT AVAILBLE ON - microsoft.devtestlab/labs/servicerunners-->
    <!--NOT AVAILBLE ON - Microsoft.DevTestLab/labs/virtualMachines (Virtual machines)-->
    <!--NOT AVAILBLE ON - microsoft.devtestlab/schedules-->
    <!--NOT AVAILBLE ON - Microsoft.DigitalTwins/digitalTwinsInstances (Azure Digital Twins)-->
    
- Microsoft.DocumentDb/databaseAccounts（Azure Cosmos DB 帐户）

    <!--NOT AVAILBLE ON - Microsoft.DomainRegistration/domains (App Service Domains)-->
    <!--NOT AVAILBLE ON - Microsoft.Elastic/monitors (Elastic)-->
    <!--NOT AVAILBLE ON - microsoft.enterpriseknowledgegraph/services-->
    
- Microsoft.EventGrid/domains（事件网格域）
- Microsoft.EventGrid/partnerNamespaces（事件网格合作伙伴命名空间）
- Microsoft.EventGrid/partnerRegistrations（事件网格合作伙伴注册）
- Microsoft.EventGrid/partnerTopics（事件网格合作伙伴主题）
- Microsoft.EventGrid/systemTopics（事件网格系统主题）
- Microsoft.EventGrid/topics（事件网格主题）
- Microsoft.EventHub/clusters（事件中心群集）
- Microsoft.EventHub/namespaces（事件中心命名空间）

    <!--NOT AVAILBLE ON - Microsoft.Experimentation/experimentWorkspaces (Experiment Workspaces)-->
    <!--NOT AVAILBLE ON - Microsoft.ExtendedLocation/CustomLocations (Custom Locations)-->
    <!--NOT AVAILBLE ON - microsoft.falcon/namespaces-->
    <!--NOT AVAILBLE ON - microsoft.footprintmonitoring/profiles-->
    <!--NOT AVAILBLE ON - microsoft.gaming/titles-->
    <!--NOT AVAILBLE ON - Microsoft.Genomics/accounts (Genomics accounts)-->
    
- microsoft.guestconfiguration/automanagedaccounts

    <!--NOT AVAILBLE ON - Microsoft.HanaOnAzure/hanaInstances (SAP HANA on Azure)-->
    <!--NOT AVAILBLE ON - Microsoft.HanaOnAzure/sapMonitors (Azure Monitors for SAP Solutions)-->
    
- microsoft.hardwaresecuritymodules/dedicatedhsms
- Microsoft.HDInsight/clusters（HDInsight 群集）

    <!--NOT AVAILBLE ON - Microsoft.HealthBot/healthBots (Azure Health Bot)-->
    <!--NOT AVAILBLE ON - Microsoft.HealthcareApis/services (Azure API for FHIR)-->
    <!--NOT AVAILBLE ON - microsoft.healthcareapis/services/privateendpointconnections-->
    <!--NOT AVAILBLE ON - microsoft.healthcareapis/workspaces-->
    <!--NOT AVAILBLE ON - microsoft.healthcareapis/workspaces/dicomservices-->
    <!--NOT AVAILBLE ON - Microsoft.HybridCompute/machines (Servers - Azure Arc)-->
    <!--NOT AVAILBLE ON - microsoft.hybridcompute/machines/extensions-->
    <!--NOT AVAILBLE ON - Microsoft.HybridCompute/privateLinkScopes (Azure Arc Private Link Scopes)-->
    <!--NOT AVAILBLE ON - Microsoft.HybridData/dataManagers (StorSimple Data Managers)-->
    <!--NOT AVAILBLE ON - Microsoft.HybridNetwork/devices (Azure Network Function Manager – Devices)-->
    <!--NOT AVAILBLE ON - Microsoft.HybridNetwork/networkFunctions (Azure Network Function Manager – Network Functions)-->
    <!--NOT AVAILBLE ON - microsoft.hybridnetwork/virtualnetworkfunctions-->
    
- Microsoft.ImportExport/jobs（导入/导出作业）

    <!--NOT AVAILBLE ON - microsoft.industrydatalifecycle/basemodels-->
    <!--NOT AVAILBLE ON - microsoft.industrydatalifecycle/custodiancollaboratives-->
    <!--NOT AVAILBLE ON - microsoft.industrydatalifecycle/derivedmodels-->
    <!--NOT AVAILBLE ON - microsoft.industrydatalifecycle/membercollaboratives-->
    <!--NOT AVAILBLE ON - microsoft.industrydatalifecycle/modelmappings-->
    <!--NOT AVAILBLE ON - microsoft.industrydatalifecycle/pipelinesets-->
    
- microsoft.insights/actiongroups
- microsoft.insights/activitylogalerts
- microsoft.insights/alertrules
- microsoft.insights/autoscalesettings
- microsoft.insights/components (Application Insights)
- microsoft.insights/datacollectionrules（数据收集规则）
- microsoft.insights/guestdiagnosticsettings
- microsoft.insights/metricalerts
- microsoft.insights/notificationgroups
- microsoft.insights/notificationrules
- Microsoft.Insights/privateLinkScopes（Azure Monitor 专用链接范围）
- microsoft.insights/querypacks
- microsoft.insights/scheduledqueryrules
- microsoft.insights/webtests（可用性测试）
- microsoft.insights/workbooks（Azure 工作簿）
- microsoft.insights/workbooktemplates（Azure 工作簿模板）

    <!--NOT AVAILBLE ON - Microsoft.IntelligentITDigitalTwin/digitalTwins (Minervas)-->
    <!--NOT AVAILBLE ON - microsoft.intelligentitdigitaltwin/digitaltwins/assets-->
    <!--NOT AVAILBLE ON - - microsoft.intelligentitdigitaltwin/digitaltwins/executionplans-->
    <!--NOT AVAILBLE ON - - microsoft.intelligentitdigitaltwin/digitaltwins/testplans-->
    <!--NOT AVAILBLE ON - - microsoft.intelligentitdigitaltwin/digitaltwins/tests-->
    
- Microsoft.IoTCentral/IoTApps（IoT Central 应用程序）
- Microsoft.IoTSpaces/Graph（数字孪生[已弃用]）
- microsoft.keyvault/hsmpools
- microsoft.keyvault/managedhsms
- Microsoft.KeyVault/vaults（密钥保管库）

    <!--NOT AVAILBLE ON - Microsoft.Kubernetes/connectedClusters (Kubernetes - Azure Arc)-->
    
- Microsoft.Kusto/clusters（Azure 数据资源管理器群集）
- Microsoft.Kusto/clusters/databases（Azure 数据资源管理器数据库）

    <!--NOT AVAILBLE ON - Microsoft.LabServices/labAccounts (Lab Services)-->
    <!--NOT AVAILBLE ON - Microsoft.LoadTestService/LoadTests (Cloud Native Load Tests)-->
    
- Microsoft.Logic/integrationAccounts（集成帐户）
- Microsoft.Logic/integrationServiceEnvironments（集成服务环境）
- Microsoft.Logic/integrationServiceEnvironments/managedApis（托管连接器）
- Microsoft.Logic/workflows（逻辑应用）

    <!--NOT AVAILBLE ON - Microsoft.Logz/monitors (Logz Main Account)-->
    <!--NOT AVAILBLE ON - Microsoft.Logz/monitors/accounts (Logz SubAccount)-->
    <!--NOT AVAILBLE ON - Microsoft.MachineLearning/commitmentPlans (Machine Learning Studio (classic) web service plans)-->
    <!--NOT AVAILBLE ON - Microsoft.MachineLearning/webServices (Machine Learning Studio (classic) web services)-->
    <!--NOT AVAILBLE ON - Microsoft.MachineLearning/workspaces (Machine Learning Studio (classic) workspaces)-->
    <!--NOT AVAILBLE ON - microsoft.machinelearningcompute/operationalizationclusters-->
    
- microsoft.machinelearningservices/modelinventories
- microsoft.machinelearningservices/modelinventory
- Microsoft.MachineLearningServices/workspaces（机器学习）
- microsoft.machinelearningservices/workspaces/batchendpoints
- microsoft.machinelearningservices/workspaces/batchendpoints/deployments
- microsoft.machinelearningservices/workspaces/inferenceendpoints
- microsoft.machinelearningservices/workspaces/inferenceendpoints/deployments
- Microsoft.MachineLearningServices/workspaces/onlineEndpoints（ML 应用）
- Microsoft.MachineLearningServices/workspaces/onlineEndpoints/deployments（ML 应用部署）
- Microsoft.Maintenance/maintenanceConfigurations（维护配置）
- microsoft.maintenance/maintenancepolicies
- microsoft.managedidentity/groups
- Microsoft.ManagedIdentity/userAssignedIdentities（托管标识）
- microsoft.managednetwork/managednetworkgroups

    <!--NOT AVAILBLE ON - microsoft.managednetwork/managednetworkpeeringpolicies-->
    <!--NOT AVAILBLE ON - microsoft.managednetwork/managednetworks-->
    <!--NOT AVAILBLE ON - microsoft.managednetwork/managednetworks/managednetworkgroups-->
    <!--NOT AVAILBLE ON - microsoft.managednetwork/managednetworks/managednetworkpeeringpolicies-->
    <!--NOT AVAILBLE ON - Microsoft.Maps/accounts (Azure Maps Accounts)-->
    <!--NOT AVAILBLE ON - microsoft.maps/accounts/creators-->
    <!--NOT AVAILBLE ON - Microsoft.Maps/accounts/privateAtlases (Azure Maps Creator Resources)-->
    <!--NOT AVAILBLE ON - Microsoft.MarketplaceApps/classicDevServices (Classic Dev Services)-->
    
- microsoft.media/mediaservices（媒体服务）
- microsoft.media/mediaservices/liveevents（直播活动）
- microsoft.media/mediaservices/streamingEndpoints（流式处理终结点）
- microsoft.media/mediaservices/transforms

    <!--NOT AVAILBLE ON - microsoft.microservices4spring/appclusters-->
    <!--NOT AVAILBLE ON - microsoft.migrate/assessmentprojects-->
    <!--NOT AVAILBLE ON - microsoft.migrate/migrateprojects-->
    <!--NOT AVAILBLE ON - microsoft.migrate/movecollections-->
    <!--NOT AVAILBLE ON - Microsoft.Migrate/projects (Migration projects)-->
    <!--NOT AVAILBLE ON - Microsoft.MixedReality/holographicsBroadcastAccounts (Holographics Broadcast Accounts)-->
    <!--NOT AVAILBLE ON - Microsoft.MixedReality/objectUnderstandingAccounts (Object Understanding Accounts)-->
    <!--NOT AVAILBLE ON - Microsoft.MixedReality/remoteRenderingAccounts (Remote Rendering Accounts)-->
    <!--NOT AVAILBLE ON - Microsoft.MixedReality/spatialAnchorsAccounts (Spatial Anchors Accounts)-->
    <!--NOT AVAILBLE ON - microsoft.mixedreality/surfacereconstructionaccounts-->
    <!--NOT AVAILBLE ON - Microsoft.NetApp/netAppAccounts (NetApp accounts)-->
    <!--NOT AVAILBLE ON - microsoft.netapp/netappaccounts/backuppolicies-->
    <!--NOT AVAILBLE ON - Microsoft.NetApp/netAppAccounts/capacityPools (Capacity pools)-->
    <!--NOT AVAILBLE ON - Microsoft.NetApp/netAppAccounts/capacityPools/Volumes (Volumes)-->
    <!--NOT AVAILBLE ON - microsoft.netapp/netappaccounts/capacitypools/volumes/mounttargets-->
    <!--NOT AVAILBLE ON - Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots (Snapshots)-->
    
- Microsoft.Network/applicationGateways（应用程序网关数）
- Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies（Web 应用程序防火墙策略 (WAF)）
- Microsoft.Network/applicationSecurityGroups（应用程序安全组）
- Microsoft.Network/azureFirewalls（防火墙）
- Microsoft.Network/bastionHosts (Bastions)
- Microsoft.Network/connections（连接）
- microsoft.network/customipprefixes
- microsoft.network/ddoscustompolicies
- Microsoft.Network/ddosProtectionPlans（DDoS 防护计划）
- Microsoft.Network/dnsZones（DNS 区域）
- microsoft.network/dscpconfigurations
- Microsoft.Network/expressRouteCircuits（ExpressRoute 线路）
- microsoft.network/expressroutecrossconnections
- microsoft.network/expressroutegateways
- Microsoft.Network/expressRoutePorts (ExpressRoute Direct)
- Microsoft.Network/firewallPolicies（防火墙策略）
- Microsoft.Network/frontdoors (Front Door)
- Microsoft.Network/FrontDoorWebApplicationFirewallPolicies（Web 应用程序防火墙策略 (WAF)）
- microsoft.network/ipallocations
- Microsoft.Network/ipGroups（IP 组）
- Microsoft.Network/LoadBalancers（负载均衡器）
- Microsoft.Network/localnetworkgateways（本地网关）
- microsoft.network/mastercustomipprefixes
- Microsoft.Network/natGateways（NAT 网关）
- Microsoft.Network/NetworkExperimentProfiles（Internet 分析器配置文件）
- microsoft.network/networkintentpolicies
- Microsoft.Network/networkinterfaces（网络接口）
- Microsoft.Network/networkManagers（网络管理器）
- microsoft.network/networkprofiles
- Microsoft.Network/NetworkSecurityGroups（网络安全组）
- microsoft.network/networkvirtualappliances
- microsoft.network/networkwatchers（网络观察程序）
- microsoft.network/networkwatchers/connectionmonitors
- microsoft.network/networkwatchers/flowlogs（NSG 流日志）
- microsoft.network/networkwatchers/lenses
- microsoft.network/networkwatchers/pingmeshes
- microsoft.network/p2svpngateways
- Microsoft.Network/privateDnsZones （专用 DNS 区域）
- microsoft.network/privatednszones/virtualnetworklinks
- microsoft.network/privateendpointredirectmaps
- Microsoft.Network/privateEndpoints（专用终结点）
- Microsoft.Network/privateLinkServices（专用链接服务）
- Microsoft.Network/PublicIpAddresses（公共 IP 地址）
- Microsoft.Network/publicIpPrefixes（公共 IP 前缀）
- Microsoft.Network/routeFilters（路由筛选器）
- Microsoft.Network/routeTables（路由表）
- microsoft.network/sampleresources
- microsoft.network/securitypartnerproviders
- Microsoft.Network/serviceEndpointPolicies（服务终结点策略）
- Microsoft.Network/trafficmanagerprofiles（流量管理器配置文件）
- microsoft.network/virtualhubs
- microsoft.network/virtualhubs/bgpconnections
- microsoft.network/virtualhubs/ipconfigurations
- Microsoft.Network/virtualNetworkGateways（虚拟网络网关）
- Microsoft.Network/virtualNetworks（虚拟网络）
- microsoft.network/virtualnetworktaps
- microsoft.network/virtualrouters
- Microsoft.Network/virtualWans（虚拟 WAN）
- microsoft.network/vpngateways
- microsoft.network/vpnserverconfigurations
- microsoft.network/vpnsites
- Microsoft.NotificationHubs/namespaces（通知中心命名空间）
- Microsoft.NotificationHubs/namespaces/notificationHubs（通知中心）

    <!--NOT AVAILBLE ON - microsoft.nutanix/interfaces-->
    <!--NOT AVAILBLE ON - microsoft.nutanix/nodes-->
    <!--NOT AVAILBLE ON - microsoft.objectstore/osnamespaces-->
    <!--NOT AVAILBLE ON - microsoft.offazure/hypervsites-->
    <!--NOT AVAILBLE ON - microsoft.offazure/importsites-->
    <!--NOT AVAILBLE ON - microsoft.offazure/mastersites-->
    <!--NOT AVAILBLE ON - microsoft.offazure/serversites-->
    <!--NOT AVAILBLE ON - microsoft.offazure/vmwaresites-->
    <!--NOT AVAILBLE ON - Microsoft.OpenLogisticsPlatform/workspaces (Open Supply Chain Platform)-->
    
- microsoft.operationalinsights/clusters
- Microsoft.OperationalInsights/querypacks（Log Analytics 查询包）
- Microsoft.OperationalInsights/workspaces（Log Analytics 工作区）
- Microsoft.OperationsManagement/solutions（解决方案）
- microsoft.operationsmanagement/views

    <!--NOT AVAILBLE ON - microsoft.orbital/contactprofiles-->
    <!--NOT AVAILBLE ON - microsoft.orbital/spacecrafts-->
    <!--NOT AVAILBLE ON - Microsoft.Peering/peerings (Peerings)-->
    <!--NOT AVAILBLE ON - Microsoft.Peering/peeringServices (Peering Services)-->
    
- Microsoft.Portal/dashboards（共享仪表板）

    <!--NOT AVAILBLE ON - microsoft.portalsdk/rootresources-->
    
- microsoft.powerbi/privatelinkservicesforpowerbi
- microsoft.powerbi/tenants
- microsoft.powerbi/workspacecollections
- Microsoft.PowerBIDedicated/capacities (Power BI Embedded)

    <!--NOT AVAILBLE ON - Microsoft.ProjectBabylon/Accounts (Babylon accounts)-->
    <!--NOT AVAILBLE ON - Microsoft.Purview/Accounts (Purview accounts)-->
    <!--NOT AVAILBLE ON - Microsoft.Quantum/Workspaces (Quantum Workspaces)-->
    
- Microsoft.RecoveryServices/vaults（恢复服务保管库）

    <!--NOT AVAILBLE ON - Microsoft.RedHatOpenShift/openShiftClusters (OpenShift clusters)-->
    
- Microsoft.Relay/namespaces（中继）

    <!--NOT AVAILBLE ON - microsoft.remoteapp/collections-->
    <!--NOT AVAILBLE ON - microsoft.resiliency/chaosexperiments-->
    <!--NOT AVAILBLE ON - microsoft.resourceconnector/appliances-->
    
- Microsoft.resourcegraph/queries（Resource Graph 查询）
- Microsoft.Resources/deploymentScripts（部署脚本）
- Microsoft.Resources/templateSpecs（模板规格）
- microsoft.resources/templatespecs/versions

    <!--NOT AVAILBLE ON - Microsoft.SaaS/applications (Software as a Service (classic))-->
    <!--NOT AVAILBLE ON - Microsoft.SaaS/resources (CPX-Placeholder)-->
    
- Microsoft.Scheduler/jobCollections（计划程序作业集合）

    <!--NOT AVAILBLE ON - microsoft.scvmm/clouds-->
    <!--NOT AVAILBLE ON - Microsoft.scvmm/virtualMachines (SCVMM virtual machine - Azure Arc)-->
    <!--NOT AVAILBLE ON - microsoft.scvmm/virtualmachinetemplates-->
    <!--NOT AVAILBLE ON - microsoft.scvmm/virtualnetworks-->
    <!--NOT AVAILBLE ON - microsoft.scvmm/vmmservers-->
    
- Microsoft.Search/searchServices（搜索服务）
- microsoft.security/automations
- microsoft.security/iotsecuritysolutions
- Microsoft.SecurityDetonation/chambers（安全引爆区域）
- Microsoft.ServiceBus/namespaces（服务总线命名空间）
- Microsoft.ServiceFabric/clusters（Service Fabric 群集）
- microsoft.servicefabric/containergroupsets
- Microsoft.ServiceFabric/managedclusters（托管的 Service Fabric 群集）

    <!--NOT AVAILBLE ON - Microsoft.ServiceFabricMesh/applications (Mesh applications)-->
    <!--NOT AVAILBLE ON - microsoft.servicefabricmesh/gateways-->
    <!--NOT AVAILBLE ON - microsoft.servicefabricmesh/networks-->
    <!--NOT AVAILBLE ON - microsoft.servicefabricmesh/secrets-->
    <!--NOT AVAILBLE ON - microsoft.servicefabricmesh/volumes-->
    <!--NOT AVAILBLE ON - Microsoft.ServicesHub/connectors (Services Hub Connectors)-->
    
- Microsoft.SignalRService/SignalR (SignalR)

    <!--NOT AVAILBLE ON - microsoft.singularity/accounts-->
    
- microsoft.solutions/appliancedefinitions
- microsoft.solutions/appliances
- Microsoft.Solutions/applicationDefinitions（服务目录托管应用程序定义）
- Microsoft.Solutions/applications（托管应用程序）
- microsoft.solutions/jitrequests

    <!--NOT AVAILBLE ON - microsoft.spoolservice/spools-->
    
- Microsoft.Sql/instancePools（实例池）
- Microsoft.Sql/managedInstances（SQL 托管实例）
- Microsoft.Sql/managedInstances/databases（托管数据库）
- Microsoft.Sql/servers（SQL 服务器）
- Microsoft.Sql/servers/databases（SQL 数据库）
- Microsoft.Sql/servers/elasticpools（SQL 弹性池）
- microsoft.sql/servers/jobaccounts
- Microsoft.Sql/servers/jobAgents（弹性作业代理）
- Microsoft.Sql/virtualClusters（虚拟群集）
- microsoft.sqlvirtualmachine/sqlvirtualmachinegroups
- Microsoft.SqlVirtualMachine/SqlVirtualMachines（SQL 虚拟机）

    <!--NOT AVAILBLE ON - microsoft.sqlvm/dwvm-->
    
- Microsoft.Storage/StorageAccounts（存储帐户）

    <!--NOT AVAILBLE ON - Microsoft.StorageCache/caches (HPC caches)-->
    <!--NOT AVAILBLE ON - microsoft.storagepool/diskpools-->
    <!--NOT AVAILBLE ON - Microsoft.StorageSync/storageSyncServices (Storage Sync Services)-->
    <!--NOT AVAILBLE ON - Microsoft.StorageSyncDev/storageSyncServices (Storage Sync Services)-->
    <!--NOT AVAILBLE ON - Microsoft.StorageSyncInt/storageSyncServices (Storage Sync Services)-->
    <!--NOT AVAILBLE ON - Microsoft.StorSimple/Managers (StorSimple Device Managers)-->
    
- Microsoft.StreamAnalytics/clusters（流分析群集）
- Microsoft.StreamAnalytics/StreamingJobs（流分析作业）

    <!--NOT AVAILBLE ON - microsoft.swiftlet/virtualmachines-->
    <!--NOT AVAILBLE ON - microsoft.swiftlet/virtualmachinesnapshots-->
    
- Microsoft.Synapse/privateLinkHubs（Azure Synapse Analytics [专用链接中心]）
- Microsoft.Synapse/workspaces (Azure Synapse Analytics)
- Microsoft.Synapse/workspaces/bigDataPools（Apache Spark 池）
- microsoft.synapse/workspaces/sqldatabases
- Microsoft.Synapse/workspaces/sqlPools（专用 SQL 池）

    <!--NOT AVAILBLE ON - microsoft.terraformoss/providerregistrations-->
    
- Microsoft.TimeSeriesInsights/environments（时序见解环境）
- Microsoft.TimeSeriesInsights/environments/eventsources（时序见解事件源）
- Microsoft.TimeSeriesInsights/environments/referenceDataSets（时序见解参考数据集）

    <!--NOT AVAILBLE ON - microsoft.token/stores-->
    <!--NOT AVAILBLE ON - microsoft.tokenvault/vaults-->
    <!--NOT AVAILBLE ON - microsoft.virtualmachineimages/imagetemplates-->
    <!--NOT AVAILBLE ON - microsoft.visualstudio/account (Azure DevOps organizations)-->
    <!--NOT AVAILBLE ON - microsoft.visualstudio/account/extension-->
    <!--NOT AVAILBLE ON - microsoft.visualstudio/account/project (DevOps Starter)-->
    <!--NOT AVAILBLE ON - microsoft.vmware/arczones-->
    <!--NOT AVAILBLE ON - microsoft.vmware/resourcepools-->
    <!--NOT AVAILBLE ON - microsoft.vmware/vcenters-->
    <!--NOT AVAILBLE ON - Microsoft.VMware/VirtualMachines (AVS virtual machines)-->
    <!--NOT AVAILBLE ON - microsoft.vmware/virtualmachinetemplates-->
    <!--NOT AVAILBLE ON - microsoft.vmware/virtualnetworks-->
    <!--NOT AVAILBLE ON - Microsoft.VMwareCloudSimple/dedicatedCloudNodes (CloudSimple Nodes)-->
    <!--NOT AVAILBLE ON - Microsoft.VMwareCloudSimple/dedicatedCloudServices (CloudSimple Services)-->
    <!--NOT AVAILBLE ON - Microsoft.VMwareCloudSimple/virtualMachines (CloudSimple Virtual Machines)-->
    <!--NOT AVAILBLE ON - microsoft.vmwareonazure/privateclouds-->
    <!--NOT AVAILBLE ON - microsoft.vmwarevirtustream/privateclouds-->
    <!--NOT AVAILBLE ON - microsoft.vsonline/accounts-->
    <!--NOT AVAILBLE ON - Microsoft.VSOnline/Plans (Visual Studio Online Plans)-->
    
- microsoft.web/apimanagementaccounts
- microsoft.web/apimanagementaccounts/apis
- microsoft.web/certificates
- Microsoft.Web/connectionGateways（本地数据网关）
- Microsoft.Web/connections（API 连接）
- Microsoft.Web/customApis（逻辑应用自定义连接器）
- Microsoft.Web/HostingEnvironments（应用服务环境）
- Microsoft.Web/KubeEnvironments（应用服务 Kubernetes 环境）
- Microsoft.Web/serverFarms（应用服务计划）
- Microsoft.Web/sites（应用服务）
- microsoft.web/sites/premieraddons
- Microsoft.Web/sites/slots（应用服务[槽]）
- Microsoft.Web/StaticSites（静态 Web 应用[预览]）

    <!--NOT AVAILBLE ON - Microsoft.WindowsESU/multipleActivationKeys (Windows Multiple Activation Keys)-->
    <!--NOT AVAILBLE ON - Microsoft.WindowsIoT/DeviceServices (Windows 10 IoT Core Services)-->
    <!--NOT AVAILBLE ON - microsoft.workloadbuilder/migrationagents-->
    <!--NOT AVAILBLE ON - microsoft.workloadbuilder/workloads-->
    <!--NOT AVAILBLE ON - MyGet.PackageManagement/services (MyGet - Hosted NuGet, NPM, Bower and Vsix)-->
    <!--NOT AVAILBLE ON - Paraleap.CloudMonix/services (CloudMonix)-->
    <!--NOT AVAILBLE ON - Pokitdok.Platform/services (PokitDok Platform)-->
    <!--NOT AVAILBLE ON - Providers.Test/statefulIbizaEngines (Application assessments)-->
    <!--NOT AVAILBLE ON - providers.test/statefulresources-->
    <!--NOT AVAILBLE ON - providers.test/statefulresources/nestedresources-->
    <!--NOT AVAILBLE ON - providers.test/statelessresources-->
    <!--NOT AVAILBLE ON - RavenHq.Db/databases (RavenHQ)-->
    <!--NOT AVAILBLE ON - Raygun.CrashReporting/apps (Raygun)-->
    <!--NOT AVAILBLE ON - Sendgrid.Email/accounts (SendGrid Accounts)-->
    <!--NOT AVAILBLE ON - Sparkpost.Basic/services (SparkPost)-->
    <!--NOT AVAILBLE ON - stackify.retrace/services (Stackify)-->
    <!--NOT AVAILBLE ON - test.shoebox/testresources-->
    <!--NOT AVAILBLE ON - test.shoebox/testresources2-->
    <!--NOT AVAILBLE ON - TrendMicro.DeepSecurity/accounts (Deep Security SaaS)-->
    <!--NOT AVAILBLE ON - U2uconsult.TheIdentityHub/services (The Identity Hub)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/fusionGroups (LiveData Planes)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/fusionGroups/azureZones (Azure Zones)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/fusionGroups/azureZones/plugins (Plugins)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/fusionGroups/hiveReplicationRules (Hive Replication Rules)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/fusionGroups/managedOnPremZones (On-premises Zones)-->
    <!--NOT AVAILBLE ON - wandisco.fusion/fusiongroups/onpremzones-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/fusionGroups/replicationRules (Replication Rules)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/migrators (LiveData Migrators)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/migrators/liveDataMigrations (Migrations)-->
    <!--NOT AVAILBLE ON - Wandisco.Fusion/migrators/targets (Targets)-->

## <a name="securityresources"></a>securityresources

- microsoft.security/assessments
- microsoft.security/assessments/subassessments
- microsoft.security/locations/alerts（安全警报[预览]）
- microsoft.security/pricings
- microsoft.security/regulatorycompliancestandards
- microsoft.security/regulatorycompliancestandards/regulatorycompliancecontrols
- microsoft.security/regulatorycompliancestandards/regulatorycompliancecontrols/regulatorycomplianceassessments
- microsoft.security/securescores
- microsoft.security/securescores/securescorecontrols

## <a name="servicehealthresources"></a>servicehealthresources

- microsoft.resourcehealth/events

## <a name="next-steps"></a>后续步骤

- 详细了解[查询语言](../concepts/query-language.md)。
- 详细了解如何[浏览资源](../concepts/explore-resources.md)。
- 查看[初级查询](../samples/starter.md)的示例。

<!--Update_Description: update meta properties, wording update, update link-->