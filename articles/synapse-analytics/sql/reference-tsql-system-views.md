---
title: Synapse SQL 中支持的系统视图
description: Synapse SQL 中支持的系统视图文档的链接。
author: filippopovic
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: reference
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 60af87c4cc368a6f677a6ad6af0396e635761253
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766318"
---
# <a name="system-views-supported-in-synapse-sql"></a>Synapse SQL 中支持的系统视图

Synapse SQL 中支持的 T-SQL 语句的相关文档的链接。

> [!NOTE]
> Synapse 无服务器 SQL 池仅支持 SQL Server 目录视图。  

## <a name="dedicated-sql-pool-and-serverless-sql-pool-catalog-views"></a>专用 SQL 池和无服务器 SQL 池目录视图

* [sys.pdw_column_distribution_properties](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-column-distribution-properties-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_distributions](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_index_mappings](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-index-mappings-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_loader_backup_run_details](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_loader_backup_runs](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_materialized_view_column_distribution_properties](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql?view=azure-sqldw-latest&preserve-view=true)（预览版）
* [sys.pdw_materialized_view_distribution_properties](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql?view=azure-sqldw-latest&preserve-view=true)（预览版）
* [sys.pdw_materialized_view_mappings](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql?view=azure-sqldw-latest&preserve-view=true)（预览版）
* [sys.pdw_nodes_column_store_dictionaries](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_column_store_row_groups](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_column_store_segments](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_indexes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-indexes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_partitions](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-partitions-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_pdw_physical_databases](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-pdw-physical-databases-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_nodes_tables](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-nodes-tables-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_permanent_table_mappings](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-permanent-table-mappings-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_table_distribution_properties](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-table-distribution-properties-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.pdw_table_mappings](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-table-mappings-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.resource_governor_workload_groups](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.workload_management_workload_classifier_details](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest&preserve-view=true)（预览版）
* [sys.workload_management_workload_classifiers](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql?view=azure-sqldw-latest&preserve-view=true)（预览版）

## <a name="dedicated-sql-pool-dynamic-management-views-dmvs"></a>专用 SQL 池动态管理视图 (DMV)

* [sys.dm_pdw_dms_cores](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-cores-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_dms_external_work](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-external-work-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_errors](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_exec_connections](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_exec_sessions](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_hadoop_operations](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-hadoop-operations-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_lock_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-lock-waits-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_nodes](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_os_threads](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-os-threads-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_resource_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-resource-waits-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_sys_info](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sys-info-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-wait-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?view=azure-sqldw-latest&preserve-view=true)

## <a name="sql-server-dmvs-applicable-to-dedicated-sql-pool"></a>适用于专用 SQL 池的 SQL Server DMV

以下 DMV 适用于专用 SQL 池，但必须在连接到 master 数据库后才能执行。

* [sys.database_service_objectives](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database?view=azure-sqldw-latest&preserve-view=true)
* [sys.fn_helpcollations()](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-helpcollations-transact-sql?view=azure-sqldw-latest&preserve-view=true)

## <a name="sql-server-catalog-views"></a>SQL Server 目录视图

