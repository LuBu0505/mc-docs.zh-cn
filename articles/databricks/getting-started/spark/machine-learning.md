---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/22/2020
title: 机器学习教程 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 机器学习库 (MLlib)。
ms.openlocfilehash: 6a5cdd670abfb6c6d2d29c86d9b785a540f5d0cf
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059874"
---
# <a name="machine-learning-tutorial"></a>机器学习教程

> [!NOTE]
>
> Databricks Runtime ML 是使用 Azure Databricks 开发和部署机器学习模型的综合性工具。 它包括最常用的机器学习和深度学习库，以及 [MLflow](../../applications/mlflow/index.md)（一种用于跟踪和管理端到端机器学习生命周期的机器学习平台 API）。 有关详细信息，请参阅[机器学习和深度学习指南](../../applications/machine-learning/index.md)。

Apache Spark 机器学习库 (MLlib) 使数据科学家能够专注于其数据问题和模型，而不是专注于解决围绕分布式数据的复杂性问题（例如基础结构、配置等）。
教程笔记本将引导你完成以下步骤：加载和预处理数据、使用 MLlib 算法训练模型、评估模型性能、优化模型以及进行预测。 它还说明了如何使用 MLlib 管道和 MLflow 机器学习平台。

## <a name="notebook"></a>笔记本

使用与群集上的 Databricks Runtime 版本相对应的笔记本。 如需更多机器学习示例，请参阅[机器学习和深度学习指南](../../applications/machine-learning/index.md)。

### <a name="get-started-with-mllib-notebook-databricks-runtime-70-and-above"></a>MLlib 笔记本入门（Databricks Runtime 7.0 及更高版本）

[获取笔记本](../../_static/notebooks/getting-started/get-started-with-mllib-dbr7.html)

### <a name="get-started-with-mllib-notebook-databricks-runtime-55-lts-or-6x"></a>MLlib 笔记本入门（Databricks Runtime 5.5 LTS 或 6.x）

[获取笔记本](../../_static/notebooks/getting-started/get-started-with-mllib-dbr6.html)