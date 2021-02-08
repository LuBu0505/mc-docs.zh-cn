---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/23/2020
title: API 参考 - Azure Databricks
description: 了解有关由 Delta Lake 提供的 API 的信息。
ms.openlocfilehash: 0194b57cf121064eaeb8e146be5f5c81c5309101
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060032"
---
# <a name="api-reference"></a>API 参考

对于 Delta 表的大多数读取和写入操作，可使用 Apache Spark [读取器和编写器](../spark/latest/dataframes-datasets/index.md) API。 有关示例，请参阅[表批处理读取和写入](delta-batch.md)以及[表流式处理读取和写入](delta-streaming.md)。

但是，有一些操作是特定于 Delta Lake 的，必须使用 Delta Lake API。 有关示例，请参阅[表实用工具命令](delta-utility.md)。

> [!NOTE]
>
> 某些 Delta Lake API 仍在不断发展，在 API 文档中用“Evolving”限定符来表示。

Azure Databricks 可确保 Delta Lake 项目与 Databricks Runtime 中的 Delta Lake 之间的二进制兼容性。 若要查看每个 Databricks Runtime 版本中打包的 Delta Lake API 版本以及指向 API 文档的链接，请参阅 [Delta Lake API 兼容性矩阵](../release-notes/runtime/releases.md#delta-lake-api-compatibility-matrix)。