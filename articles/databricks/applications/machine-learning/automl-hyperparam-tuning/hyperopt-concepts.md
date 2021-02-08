---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/03/2020
title: Hyperopt 概念 - Azure Databricks
description: 了解 Databricks 中的 Hyperopt 概念。
ms.openlocfilehash: 5ab24c776dccfebe7af58d12f2dbd7d73b3d7094
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061002"
---
# <a name="hyperopt-concepts"></a>Hyperopt 概念

本文介绍使用分布式 Hyperopt 所需了解的一些概念。

## <a name="in-this-section"></a>本部分内容：

* [``fmin()``](#fmin)
* [``SparkTrials`` 类](#the-sparktrials-class)
* [``SparkTrials`` 和 MLflow](#sparktrials-and-mlflow)

若要查看示例了解如何在 Azure Databricks 中使用 Hyperopt，请参阅[使用 Hyperopt 进行超参数优化](index.md#hyperopt-overview)。

## ``fmin()``

使用 ``fmin()`` 执行 Hyperopt 运行。 表中显示了 ``fmin()`` 的参数；有关详细信息，请参阅 [Hyperopt 文档](https://github.com/hyperopt/hyperopt/wiki/FMin)。 若要查看示例了解如何使用每个参数，请参阅[示例笔记本](index.md#hyperopt-overview)。

| 参数名称 | 说明                                                                                                                                                                                                                                                                                                                                                                   |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fn            | 目标函数。 Hyperopt 使用根据 space 参数中提供的超参数空间生成的值来调用此函数。 此函数可以标量值或字典的形式返回损失（有关详细信息，请参阅 [Hyperopt 文档](https://github.com/hyperopt/hyperopt/wiki/FMin)）。 此函数通常包含用于训练模型和计算损失的代码。 |
| space         | 定义要搜索的超参数空间。 借助 Hyperopt，可在如何定义此空间方面获得极大的灵活性。 你可选择分类选项（如算法），也可选择数值的概率分布（如均匀和对数）。                                                                                                                                |
| algo          | 用于搜索超参数空间的 Hyperopt 搜索算法。 最常用的是随机搜索的 ``hyperopt.rand.suggest`` 和 TPE 的 ``hyperopt.tpe.suggest``。                                                                                                                                                                                                     |
| max_evals     | 要尝试的超参数设置的数量（也就是要匹配的模型数量）。                                                                                                                                                                                                                                                                                                       |
| max_queue_len | Hyperopt 应提前生成的超参数设置数目。 由于 Hyperopt TPE 生成算法可能需要一些时间，因此将时间调高到默认值 1 以上可能会有所帮助，但通常不超过 ``SparkTrials`` 设置 ``parallelism``。                                                                                       |
| trials        | ``Trials`` 或 ``SparkTrials`` 对象。 在目标函数中调用单机算法（如 scikit-learn 方法）时，请使用 ``SparkTrials``。 在目标函数中调用分布式训练算法（如 MLlib 方法或 Horovod）时，请使用 ``Trials``。                                                                                          |

## <a name="the-sparktrials-class"></a>``SparkTrials`` 类

``SparkTrials`` 是 Databricks 开发的 API，可用于在不对 Hyperopt 代码进行其他更改的情况下分发 Hyperopt 运行。 ``SparkTrials`` 通过向 Spark 辅助角色分配试验，可加速单机优化。

> [!NOTE]
>
> ``SparkTrials`` 旨在并行化处理单机 ML 模型（如 scikit-learn）的计算。 对于使用分布式 ML 算法（如 MLlib 或 Horovod）创建的模型，请勿使用 ``SparkTrials``。 在这种情况下，模型构建过程会在群集上自动并行化，你应使用默认的 Hyperopt 类 ``Trials``。

本部分描述如何配置传递给 ``SparkTrials`` 的参数和 ``SparkTrials`` 的实现方面。

### <a name="arguments"></a>自变量

``SparkTrials`` 采用两个可选参数：

* ``parallelism``：要同时评估的试验的最大数量。 使用的数字越大，可进行横向扩展测试的超参数设置越多。 Hyperopt 会基于过去的结果提议新试验，因此需在并行度和适应度之间进行权衡。 对于固定的 ``max_evals``，并行度越大，计算速度越快；但并行度更小时，由于每个迭代有权访问更多过去的结果，因此可能获得更好的结果。

  默认值：可用的 Spark 执行程序数目。 最大值：128。 如果该值大于群集配置允许的并发任务数，则 ``SparkTrials`` 会将并行度减少到等于此值。

* ``timeout``：``fmin()`` 调用可使用的最大秒数。 超过此数目后，所有运行都将终止，且 ``fmin()`` 将退出。 系统将保存已完成的运行的相关信息。

### <a name="implementation"></a>实现

当定义传递给 ``fmin()`` 的目标函数 ``fn`` 时，以及在选择群集设置时，了解 ``SparkTrials`` 如何分配优化任务是很有帮助的。

在 Hyperopt 中，一次试验通常相当于在一组超参数上拟合一个模型。 Hyperopt 以迭代方式生成试用，评估它们，并重复执行。

使用 ``SparkTrials``，群集的驱动程序节点生成新的试用，工作器节点评估这些试用。 每个试用都是由具有一个任务的 Spark 作业生成的，并在辅助角色计算机上的任务中进行评估。 如果群集设置为每个辅助角色运行多个任务，则可以在该辅助角色上同时评估多个试用。

## <a name="sparktrials-and-mlflow"></a>``SparkTrials`` 和 MLflow

Databricks Runtime 版本决定了对 [MLflow](../../mlflow/index.md) 的支持级别：

* Databricks Runtime 5.5 ML LTS

  支持自动化 MLflow 跟踪，用于在 Python 中使用 Hyperopt 和 ``SparkTrials`` 实现超参数优化。 启用自动化 MLflow 跟踪并使用 ``SparkTrials`` 运行 ``fmin()`` 时，系统会自动在 MLflow 中记录超参数和评估指标。 如果没有自动化 MLflow 跟踪，需要进行显式 API 调用来记录 MLflow。 自动化 MLflow 跟踪功能是默认启用的。 若要禁用此设置，请将 Spark 配置 ``spark.databricks.mlflow.trackHyperopt.enabled`` 设置为 false。 即使没有自动化 MLflow 跟踪，仍然可以使用 ``SparkTrials`` 来分发优化。

* Databricks Runtime 6.3 ML 及更高版本支持从辅助角色记录到 MLflow。 可以在传递给 Hyperopt 的目标函数中添加自定义日志代码。

``SparkTrials`` 将优化结果记录为嵌套的 MLflow 运行，如下所示：

* 主运行或父运行：对 ``fmin()`` 的调用被记录为“主”运行。 如果有一个活动的运行，``SparkTrials`` 将在此活动运行下记录，并且在 ``fmin()`` 返回时不会结束运行。 如果没有活动的运行，``SparkTrials`` 将创建一个新的运行，在其中进行记录，并在 ``fmin()`` 返回之前结束该运行。
* 子运行：经过测试的每个超参数设置（“试验”）都记录为主运行下的子运行。 对于在 Databricks Runtime 6.0 ML 及更高版本上运行的群集，来自辅助角色的 MLflow 日志记录也存储在相应的子运行下。

调用 ``fmin()`` 时，Databricks 建议使用活动 MLflow 运行管理；也就是说，将对 ``fmin()`` 的调用包装在 ``with mlflow.start_run():`` 语句中。 这样可确保每个 ``fmin()`` 调用都记录在单独的 MLflow“主”运行中，并且可更轻松地将额外的标记、参数或指标记录到该运行中。

> [!NOTE]
>
> 在同一个活动 MLflow 运行中多次调用 ``fmin()`` 时，MLflow 会将这些调用记录到同一个“主”运行中。 为了解决记录的参数和标记所出现的名称冲突，MLflow 会在发生冲突的名称中追加一个 UUID。

在 Databricks Runtime 6.3 ML 及更高版本上从辅助角色进行日志记录时，无需在目标函数中显式管理运行。 在目标函数中调用 ``mlflow.log_param("param_from_worker", x)`` 以将参数记录到子运行中。 你可在目标函数中记录参数、指标、标记和项目。