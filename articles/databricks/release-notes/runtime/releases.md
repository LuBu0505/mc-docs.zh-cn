---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/20/2021
title: Databricks Runtime 版本 - Azure Databricks
description: Databricks Runtime 版本的发行说明。
ms.openlocfilehash: 5256adbab48eef2648d053818b8bbefc6093f2c7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060026"
---
# <a name="databricks-runtime-releases"></a>Databricks 运行时版本

本文列出了所有 Databricks Runtime 版本和受支持版本的计划。 有关 Databricks Runtime 支持策略和计划的详细信息，请参阅 [Databricks Runtime 支持生命周期](databricks-runtime-ver.md#runtime-support)。

## <a name="supported-releases"></a><a id="supported-list"> </a><a id="supported-releases"> </a>支持的版本

Azure Databricks 当前支持本部分中列出的 Databricks Runtime 版本。

### <a name="supported-databricks-runtime-releases-and-support-schedule"></a><a id="supported-databricks-runtime-releases-and-support-schedule"> </a><a id="supported-releases"> </a>支持的 Databricks Runtime 版本和支持计划

下表列出了支持的 Databricks Runtime 版本的 Apache Spark 版本、发布日期和支持结束日期。

| 版本                      | 变量                                                     | Apache Spark 版本 | 发布日期     | 支持结束日期                |
|------------------------------|-------------------------------------------------------------|-----------------------|------------------|------------------------------------|
| 7.5                          |                                                             | 3.0.1                 | 2020 年 12 月 16 日     | 2021 年 6 月 16 日                       |
|                              | [Databricks Runtime 7.5](7.5.md)                            |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 7.5](7.5ml.md)     |                       |                  |                                    |
|                              | [用于基因组学的 Databricks Runtime 7.5](7.5genomics.md)       |                       |                  |                                    |
| 7.4                          |                                                             | 3.0.1                 | 2020 年 11 月 3 日     | 2021 年 5 月 3 日                       |
|                              | [Databricks Runtime 7.4](7.4.md)                            |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 7.4](7.4ml.md)     |                       |                  |                                    |
|                              | [用于基因组学的 Databricks Runtime 7.4](7.4genomics.md)       |                       |                  |                                    |
| 7.3 LTS                      |                                                             | 3.0.1                 | 2020 年 9 月 24 日     | 2022 年 9 月 24 日                       |
|                              | [Databricks Runtime 7.3 LTS](7.3.md)                        |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 7.3 LTS](7.3ml.md) |                       |                  |                                    |
|                              | [用于基因组学的 Databricks Runtime 7.3 LTS](7.3genomics.md)   |                       |                  |                                    |
| 7.2                          |                                                             | 3.0.0                 | 2020 年 8 月 11 日     | 2021 年 2 月 11 日                       |
|                              | [Databricks Runtime 7.2](7.2.md)                            |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 7.2](7.2ml.md)     |                       |                  |                                    |
|                              | [用于基因组学的 Databricks Runtime 7.2](7.2genomics.md)       |                       |                  |                                    |
| 7.1                          |                                                             | 3.0.0                 | 2020 年 7 月 21 日     | 2021 年 1 月 21 日                       |
|                              | [Databricks Runtime 7.1](7.1.md)                            |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 7.1](7.1ml.md)     |                       |                  |                                    |
|                              | [用于基因组学的 Databricks Runtime 7.1](7.1genomics.md)       |                       |                  |                                    |
| 6.4                          |                                                             | 2.4.5                 | 2020 年 2 月 26 日     | [2021 年 4 月 1 日](#extended-release)   |
|                              | [Databricks Runtime 6.4](6.4.md)                            |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 6.4](6.4ml.md)     |                       |                  |                                    |
|                              | [用于基因组学的 Databricks Runtime 6.4](6.4genomics.md)       |                       |                  |                                    |
| 5.5 LTS                      |                                                             | 2.4.3                 | 2019 年 7 月 10 日     | 2021 年 7 月 10 日                       |
|                              | [Databricks Runtime 5.5 LTS](5.5.md)                        |                       |                  |                                    |
|                              | [用于机器学习的 Databricks Runtime 5.5 LTS](5.5ml.md) |                       |                  |                                    |

> [!NOTE]
>
> 我们已将对 Databricks Runtime 6.4 的支持延长至 2021 年 4 月。

### <a name="compatibility-matrixes"></a><a id="compatibility-matrixes"> </a><a id="extended-release"> </a>兼容性矩阵

本部分列出了 Databricks Runtime 和 Databricks Runtime ML 版本及其各自的 Delta Lake API 和 MLflow 版本。

