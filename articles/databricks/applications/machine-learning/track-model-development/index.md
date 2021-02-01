---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/28/2020
title: 跟踪模型开发 - Azure Databricks
description: 了解在 Azure Databricks 中跟踪模型开发。
ms.openlocfilehash: 8e171b15cbbe9221edbe5987e86b6304bba84d44
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060917"
---
# <a name="track-model-development"></a>跟踪模型开发

模型开发过程是迭代的过程，当你开发和优化模型时，跟踪工作可能会很有挑战性。 在 Azure Databricks 中，可使用 [MLflow 跟踪](https://mlflow.org/docs/latest/tracking.html)来帮助你跟踪模型开发过程，包括你尝试过的参数设置或组合，以及它们如何影响模型的性能。

MLflow tracking使用试验和运行来记录和跟踪模型开发 。 一次运行就是执行一次模型代码。 在一次 MLflow 运行期间，可记录模型参数和结果。 试验是相关运行的集合。 在一个试验中，可比较并筛选运行，了解模型如何执行，以及它的性能如何取决于参数设置、输入数据等。

本文中的笔记本提供简单的示例，可帮助你快速开始使用 MLflow 来跟踪模型开发。 有关在 Azure Databricks 中使用 MLflow 跟踪的更多详细信息，请参阅[跟踪机器学习训练运行](../../mlflow/tracking.md)。

## <a name="use-autologging-to-track-model-development"></a>使用 autologging 跟踪模型开发

MLflow 可自动记录在许多 ML 框架中编写的训练代码。 这是开始使用 MLflow 跟踪的最简单方法。

此示例笔记本介绍如何结合使用 autologging 和 [scikit-learn](https://scikit-learn.org/stable/index.html)。 有关使用其他 Python 库的 autologging 的信息，请参阅[自动将训练运行记录到 MLflow](../../mlflow/quick-start-python.md#automatically-log-training-runs-to-mlflow)。

### <a name="mlflow-autologging-quick-start-python-notebook"></a>MLflow Autologging 快速入门 Python 笔记本

[获取笔记本](../../../_static/notebooks/mlflow/mlflow-quick-start-python.html)

## <a name="use-the-logging-api-to-track-model-development"></a>使用日志记录 API 跟踪模型开发

此笔记本说明了如何使用 MLflow 日志记录 API。 使用日志记录 API，你可以更好地控制记录的指标，还可以记录其他项目，例如表或绘图。

此示例笔记本介绍如何使用 [Python 日志记录 API](https://mlflow.org/docs/latest/python_api/index.html)。 MLflow 还具有 [REST、R 和 Java API](https://mlflow.org/docs/latest/tracking.html)。

### <a name="mlflow-logging-api-quick-start-python-notebook"></a>MLflow 日志记录 API 快速入门 Python 笔记本

[获取笔记本](../../../_static/notebooks/mlflow/mlflow-logging-api-quick-start-python.html)

## <a name="end-to-end-example"></a>端到端示例

本教程笔记本介绍了一个在 Azure Databricks 中训练模型的端到端示例，包括加载数据、对数据进行可视化、设置并行超参数优化，以及使用 MLflow 查看结果、注册模型和通过 Spark UDF 中的已注册模型对新数据执行推理。

### <a name="requirements"></a>要求

Databricks Runtime 6.5 ML 或更高版本。

#### <a name="mlflow-end-to-end-example-notebook"></a>MLflow 端到端示例笔记本

[获取笔记本](../../../_static/notebooks/mlflow/mlflow-end-to-end-example-azure.html)