* [sys.all_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-all-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.all_objects](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-all-objects-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.all_parameters](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-all-parameters-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.all_sql_modules](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-all-sql-modules-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.all_views](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-all-views-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.assemblies](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-assemblies-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.assembly_modules](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-assembly-modules-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.assembly_types](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-assembly-types-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.certificates](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-certificates-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.check_constraints](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-check-constraints-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.computed_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-computed-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.credentials](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-credentials-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.data_spaces](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-data-spaces-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.database_credentials](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-credentials-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.database_permissions](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-permissions-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.database_principals](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.database_query_store_options](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.database_role_members](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-role-members-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.databases](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.default_constraints](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-default-constraints-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.external_data_sources](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-external-data-sources-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.external_file_formats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-external-file-formats-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.external_tables](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-external-tables-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.filegroups](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-filegroups-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.foreign_key_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-foreign-key-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.foreign_keys](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-foreign-keys-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.identity_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.index_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-index-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.indexes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-indexes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.key_constraints](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-key-constraints-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.numbered_procedures](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.objects](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-objects-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.parameters](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-parameters-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.partition_functions](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-partition-functions-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.partition_parameters](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-partition-parameters-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.partition_range_values](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-partition-range-values-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.partition_schemes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-partition-schemes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.partitions](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-partitions-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.procedures](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-procedures-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.query_context_settings](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-context-settings-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.query_store_plan](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-plan-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.query_store_query](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-query-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.query_store_query_text](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.query_store_runtime_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.query_store_runtime_stats_interval](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.schemas](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas?view=azure-sqldw-latest&preserve-view=true)
* [sys.securable_classes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-securable-classes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.sql_expression_dependencies](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.sql_modules](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sql-modules-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.stats_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.symmetric_keys](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.synonyms](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-synonyms-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.syscharsets](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-syscharsets-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.syscolumns](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-syscolumns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.sysdatabases](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-sysdatabases-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.syslanguages](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-syslanguages-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.sysobjects](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-sysobjects-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.sysreferences](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-sysreferences-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.system_columns](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-system-columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.system_objects](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-system-objects-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.system_parameters](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-system-parameters-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.system_sql_modules](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-system-sql-modules-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.system_views](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-system-views-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.systypes](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-systypes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.sysusers](https://docs.microsoft.com/sql/relational-databases/system-compatibility-views/sys-sysusers-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.tables](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-tables-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.types](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-types-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.views](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-views-transact-sql?view=azure-sqldw-latest&preserve-view=true)

## <a name="sql-server-dmvs-available-in-dedicated-sql-pool"></a>专用 SQL 池中提供的 SQL Server DMV

SQL 池公开了许多 SQL Server 动态管理视图 (DMV)。 在专用 SQL 池中查询这些视图时，它们会报告分布区上运行的 SQL 数据库的状态。

SQL 池和分析平台系统的并行数据仓库 (PDW) 使用相同的系统视图。 每个 DMV 都有名为 pdw_node_id（它是计算节点的标识符）的列。

> [!NOTE]
> 若要使用这些视图，请在名称中插入“pdw_nodes_”，如下表所示：

| 专用 SQL 池中的 DMV 名称 | SQL Server Transact-SQL 文章|
|:--- |:--- |
| sys.dm_pdw_nodes_db_column_store_row_group_physical_stats | [sys.dm_db_column_store_row_group_physical_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true)|
| sys.dm_pdw_nodes_db_column_store_row_group_operational_stats | [sys.dm_db_column_store_row_group_operational_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true)|
| sys.dm_pdw_nodes_db_file_space_usage |[sys.dm_db_file_space_usage](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_db_index_usage_stats |[sys.dm_db_index_usage_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_db_partition_stats |[sys.dm_db_partition_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_db_session_space_usage |[sys.dm_db_session_space_usage](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_db_task_space_usage |[sys.dm_db_task_space_usage](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_background_job_queue |[sys.dm_exec_background_job_queue](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_background_job_queue_stats |[sys.dm_exec_background_job_queue_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_cached_plans |[sys.dm_exec_cached_plans](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_connections |[sys.dm_exec_connections](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_procedure_stats |[sys.dm_exec_procedure_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_query_memory_grants |[sys.dm_exec_query_memory_grants](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_query_optimizer_info |[sys.dm_exec_query_optimizer_info](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_query_resource_semaphores |[sys.dm_exec_query_resource_semaphores](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-resource-semaphores-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_query_stats |[sys.dm_exec_query_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_requests |[sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_exec_sessions |[sys.dm_exec_sessions](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_io_pending_io_requests |[sys.dm_io_pending_io_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-pending-io-requests-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_io_virtual_file_stats |[sys.dm_io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_buffer_descriptors |[sys.dm_os_buffer_descriptors](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_child_instances |[sys.dm_os_child_instances](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-child-instances-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_cluster_nodes |[sys.dm_os_cluster_nodes](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_dispatcher_pools |[sys.dm_os_dispatcher_pools](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-dispatcher-pools-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_dispatchers |未提供 Transact-SQL 文档。 |
| sys.dm_pdw_nodes_os_hosts |[sys.dm_os_hosts](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_latch_stats |[sys.dm_os_latch_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_brokers |[sys.dm_os_memory_brokers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-brokers-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_cache_clock_hands |[sys.dm_os_memory_cache_clock_hands](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-clock-hands-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_cache_counters |[sys.dm_os_memory_cache_counters](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_cache_entries |[sys.dm_os_memory_cache_entries](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_cache_hash_tables |[sys.dm_os_memory_cache_hash_tables](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-hash-tables-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_clerks |[sys.dm_os_memory_clerks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_node_access_stats |未提供 Transact-SQL 文档。 |
| sys.dm_pdw_nodes_os_memory_nodes |[sys.dm_os_memory_nodes](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_objects |[sys.dm_os_memory_objects](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_memory_pools |[sys.dm_os_memory_pools](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-pools-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_nodes |[sys.dm_os_nodes](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-nodes-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_performance_counters |[sys.dm_os_performance_counters](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_process_memory |[sys.dm_os_process_memory](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-process-memory-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_schedulers |[sys.dm_os_schedulers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_spinlock_stats |未提供 Transact-SQL 文档。 |
| sys.dm_pdw_nodes_os_sys_info |[sys.dm_os_sys_info](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_sys_memory |[sys.dm_os_memory_nodes](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_tasks |[sys.dm_os_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_threads |[sys.dm_os_threads](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_virtual_address_dump |[sys.dm_os_virtual_address_dump](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-virtual-address-dump-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_wait_stats |[sys.dm_os_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_waiting_tasks |[sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_os_workers |[sys.dm_os_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_active_snapshot_database_transactions |[sys.dm_tran_active_snapshot_database_transactions](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_active_transactions |[sys.dm_tran_active_transactions](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-active-transactions-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_commit_table |[sys.dm_tran_commit_table](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/change-tracking-sys-dm-tran-commit-table?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_current_snapshot |[sys.dm_tran_current_snapshot](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-current-snapshot-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_current_transaction |[sys.dm_tran_current_transaction](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_database_transactions |[sys.dm_tran_database_transactions](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_locks |[sys.dm_tran_locks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_session_transactions |[sys.dm_tran_session_transactions](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql?view=azure-sqldw-latest&preserve-view=true) |
| sys.dm_pdw_nodes_tran_top_version_generators |[sys.dm_tran_top_version_generators](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-tran-top-version-generators-transact-sql?view=azure-sqldw-latest&preserve-view=true) |

## <a name="sql-server-2016-polybase-dmvs-available-in-dedicated-sql-pool"></a>专用 SQL 池中提供的 SQL Server 2016 PolyBase DMV

以下 DMV 适用于专用 SQL 池，但必须在连接到 master 数据库后才能执行。

* [sys.dm_exec_compute_node_errors](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_compute_node_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_compute_nodes](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_distributed_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_distributed_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-requests-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_distributed_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-sql-requests-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_dms_services](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-dms-services-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_external_operations](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-external-operations-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_exec_external_work](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-external-work-transact-sql?view=azure-sqldw-latest&preserve-view=true)

## <a name="sql-server-information_schema-views"></a>SQL Server INFORMATION_SCHEMA 视图

* [CHECK_CONSTRAINTS](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/check-constraints-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [COLUMNS](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/columns-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [PARAMETERS](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/parameters-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [ROUTINES](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/routines-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [SCHEMATA](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/schemata-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [TABLES](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/tables-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [VIEW_COLUMN_USAGE](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/view-column-usage-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [VIEW_TABLE_USAGE](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/view-table-usage-transact-sql?view=azure-sqldw-latest&preserve-view=true)
* [VIEWS](https://docs.microsoft.com/sql/relational-databases/system-information-schema-views/views-transact-sql?view=azure-sqldw-latest&preserve-view=true)

## <a name="next-steps"></a>后续步骤

有关更多参考信息，请参阅 [Synapse SQL 中的 T-SQL 语句](../sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements.md)和 [Synapse SQL 中的 T-SQL 语言元素](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md)。