#### <a name="delta-lake-api-compatibility-matrix"></a>Delta Lake API 兼容性矩阵

| Databricks Runtime 版本                         | Delta Lake API 版本                                 |
|----------------------------------------------------|--------------------------------------------------------|
| 7.1 - 7.5                                          | [0.7.0](https://docs.delta.io/0.7.0/delta-apidoc.html) |
| 6.4                                                | [0.5.0](https://docs.delta.io/0.5.0/delta-apidoc.html) |

#### <a name="mlflow-compatibility-matrix"></a>MLflow 兼容性矩阵

| Databricks Runtime ML 版本                      | MLflow 版本                                          |
|----------------------------------------------------|---------------------------------------------------------|
| 7.6                                                | [1.13.1](https://www.mlflow.org/docs/1.13.1/index.html) |
| 7.5                                                | [1.12.1](https://www.mlflow.org/docs/1.12.1/index.html) |
| 7.4                                                | [1.11.0](https://www.mlflow.org/docs/1.11.0/index.html) |
| 7.3 LTS                                            | [1.11.0](https://www.mlflow.org/docs/1.11.0/index.html) |
| 7.2                                                | [1.9.1](https://www.mlflow.org/docs/1.9.1/index.html)   |
| 7.1                                                | [1.9.1](https://www.mlflow.org/docs/1.9.1/index.html)   |
| 6.4                                                | [1.5.0](https://www.mlflow.org/docs/1.5.0/index.html)   |
| 5.5 LTS                                            | [1.0.0](https://www.mlflow.org/docs/1.0.0/index.html)   |

### <a name="supported-databricks-light-releases"></a>支持的 Databricks Light 版本

| 版本                      | Apache Spark 版本 | 发布日期       | 支持结束日期   |
|------------------------------|----------------------|--------------------|-----------------------|
| [2.4](2.4light.md)           | 2.4.0 及更高版本               | 2019 年 2 月 27 日       | –                     |

### <a name="release-notes"></a>发行说明

* [用于基因组学的 Databricks Runtime 7.5](7.5genomics.md)
* [用于机器学习的 Databricks Runtime 7.5](7.5ml.md)
* [Databricks Runtime 7.5](7.5.md)
* [用于基因组学的 Databricks Runtime 7.4](7.4genomics.md)
* [用于机器学习的 Databricks Runtime 7.4](7.4ml.md)
* [Databricks Runtime 7.4](7.4.md)
* [用于基因组学的 Databricks Runtime 7.3 LTS](7.3genomics.md)
* [用于机器学习的 Databricks Runtime 7.3 LTS](7.3ml.md)
* [Databricks Runtime 7.3 LTS](7.3.md)
* [用于基因组学的 Databricks Runtime 7.2](7.2genomics.md)
* [用于机器学习的 Databricks Runtime 7.2](7.2ml.md)
* [Databricks Runtime 7.2](7.2.md)
* [用于基因组学的 Databricks Runtime 7.1](7.1genomics.md)
* [用于机器学习的 Databricks Runtime 7.1](7.1ml.md)
* [Databricks Runtime 7.1](7.1.md)
* [用于基因组学的 Databricks Runtime 6.4](6.4genomics.md)
* [用于机器学习的 Databricks Runtime 6.4](6.4ml.md)
* [Databricks Runtime 6.4](6.4.md)
* [用于机器学习的 Databricks Runtime 5.5 LTS](5.5ml.md)
* [Databricks Runtime 5.5 LTS](5.5.md)
* [Databricks Light 2.4](2.4light.md)

## <a name="beta-releases"></a>Beta 版本

Databricks Runtime 的预览版始终标记为 Beta 版本。 请参阅 [Databricks Runtime 预览版](../release-types.md#runtime-releases)。 本部分列出了所有当前的 Databricks Runtime Beta 版本。

* [用于机器学习的 Databricks Runtime 7.6 Beta 版](7.6ml.md)
* [Databricks Runtime 7.6 Beta 版](7.6.md)

## <a name="unsupported-releases"></a>不支持的版本

Azure Databricks 不再支持本部分中列出的 Databricks Runtime 版本。 有关 Databricks Runtime 支持策略和计划的详细信息，请参阅 [Databricks Runtime 支持生命周期](databricks-runtime-ver.md#runtime-support)。

### <a name="end-of-support-history"></a><a id="end-of-support-history"> </a><a id="unsupported-list"> </a>支持结束历史记录

| 版本（所有变体）       | Apache Spark 版本 | 发布日期     | 支持结束日期   |
|------------------------------|----------------------|------------------|-----------------------|
| 7.0                          | 3.0.0                | 2020 年 6 月 17 日     | 2021 年 1 月 14 日          |
| 6.6                          | 2.4.5                | 2020 年 5 月 26 日     | 2020 年 11 月 26 日          |
| 6.5                          | 2.4.5                | 2020 年 4 月 14 日     | 2020 年 10 月 14 日          |
| 6.3                          | 2.4.4                | 2020 年 1 月 22 日     | 2020 年 7 月 22 日          |
| 6.2                          | 2.4.4                | 2019 年 12 月 3 日     | 2020 年 6 月 3 日          |
| 6.1                          | 2.4.4                | 2019 年 10 月 16 日     | 2020 年 4 月 16 日          |
| 6.0                          | 2.4.3                | 2019 年 10 月 1 日     | 2020 年 4 月 1 日          |
| 5.4                          | 2.4                  | 2019 年 6 月 3 日     | 2019 年 12 月 3 日          |
| 5.3                          | 2.4                  | 2019 年 4 月 3 日     | 2019 年 12 月 3 日          |
| 5.2                          | 2.4                  | 2019 年 1 月 24 日     | 2019 年 9 月 30 日          |
| 5.1                          | 2.4                  | 2018 年 12 月 18 日     | 2019 年 8 月 19 日          |
| 5.0                          | 2.4                  | 2018 年 11 月 8 日     | 2019 年 7 月 8 日          |
| 4.3                          | 2.3                  | 2018 年 8 月 10 日     | 2019 年 4 月 9 日          |
| 4.2                          | 2.3                  | 2018 年 7 月 9 日     | 2019 年 3 月 5 日          |
| 4.1                          | 2.3                  | 2018 年 5 月 17 日     | 2019 年 1 月 17 日          |
| 4.0                          | 2.3                  | 2018 年 3 月 1 日     | 2018 年 11 月 1 日          |
| 3.5 LTS                      | 2.2                  | 2017 年 12 月 21 日     | 2020 年 1 月 2 日          |
| 3.4                          | 2.2                  | 2017 年 11 月 20 日     | 2018 年 7 月 30 日          |

### <a name="release-notes"></a>发行说明

* [用于基因组学的 Databricks Runtime 7.0（不受支持）](7.0genomics.md)
* [Databricks Runtime 7.0 ML（不受支持）](7.0ml.md)
* [Databricks Runtime 7.0（不受支持）](7.0.md)
* [用于基因组学的 Databricks Runtime 6.6（不受支持）](6.6genomics.md)
* [Databricks Runtime 6.6 ML（不受支持）](6.6ml.md)
* [Databricks Runtime 6.6（不受支持）](6.6.md)
* [用于基因组学的 Databricks Runtime 6.5（不受支持）](6.5genomics.md)
* [用于机器学习的 Databricks Runtime 6.5（不受支持）](6.5ml.md)
* [Databricks Runtime 6.5（不受支持）](6.5.md)
* [用于基因组学的 Databricks Runtime 6.3（不受支持）](6.3genomics.md)
* [用于机器学习的 Databricks Runtime 6.3（不受支持）](6.3ml.md)
* [Databricks Runtime 6.3（不受支持）](6.3.md)
* [用于基因组学的 Databricks Runtime 6.2（不受支持）](6.2genomics.md)
* [用于机器学习的 Databricks Runtime 6.2（不受支持）](6.2ml.md)
* [Databricks Runtime 6.2（不受支持）](6.2.md)
* [用于机器学习的 Databricks Runtime 6.1（不受支持）](6.1ml.md)
* [Databricks Runtime 6.1（不受支持）](6.1.md)
* [用于机器学习的 Databricks Runtime 6.0（不受支持）](6.0ml.md)
* [Databricks Runtime 6.0（不受支持）](6.0.md)
* [用于机器学习的 Databricks Runtime 5.4（不受支持）](5.4ml.md)
* [Databricks Runtime 5.4（不受支持）](5.4.md)
* [Databricks Runtime 5.3 ML（不受支持）](5.3ml.md)
* [Databricks Runtime 5.3（不受支持）](5.3.md)
* [Databricks Runtime 5.2（不受支持）](5.2.md)
* [Databricks Runtime 5.1（不受支持）](5.1.md)
* [Databricks Runtime 5.0（不受支持）](5.0.md)
* [Databricks Runtime 4.3（不受支持）](4.3.md)
* [Databricks Runtime 4.2（不受支持）](4.2.md)
* [Databricks Runtime 4.1（不受支持）](4.1.md)
* [Databricks Runtime 4.0（不受支持）](4.0.md)
* [Databricks Runtime 3.5 LTS（不受支持）](3.5.md)
* [Databricks Runtime 3.4（不受支持）](3.4.md)