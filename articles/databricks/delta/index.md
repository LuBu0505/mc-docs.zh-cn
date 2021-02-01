---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: Delta Lake 和 Delta Engine 指南 - Azure Databricks
description: 了解 Delta Lake 存储层以及随 Azure Databricks 上的 Delta Lake 提供的优化。
ms.openlocfilehash: d35b1b7c2cd0309f2273b293fdcd16a58f0f24a3
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058593"
---
# <a name="delta-lake-and-delta-engine-guide"></a>Delta Lake 和 Delta Engine 指南

[Delta Lake](https://delta.io) 是可以提高 Data Lake 可靠性的[开源存储层](https://github.com/delta-io/delta)。 Delta Lake 提供 ACID 事务和可缩放的元数据处理，并可以统一流处理和批数据处理。 Delta Lake 在现有 Data Lake 的顶层运行，与 Apache Spark API 完全兼容。 利用 Azure Databricks 上的 Delta Lake，便可以根据工作负载模式配置 Delta Lake。

Azure Databricks 还包括 [Delta Engine](optimizations/index.md)，这为快速交互式查询提供了优化的布局和索引。

本指南介绍 Azure Databricks 上的 Delta Lake 和 Delta Engine。

* [介绍](delta-intro.md)
* [Delta Lake 快速入门](quick-start.md)
* [介绍性笔记本](intro-notebooks.md)
* [将数据引入到 Delta Lake](delta-ingest.md)
* [表批量读取和写入](delta-batch.md)
* [表流读取和写入](delta-streaming.md)
* [表删除、更新和合并](delta-update.md)
* [表实用工具命令](delta-utility.md)
* [约束](delta-constraints.md)
* [表版本控制](versioning.md)
* [API 参考](delta-apidoc.md)
* [并发控制](concurrency-control.md)
* [迁移指南](porting.md)
* [最佳做法](best-practices.md)
* [常见问题 (FAQ)](delta-faq.md)
* [资源](delta-resources.md)
* [Delta Engine](optimizations/index.md)