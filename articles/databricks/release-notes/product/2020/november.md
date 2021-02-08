---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/01/2020
title: 2020 年 11 月 - Azure Databricks
description: 有关 Azure Databricks 新功能和改进功能的 2020 年 11 月发行说明。
ms.openlocfilehash: 9a18eccf09feeaa328462f2d35708f403974f1b7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060761"
---
# <a name="november-2020"></a>2020 年 11 月

这些功能和 Azure Databricks 平台的改进已于 2020 年 11 月发布。

> [!NOTE]
>
> 发布分阶段进行。 Azure Databricks 帐户可能要等到初始发布日期后的一周或更长时间才会更新。

## <a name="databricks-runtime-66-series-support-ends"></a>Databricks Runtime 6.6 系列支持结束

**2020 年 11 月 26 日**

对 Databricks Runtime 6.6、用于机器学习的 Databricks Runtime 6.6 以及用于基因组学的 Databricks Runtime 6.6 的支持已于 11 月 26 日结束。 请参阅 [Databricks 运行时支持生命周期](../../runtime/databricks-runtime-ver.md#runtime-support)。

## <a name="mlflow-model-registry-ga"></a>MLflow 模型注册表正式版

2020 年 11 月 18 日- 12 月 1 日：版本 3.33

[MLflow 模型注册表](../../../applications/mlflow/model-registry.md)现已正式发布。 自从模型注册表发布公共预览版以来，已经进行了一些改进：

* 对模型注册表对象的操作的诊断日志记录。 现在，模型注册表中的操作会捕获到诊断日志中。 有关所记录的操作和参数，请参阅[诊断日志事件](../../../administration-guide/account-settings/azure-diagnostic-logs.md#events)。

* 模型版本的注释。 你现在可以添加对模型版本的注释，以便使用模型注册表进行团队讨论，帮助管理模型生产化管道。
* 有关模型和模型版本的标记。 你可以为模型和模型版本创建标记，并使用 API 搜索它们。
* 对[已注册模型页](../../../applications/mlflow/model-registry.md#registered-models-page)的 URL 的改进。 此页面的 URL 现在保留其历史记录，因此，在此页中进行查询和查看模型时，可以使用浏览器的后退和前进按钮进行导航。 你还可以将该 URL 与同事共享，同事会看到同一视图。

## <a name="filter-experiment-runs-based-on-whether-a-registered-model-is-associated"></a>根据注册的模型是否关联来筛选试验运行

2020 年 11 月 18 日- 12 月 1 日：版本 3.33

查看试验的运行时，现在可以根据运行是否创建了模型版本来筛选运行。 有关详细信息，请参阅[筛选运行](../../../applications/mlflow/tracking.md#filter-runs)。

## <a name="partner-integrations-gallery-now-available-through-the-data-tab"></a>现可通过“数据”选项卡获取合作伙伴集成库

2020 年 11 月 18 日- 12 月 1 日：版本 3.33

“合作伙伴集成”库已从“帐户”菜单移动到“添加数据”选项卡。有关详细信息，请参阅[合作伙伴数据集成](../../../integrations/ingestion/index.md)。

## <a name="cluster-policies-now-use-_allowlist_-and-_blocklist_-as-policy-type-names"></a>群集策略现使用“允许列表”和“阻止列表”作为策略类型名称 

2020 年 11 月 18 日- 12 月 1 日：版本 3.33

群集策略现使用“允许列表”和“阻止列表”作为策略类型，替换了“白名单”和“黑名单”。 请参阅[群集策略定义](../../../administration-guide/clusters/policies.md#cluster-policy-definitions)。 请注意，这最初是作为 3.31 版功能发布的，这是不正确的。

## <a name="automatic-retries-when-the-creation-of-a-job-cluster-fails"></a>创建作业群集失败时自动重试

2020 年 11 月 18 日- 12 月 1 日：版本 3.33

当发生特定的可恢复错误时，Azure Databricks 现在会自动重试创建作业群集。 作业运行会保持 [RunLifeCycleState:挂起](../../../dev-tools/api/latest/jobs.md#runlifecyclestate)状态，直到群集成功启动。 每次尝试都有不同的 ``cluster_id`` 和名称。 成功创建群集后，运行会转变为 [RunLifeCycleState:正在运行](../../../dev-tools/api/latest/jobs.md#runlifecyclestate)状态。

## <a name="navigate-notebooks-using-the-table-of-contents"></a>使用目录导航笔记本

2020 年 11 月 18 日- 12 月 1 日：版本 3.33

你现在可以查看笔记本的目录，并使用它在笔记本中快速导航。 笔记本目录是基于 Markdown 标题自动创建的。 有关详细信息，请参阅[查看目录](../../../notebooks/notebooks-use.md#view-table-of-contents)。

## <a name="sql-analytics-public-preview"></a>SQL Analytics（公共预览版）

**2020 年 11 月 18 日**

Databricks 很高兴地推出 SQL Analytics，这是一个直观的环境，可用于运行临时查询和基于数据湖中存储的数据创建仪表板。  SQL Analytics 可助力组织运行多云 [lakehouse 体系结构](https://databricks.com/glossary/data-lakehouse)，该体系结构可为数据仓库性能提供数据湖经济性，同时提供良好的 SQL Analytics 用户体验。 SQL Analytics：

* 与当前使用的 BI 工具（例如 Tableau 和 Microsoft Power BI）集成，查询数据湖中最完整和最新的数据。
* 使用 SQL 原生接口对现有 BI 工具进行补充，该接口支持数据分析师和数据科学家直接在 Azure Databricks 中查询数据湖数据。
* 支持通过丰富的可视化效果和拖放式仪表板共享查询见解，以及自动在重要数据发生更改时发出警报。
* 使用 [SQL 终结点](../../../sql/admin/sql-endpoints.md)为数据湖引入可靠性、质量、缩放、安全性和性能，以便用户使用最新和最完整的数据来运行常规的分析工作负载。

有关详细信息，请参阅 [Azure Databricks SQL Analytics 指南](../../../sql/index.md)。 请联系 Azure Databricks 代表，以申请访问权限。

## <a name="single-node-clusters-now-support-databricks-container-services"></a>单节点群集现支持 Databricks 容器服务

2020 年 11 月 4-10 日：版本 3.32

你现在可以在单节点群集上使用 Databricks 容器服务。 有关详细信息，请参阅[单节点群集](../../../clusters/single-node.md)和[使用 Databricks 容器服务自定义容器](../../../clusters/custom-containers.md)。

## <a name="databricks-runtime-74-ga"></a>Databricks Runtime 7.4 正式版

**2020 年 11 月 3 日**

Databricks Runtime 7.4、Databricks Runtime 7.4 ML 和用于基因组学的 Databricks Runtime 7.4 现已正式发布。

有关信息，请参阅 [Databricks Runtime 7.4](../../runtime/7.4.md)、[用于机器学习的 Databricks Runtime 7.4](../../runtime/7.4ml.md) 和[用于基因组学的 Databricks Runtime 7.4](../../runtime/7.4genomics.md) 提供的完整发行说明。

## <a name="databricks-jdbc-driver-update"></a>Databricks JDBC 驱动程序更新

**2020 年 11 月 3 日**

已[发布](https://databricks.com/spark/odbc-driver-download)新版本的 Databricks JDBC 驱动程序。 新版本包含很多 bug 修复，最值得注意的是，驱动程序现在会返回通过 DML 操作修改的行的正确数目（如果 Databricks Runtime 提供该行数）。

## <a name="databricks-connect-73-beta"></a>Databricks Connect 7.3（beta 版本）

**2020 年 11 月 3 日**

[Databricks Connect](../../../dev-tools/databricks-connect.md) 7.3 现在作为 Beta 版本提供。

Databricks Connect 7.3 允许你使用 Azure Active Directory 令牌向 Azure Databricks 进行身份验证，并支持 Azure Active Directory 凭据直通。 这样就可以通过 Databricks Connect 使用对 Azure Databricks 进行身份验证所用的 Azure Active Directory 标识对 Azure Data Lake Storage Gen1 和 Azure Data Lake Storage Gen2 自动进行身份验证。

有关详细信息，请参阅 [Databricks Connect](../../../dev-tools/databricks-connect.md) 和 [Databricks Connect 发行说明](../../dbconnect/index.md)。