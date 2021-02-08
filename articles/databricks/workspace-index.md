---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/02/2020
title: Azure Databricks 工作区指南 - Azure Databricks
description: 了解 Azure Databricks 工作区入门指南。
ms.openlocfilehash: d48c5c57649aab70729eee74779a068e3adbcb46
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060713"
---
# <a name="azure-databricks-workspace-guide"></a>Azure Databricks 工作区指南

Azure Databricks 工作区指南包括入门教程、操作方法指南和参考信息，以帮助数据工程师、数据科学家和机器学习工程师充分利用 Azure Databricks 的协作分析平台。

* [Azure Databricks 工作区概念](getting-started/concepts.md)
  * [工作区](getting-started/concepts.md#workspace)
  * [Interface](getting-started/concepts.md#interface)
  * [数据管理](getting-started/concepts.md#data-management)
  * [计算管理](getting-started/concepts.md#computation-management)
  * [模型管理](getting-started/concepts.md#model-management)
  * [身份验证和授权](getting-started/concepts.md#authentication-and-authorization)
* [获取 Databricks 培训](getting-started/training-faq.md)
* [语言路线图](languages/index.md)
  * [Python](languages/python.md)
  * [R](spark/latest/sparkr/index.md)
  * [Scala](languages/scala.md)
  * [SQL](spark/latest/spark-sql/index.md)
* [用户指南](user-guide/index.md)
  * [Databricks 运行时](runtime/index.md)
  * [工作区](workspace/index.md)
  * [群集](clusters/index.md)
  * [Notebook](notebooks/index.md)
  * [作业](jobs.md)
  * [库](libraries/index.md)
  * [Databricks 文件系统 (DBFS)](data/databricks-file-system.md)
  * [开发人员工具](dev-tools/index.md)
  * [迁移](migration/index.md)
  * [安全性和隐私性](security/index.md)
* [数据指南](data/index.md)
  * [导入、读取和修改数据简介](data/data.md)
  * [数据库和表](data/tables.md)
  * [Metastores](data/metastores/index.md)（元存储）
  * [合作伙伴数据集成](integrations/ingestion/index.md)
  * [数据源](data/data-sources/index.md)
  * [数据帧和数据集](spark/latest/dataframes-datasets/index.md)
  * [结构化流](spark/latest/structured-streaming/index.md)
  * [图分析](spark/latest/graph-analysis/index.md)
  * [Spark 流（旧版）](spark/latest/rdd-streaming/index.md)
* [Delta Lake 和 Delta Engine 指南](delta/index.md)
  * [介绍](delta/delta-intro.md)
  * [Delta Lake 快速入门](delta/quick-start.md)
  * [介绍性笔记本](delta/intro-notebooks.md)
  * [将数据引入到 Delta Lake](delta/delta-ingest.md)
  * [表批量读取和写入](delta/delta-batch.md)
  * [表流读取和写入](delta/delta-streaming.md)
  * [表删除、更新和合并](delta/delta-update.md)
  * [表实用工具命令](delta/delta-utility.md)
  * [约束](delta/delta-constraints.md)
  * [表版本控制](delta/versioning.md)
  * [API 参考](delta/delta-apidoc.md)
  * [并发控制](delta/concurrency-control.md)
  * [迁移指南](delta/porting.md)
  * [最佳做法](delta/best-practices.md)
  * [常见问题 (FAQ)](delta/delta-faq.md)
  * [资源](delta/delta-resources.md)
  * [Delta Engine](delta/optimizations/index.md)
* [机器学习和深度学习指南](applications/machine-learning/index.md)
  * [10 分钟教程：Azure Databricks 上的机器学习入门](applications/machine-learning/tutorial/index.md)
  * [加载数据](applications/machine-learning/load-data/index.md)
  * [预处理数据](applications/machine-learning/preprocess-data/index.md)
  * [训练模型](applications/machine-learning/train-model/index.md)
  * [超参数优化和 AutoML](applications/machine-learning/automl-hyperparam-tuning/index.md)
  * [跟踪模型开发](applications/machine-learning/track-model-development/index.md)
  * [模型推理](applications/machine-learning/model-inference/index.md)
  * [管理模型](applications/machine-learning/manage-model-lifecycle/index.md)
  * [部署和提供模型](applications/machine-learning/model-deploy/index.md)
  * [导出和导入模型](applications/machine-learning/model-export/index.md)
  * [参考解决方案](applications/machine-learning/reference-solutions/index.md)
* [MLflow 指南](applications/mlflow/index.md)
  * [快速入门](applications/mlflow/quick-start.md)
  * [跟踪机器学习训练运行](applications/mlflow/tracking.md)
  * [记录、加载和部署 MLflow 模型](applications/mlflow/models.md)
  * [在 Azure Databricks 上运行 MLflow 项目](applications/mlflow/projects.md)
  * [Azure Databricks 上的 MLflow 模型注册表](applications/mlflow/model-registry.md)
  * [Azure Databricks 上的 MLflow 模型服务](applications/mlflow/model-serving.md)
  * [关于在 Azure Databricks 上构建机器学习模型的端到端示例](applications/mlflow/end-to-end-example.md)
* [基因组学指南](applications/genomics/index.md)
  * [二级分析](applications/genomics/secondary/index.md)
  * [三级分析](applications/genomics/tertiary/index.md)
* [管理指南](administration-guide/index.md)
  * [管理 Azure Databricks 帐户](administration-guide/account-settings/index.md)
  * [访问管理控制台](administration-guide/admin-console.md)
  * [管理用户和组](administration-guide/users-groups/index.md)
  * [启用访问控制](administration-guide/access-control/index.md)
  * [管理工作区对象和行为](administration-guide/workspace/index.md)
  * [管理群集配置选项](administration-guide/clusters/index.md)
  * [管理虚拟网络](administration-guide/cloud-configurations/azure/index.md)
  * [灾难恢复](administration-guide/disaster-recovery.md)
* [API 参考](dev-tools/api/index.md)
  * [REST API 2.0](dev-tools/api/latest/index.md)
  * [REST API 1.2](dev-tools/api/1.2/index.md)
* [发行说明](release-notes/index.md)
  * [平台发行说明](release-notes/product/index.md)
  * [Databricks 运行时发行说明](release-notes/runtime/index.md)
  * [Databricks Connect 发行说明](release-notes/dbconnect/index.md)
  * [Azure Databricks 预览版](release-notes/release-types.md)

更多可帮助你入门的资源：

* [Apache Spark 简介](getting-started/spark/index.md)