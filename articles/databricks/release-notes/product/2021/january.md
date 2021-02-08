---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/20/2021
title: 2021 年 1 月 - Azure Databricks
description: 新 Azure Databricks 功能和改进的 2021 年 1 月发行说明。
ms.openlocfilehash: dfd7348123d4d8de5370a25213b02205cc508207
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060760"
---
# <a name="january-2021"></a>2021 年 1 月

这些功能和 Azure Databricks 平台的改进已于 2021 年 1 月发布。

> [!NOTE]
>
> 发布分阶段进行。 Azure Databricks 帐户可能要等到初始发布日期后的一周或更长时间才会更新。
>
> 本月发布了 Azure Databricks 平台版本 3.37。 没有发布版本 3.35 或 3.36。

## <a name="start-clusters-faster-with-docker-images-preloaded-into-instance-pools"></a>通过将 Docker 映像预加载到实例池，更快地启动群集

2021 年 1 月 20-28 日：版本 3.37

在使用 API 创建实例池时，现在可以指定预加载的 Docker 映像。 使用已预加载的 Docker 映像的池群集启动速度更快，因为它们不需要等待映像下载。 请参阅[创建实例池](../../../dev-tools/api/latest/instance-pools.md#clusterinstancepoolservicecreateinstancepool)。

## <a name="notebook-find-and-replace-now-supports-changing-all-occurrences-of-a-match"></a>笔记本查找和替换现在支持更改所有匹配项

2021 年 1 月 20-28 日：版本 3.37

现在，在笔记本中使用查找和替换功能时，可以选择替换所有匹配项。 有关详细信息，请参阅[查找和替换文本](../../../notebooks/notebooks-use.md#find-and-replace-text)。

## <a name="single-node-clusters-ga"></a>单节点群集 (GA)

2021 年 1 月 20-28 日：版本 3.37

单节点群集是包含 Spark 驱动程序但不包含 Spark 工作器的群集。 相对而言，标准模式群集至少需要一个 Spark 工作器才能运行 Spark 作业。 单节点模式群集在以下情况下很有用：

* 运行需要 Spark 来加载和保存数据的单节点机器学习工作负荷
* 轻型探索性数据分析 (EDA)

有关详细信息，请参阅[单节点群集](../../../clusters/single-node.md)。

## <a name="free-form-cluster-policy-type-renamed-to-unrestricted"></a>自由格式的群集策略类型已重命名为“不受限制”

2021 年 1 月 20-28 日：版本 3.37

自由格式群集策略类型已重命名为“不受限制”。 有关详细信息，请参阅[管理群集策略](../../../administration-guide/clusters/policies.md)和[群集策略](../../../clusters/configure.md#cluster-policy)。

## <a name="cluster-policy-field-not-shown-if-a-user-only-has-access-to-one-policy"></a>如果用户仅有权访问一个策略，则不会显示群集策略字段

2021 年 1 月 20-28 日：版本 3.37

在创建标准群集或作业群集时，如果你只能访问一个策略，或者，如果尚未定义任何策略，则“群集策略”字段不会出现。

## <a name="databricks-runtime-70-series-support-ends"></a>Databricks Runtime 7.0 系列支持结束

2021 年 1 月 14 日

对 Databricks Runtime 7.0、适用于机器学习的 Databricks Runtime 7.0 以及适用于基因组学的 Databricks Runtime 7.0 的支持已于 1 月 14 日结束。 请参阅 [Databricks 运行时支持生命周期](../../runtime/databricks-runtime-ver.md#runtime-support)。