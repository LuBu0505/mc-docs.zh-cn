---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 工作区用户指南 - Azure Databricks
description: 了解如何使用 Azure Databricks 工作区平台。
ms.openlocfilehash: d7dbcbc07cb448172655e07b9d273ae2e474a1d6
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692063"
---
# <a name="workspace-user-guide"></a>工作区用户指南

本指南介绍 Azure Databricks 工作区中提供的工具，以及迁移和安全指南。

* [Databricks 运行时](../runtime/index.md)
  * [Databricks Runtime](../runtime/dbr.md)
  * [用于机器学习的 Databricks Runtime](../runtime/mlruntime.md)
  * [用于基因组学的 Databricks Runtime](../runtime/genomicsruntime.md)
  * [Databricks Light](../runtime/light.md)
* [工作区](../workspace/index.md)
  * [工作区资产](../workspace/workspace-assets.md)
  * [使用工作区对象](../workspace/workspace-objects.md)
  * [获取工作区、群集、笔记本、模型和作业标识符](../workspace/workspace-details.md)
  * [每工作区 URL](../workspace/per-workspace-urls.md)
* [群集](../clusters/index.md)
  * [创建群集](../clusters/create.md)
  * [管理群集](../clusters/clusters-manage.md)
  * [配置群集](../clusters/configure.md)
  * [任务抢占](../clusters/preemption.md)
  * [使用 Databricks 容器服务自定义容器](../clusters/custom-containers.md)
  * [群集节点初始化脚本](../clusters/init-scripts.md)
  * [支持 GPU 的群集](../clusters/gpu.md)
  * [单节点群集](../clusters/single-node.md)
  * [池](../clusters/instance-pools/index.md)
  * [Web 终端](../clusters/web-terminal.md)
* [笔记本](../notebooks/index.md)
  * [管理笔记本](../notebooks/notebooks-manage.md)
  * [使用笔记本](../notebooks/notebooks-use.md)
  * [可视化效果](../notebooks/visualizations/index.md)
  * [仪表板](../notebooks/dashboards.md)
  * [小组件](../notebooks/widgets.md)
  * [笔记本工作流](../notebooks/notebook-workflows.md)
  * [包单元](../notebooks/package-cells.md)
* [作业](../jobs.md)
  * [查看作业](../jobs.md#view-jobs)
  * [创建作业](../jobs.md#create-a-job)
  * [查看作业详细信息](../jobs.md#view-job-details)
  * [运行作业](../jobs.md#run-a-job)
  * [使用不同参数运行作业](../jobs.md#run-a-job-with-different-parameters)
  * [Notebook 作业提示](../jobs.md#notebook-job-tips)
  * [JAR 作业提示](../jobs.md#jar-job-tips)
  * [查看作业运行详细信息](../jobs.md#view-job-run-details)
  * [导出作业运行结果](../jobs.md#export-job-run-results)
  * [编辑作业](../jobs.md#edit-a-job)
  * [删除作业](../jobs.md#delete-a-job)
  * [库依赖项](../jobs.md#library-dependencies)
  * [高级作业选项](../jobs.md#advanced-job-options)
  * [控制对作业的访问](../jobs.md#control-access-to-jobs)
* [Libraries](../libraries/index.md)
  * [工作区库](../libraries/workspace-libraries.md)
  * [群集库](../libraries/cluster-libraries.md)
  * [笔记本范围内的 Python 库](../libraries/notebooks-python-libraries.md)
* [Databricks 文件系统 (DBFS)](../data/databricks-file-system.md)
  * [DBFS 根](../data/databricks-file-system.md#dbfs-root)
  * [使用 UI 浏览 DBFS](../data/databricks-file-system.md#browse-dbfs-using-the-ui)
  * [将对象存储装载到 DBFS](../data/databricks-file-system.md#mount-object-storage-to-dbfs)
  * [访问 DBFS](../data/databricks-file-system.md#access-dbfs)
* [开发人员工具](../dev-tools/index.md)
  * [Databricks 实用程序](../dev-tools/databricks-utils.md)
  * [Databricks CLI](../dev-tools/cli/index.md)
  * [Databricks Connect](../dev-tools/databricks-connect.md)
  * [持续集成和交付](../dev-tools/ci-cd/index.md)
  * [管理数据管道中的依赖项](../dev-tools/data-pipelines.md)
* [迁移](../migration/index.md)
  * [将生产工作负荷迁移到 Azure Databricks](../migration/production.md)
  * [将单节点工作负荷迁移到 Azure Databricks](../migration/single-node.md)
* [安全性和隐私性](../security/index.md)
  * [Azure Databricks 的企业安全性](../security/security-overview-azure.md)
  * [访问控制](../security/access-control/index.md)
  * [凭据直通身份验证](../security/credential-passthrough/index.md)
  * [机密管理](../security/secrets/index.md)
  * [为笔记本启用客户管理的密钥](../security/keys/customer-managed-key-notebook.md)
  * [为 DBFS 根配置客户管理的密钥](../security/keys/customer-managed-keys-dbfs/index.md)
  * [安全群集连接](../security/secure-cluster-connectivity.md)
  * [加密群集工作器节点之间的流量](../security/encryption/encrypt-otw.md)
  * [IP 访问列表](../security/network/ip-access-list.md)
  * [配置域名防火墙规则](../security/network/firewall-rules.md)
  * [最佳做法：使用 Delta Lake 实现 GDPR 和 CCPA 符合性](../security/privacy/gdpr-delta.md)
  * [最佳做法：Azure Databricks 上的数据管理](../security/data-governance.md)