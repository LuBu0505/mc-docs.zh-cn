---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/02/2020
title: horovod.spark - 使用 Horovod 进行分布式深度学习 - Azure Databricks
description: 了解如何使用 horovod.spark 包对机器学习模型执行分布式训练。
ms.openlocfilehash: 1bb112f79355b9604d93fe8d80a4a2a5135297a8
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061200"
---
# <a name="horovodspark-distributed-deep-learning-with-horovod"></a><a id="horovodspark"> </a><a id="horovodspark-distributed-deep-learning-with-horovod"> </a>``horovod.spark``：使用 Horovod 进行分布式深度学习

Azure Databricks 支持 ``horovod.spark`` 包，该包提供了一个可在 Keras 和 PyTorch 的 ML 管道中使用的估算器 API。 有关详细信息，请参阅 [Spark 上的 Horovod](https://github.com/horovod/horovod/blob/master/docs/spark.rst)，其中一节介绍了 [Databricks 上的 Horovod](https://github.com/horovod/horovod/blob/master/docs/spark.rst#horovod-on-databricks)。

> [!NOTE]
>
> * Azure Databricks 会安装具有依赖项的 ``horovod`` 包。 如果升级或降级这些依赖项，则可能会出现兼容性问题。
> * 在 Keras 中将 ``horovod.spark`` 与自定义回调一起使用时，必须以 TensorFlow SavedModel 格式保存模型。
>   * 对于 TensorFlow 2.x，请在文件名中使用 ``.tf`` 后缀。
>   * 对于 TensorFlow 1.x，请设置选项 ``save_weights_only=True``。

## <a name="requirements"></a>要求

Databricks Runtime ML 7.4 或更高版本。

## <a name="examples"></a>示例

这些笔记本演示如何将 Horovod Spark 估算器 API 用于 Keras 和 PyTorch。

### <a name="horovod-spark-estimator-keras-notebook"></a>Horovod Spark 估算器 Keras 笔记本

[获取笔记本](../../../../_static/notebooks/deep-learning/horovod-spark-estimator-keras.html)

### <a name="horovod-spark-estimator-pytorch-notebook"></a>Horovod Spark 估算器 PyTorch 笔记本

[获取笔记本](../../../../_static/notebooks/deep-learning/horovod-spark-estimator-pytorch.html)