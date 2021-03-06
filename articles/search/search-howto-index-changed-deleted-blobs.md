---
title: 已更改的和已删除的 Blob
titleSuffix: Azure Cognitive Search
description: 在从 Azure Blob 存储导入的初始搜索索引生成后，后续索引可以只选取那些已更改或已删除的 Blob。 本文介绍详细信息。
manager: nitinme
author: MarkHeff
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 09/25/2020
ms.date: 11/27/2020
ms.openlocfilehash: 3e8c84cf28d0e8a23ab58500ba54e5b402e85cf6
ms.sourcegitcommit: b6fead1466f486289333952e6fa0c6f9c82a804a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2020
ms.locfileid: "96300813"
---
# <a name="how-to-set-up-change-and-deletion-detection-for-blobs-in-azure-cognitive-search-indexing"></a>如何在 Azure 认知搜索索引中为 Blob 设置更改和删除检测

创建初始搜索索引后，你可能需要配置后续索引器作业，以只选取自初始运行以来已创建或删除的文档。 对于源自 Azure Blob 存储的搜索内容，当你使用计划触发索引时，将自动执行更改检测。 默认情况下，服务仅为已更改的 Blob 重新编制索引，正如由 Blob 的 `LastModified` 时间戳所确定。 与搜索索引器支持的其他数据源不同，Blob 始终具有时间戳，无需手动设置更改检测策略。

虽然更改检测是指定的，但删除检测不是。 如果要检测已删除的文档，请确保使用“软删除”方法。 如果彻底删除 Blob，相应的文档不会从搜索索引中删除。

可通过两种方法实现软删除方法。 下面介绍了这两种方法。

## <a name="native-blob-soft-delete-preview"></a>本机 Blob 软删除（预览版）

> [!IMPORTANT]
> 对本机 Blob 软删除的支持目前为预览版。 提供的预览版功能不附带服务级别协议，我们不建议将其用于生产工作负荷。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。 [REST API 版本 2020-06-30-Preview](./search-api-preview.md) 提供此功能。 目前不支持门户或 .NET SDK。

> [!NOTE]
> 使用本机 Blob 软删除策略时，索引中文档的文档键必须是 Blob 属性或 Blob 元数据。

在此方法中，你将使用 Azure Blob 存储提供的[本机 Blob 软删除](../storage/blobs/soft-delete-blob-overview.md)功能。 如果在存储帐户中启用了本机 Blob 软删除，你的数据源已设置了本地软删除策略，并且索引器找到了一个已转变为软删除状态的 Blob，则索引器会从索引中删除该文档。 为 Azure Data Lake Storage Gen2 中的 Blob 编制索引时，不支持本机 Blob 软删除策略。

使用以下步骤：

1. [为 Azure Blob 存储启用本地软删除](../storage/blobs/soft-delete-blob-overview.md)。 我们建议将保留策略设置为比索引器间隔计划大得多的值。 这样，如果在运行索引器时出现问题，或者如果有大量的文档需要编制索引，可以为索引器留出大量的时间来最终处理已软删除的 Blob。 仅当 Azure 认知搜索索引器在处理处于“已软删除”状态的 Blob 时，才会从索引中删除文档。

1. 在数据源中配置本机 Blob 软删除检测策略。 下面显示了一个示例。 由于此功能目前为预览版，因此必须使用预览版 REST API。

1. 运行索引器，或者将索引器设置为按计划运行。 当索引器运行并处理 Blob 时，将从索引中删除文档。

    ```http
    PUT https://[service name].search.azure.cn/datasources/blob-datasource?api-version=2020-06-30-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

### <a name="reindexing-un-deleted-blobs-using-native-soft-delete-policies"></a>对取消删除的 Blob 重新编制索引（使用本机软删除策略）

在存储帐户中启用本机软删除后，如果从 Azure Blob 存储中删除某个 Blob，该 Blob 将转变为“已软删除”状态，允许你在保留期内取消删除该 Blob。 如果在索引器处理删除后反转删除，索引器不会始终为已还原的 Blob 编制索引。 这是因为，索引器根据 Blob 的 `LastModified` 时间戳确定要为哪些 Blob 编制索引。 取消删除某个已软删除的 Blob 时，该 Blob 的 `LastModified` 时间戳不会更新，因此，如果索引器已处理的 Blob 的 `LastModified` 时间戳更接近当前时间，则索引器不会为取消删除的 Blob 重新编制索引。 

若要确保为取消删除的 Blob 重新编制索引，需要更新该 Blob 的 `LastModified` 时间戳。 为此，可以重新保存该 Blob 的元数据。 你无需更改元数据，但重新保存元数据会更新 Blob 的 `LastModified` 时间戳，使索引器知道它需要为此 Blob 重新编制索引。

## <a name="soft-delete-using-custom-metadata"></a>使用自定义元数据的软删除

在此方法中，你将使用 Blob 的元数据来指示何时应从搜索索引中删除文档。 此方法需要两个单独的操作：从索引中删除搜索文档，然后在 Azure 存储中删除 Blob。

使用以下步骤：

1. 将一个自定义元数据键值对属性添加到 Blob，以告知 Azure 认知搜索该 Blob 已采用逻辑方式删除。

1. 在数据源中配置软删除列检测策略。 下面显示了一个示例。

1. 在索引器处理 Blob 并从索引中删除文档后，你可以删除 Azure Blob 存储中的 Blob。

例如，如果某个 Blob 具有值为 `true` 的元数据属性 `IsDeleted`，以下策略会将该 Blob 视为已删除：

```http
    PUT https://[service name].search.azure.cn/datasources/blob-datasource?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }
```

### <a name="reindexing-un-deleted-blobs-using-custom-metadata"></a>对取消删除的 Blob 重新编制索引（使用自定义元数据）

索引器处理已删除的 Blob 并从索引中删除相应的搜索文档后，如果你稍后还原该 Blob 并且 Blob 的 `LastModified` 时间戳早于上次索引器运行，则它不会重新访问该 Blob。

若要为该文档重新编制索引，请更改该 Blob 的 `"softDeleteMarkerValue" : "false"`，然后重新运行索引器。

<!--
## Help us make Azure Cognitive Search better

If you have feature requests or ideas for improvements, provide your input on [UserVoice](https://feedback.azure.com/forums/263029-azure-search/). If you need help using the existing feature, post your question on [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870).
-->

## <a name="next-steps"></a>后续步骤

* [Azure 认知搜索中的索引器](search-indexer-overview.md)
* [如何配置 Blob 索引器](search-howto-indexing-azure-blob-storage.md)
* [Blob 索引概述](search-blob-storage-integration.md)