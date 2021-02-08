---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/16/2020
title: 全局初始化脚本 API - Azure Databricks
description: 使用全局初始化脚本 API 可以添加全局群集初始化脚本。
ms.openlocfilehash: 099ea892bc1551e30401842896a72ecda25d3a87
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060683"
---
# <a name="global-init-scripts-api"></a>全局初始化脚本 API

[全局初始化脚本](../../../clusters/init-scripts.md#global-init-scripts)是工作区中每个群集的每个群集节点上在启动期间（在启动 Apache Spark 驱动程序或辅助 JVM 之前）运行的 shell 脚本。 这些脚本有助于在工作区中强制实施一致的群集配置。 请谨慎使用，因为它们可能会造成非预期影响，例如库冲突。

使用全局初始化脚本 API，Azure Databricks 管理员能够以安全且受控的方式添加全局群集初始化脚本。 若要了解如何使用 UI 添加这些脚本，请参阅[全局初始化脚本](../../../clusters/init-scripts.md#global-init-scripts)。

全局初始化脚本 API 作为 OpenAPI 3.0 规范提供，你可以下载它并在喜欢的 OpenAPI 编辑器中将它作为结构化 API 参考进行查看。

* [下载 OpenAPI 规范](../../../_static/api-refs/global-init-scripts-azure.yaml)
* [在 Redocly 中查看](https://redocly.github.io/redoc/?url=https://docs.microsoft.com/azure/databricks/_static/api-refs/global-init-scripts-azure.yaml)：此链接会立即将 OpenAPI 规范作为结构化 API 参考打开，便于用户查看。
* [在 Postman 中查看](https://learning.postman.com/)：Postman 是一个必须[下载](https://www.postman.com/downloads/)到计算机的应用。 下载完以后，可以文件或 URL 形式[导入 OpenAPI 规范](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-api-specifications)。
* [在 Swagger 编辑器中查看](https://editor.swagger.io/)：在联机 Swagger 编辑器中，转到“文件”菜单，然后单击“导入文件”以导入并查看下载的 OpenAPI 规范。

> [!IMPORTANT]
>
> 要访问 Databricks REST API，必须[进行身份验证](authentication.md)。