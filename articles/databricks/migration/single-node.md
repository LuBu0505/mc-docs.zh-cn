---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 将单节点工作负载迁移到 Azure Databricks -Azure Databricks
description: 了解如何将单节点工作负载迁移到 Azure Databricks。
ms.openlocfilehash: 2a6570e629b4e9edf945e1141f0b31b138f8dcf0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058489"
---
# <a name="migrate-single-node-workloads-to-azure-databricks"></a>将单节点工作负荷迁移到 Azure Databricks

本文解答将单节点工作负载迁移到 Azure Databricks 时出现的典型问题。

我刚刚创建了 20 个节点的 Spark 群集，但 pandas 代码并没有更快运行。出什么问题了吗？

如果你使用的是任意单节点库，则在你切换为使用 Azure Databricks 时，它们本质上不会变成分布式库。 你需要使用 Apache Spark Python API（即 [PySpark](https://spark.apache.org/docs/latest/api/python/index.html)）重写代码。

或者，可以使用 [Koalas](../languages/koalas.md)，从而可以使用 pandas DataFrame API 来访问 Apache Spark DataFrames 中的数据。

我喜欢 sklearn 中的一种算法，但 Spark ML 不支持它（例如 DBSCAN）。如何能够使用此算法且仍可利用 Spark？

* 使用 [joblib-spark](https://github.com/joblib/joblib-spark)，这是 joblib 的 Apache Spark 后端，用于在 Spark 群集上分配任务。
* 使用 [pandas 用户定义的函数](../spark/latest/spark-sql/udf-python-pandas.md)。
* 对于超参数优化，请使用 [Hyperopt](../applications/machine-learning/automl-hyperparam-tuning/index.md)。

Spark ML 的部署选项有哪些？

最佳部署选项取决于应用程序的延迟要求。

* 有关批预测，请参阅[部署模型并为模型提供服务](../applications/machine-learning/model-deploy/index.md)以及[模型推理](../applications/machine-learning/model-inference/index.md)。
* 对于流式处理应用程序，请参阅 [结构化流式处理](../spark/latest/structured-streaming/index.md)。

* 对于低延迟模型推理，请考虑 [MLflow 模型服务](../applications/mlflow/model-serving.md)或基于云提供商的解决方案，如 [Azure 机器学习](https://azure.microsoft.com/en-us/services/machine-learning/)。

如何安装或更新 pandas 或其他库？

可采用多种方法安装或更新库。

* 若要为群集上的所有用户安装或更新库，请参阅[群集库](../libraries/cluster-libraries.md)。
* 若要使 Python 库或库版本仅可用于特定笔记本，请参阅[笔记本范围内的 Python 库](../libraries/notebooks-python-libraries.md)。

如何只通过驱动程序查看 DBFS 上的数据？

将 ``/dbfs/`` 添加到文件路径的开头。 请参阅[本地文件 API](../data/databricks-file-system.md#fuse)。

如何将数据导入 Azure Databricks？

* 装载。 请参阅[将对象存储装载到 DBFS](../data/databricks-file-system.md#mount-storage)。
* “数据”选项卡。请参阅[导入、读取和修改数据简介](../data/data.md)。
* ``%sh wget``

  如果 URL 中包含数据文件，则可以使用 ``%sh wget <url>/<filename>`` 将数据导入到 Spark 驱动程序节点。

  > [!NOTE]
  >
  > 单元格输出打印 ``Saving to: '<filename>'``，但文件实际上保存到 ``file:/databricks/driver/<filename>``。

  例如，如果你通过以下命令下载文件 ``https://data.cityofnewyork.us/api/views/25th-nujf/rows.csv?accessType=DOWNLOAD``：

  ```bash
  %sh wget https://data.cityofnewyork.us/api/views/25th-nujf/rows.csv?accessType=DOWNLOAD
  ```

  若要加载此数据，请运行：

  ```python
  pandas_df = pd.read_csv("file:/databricks/driver/rows.csv?accessType=DOWNLOAD", header='infer')
  ```