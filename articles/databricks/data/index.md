---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 数据指南 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用数据。
ms.openlocfilehash: f5a425fe651c31e22d41f22c642a075ab243a3e2
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059799"
---
# <a name="data-guide"></a><a id="data"> </a><a id="data-guide"> </a>数据指南

本指南介绍如何在 Azure Databricks 中处理数据：

* 直接从导入的数据创建表。 表架构存储在默认 Azure Databricks 内部元存储中，你也可以配置和使用外部元存储。

  * [导入、读取和修改数据简介](data.md)
  * [数据库和表](tables.md)
  * [Metastores](metastores/index.md)（元存储）
* 将数据引入 Azure Databricks

  * [合作伙伴数据集成](../integrations/ingestion/index.md)
* 访问 Apache Spark 格式的数据和外部数据源中的数据：

  * [数据源](data-sources/index.md)
  * [Delta Lake 和 Delta Engine 指南](../delta/index.md)
* 使用 Apache Spark API 处理数据。

  * [数据帧和数据集](../spark/latest/dataframes-datasets/index.md)
  * [结构化流](../spark/latest/structured-streaming/index.md)
  * [图分析](../spark/latest/graph-analysis/index.md)
* 使用旧版 Apache Spark API。

  * [Spark 流（旧版）](../spark/latest/rdd-streaming/index.md)