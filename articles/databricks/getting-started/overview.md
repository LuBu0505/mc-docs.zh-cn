---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/08/2020
title: Azure Databricks 体系结构概述 - Azure Databricks
description: 获取 Azure Databricks 及其企业体系结构的简要概述。
ms.openlocfilehash: f9def2e2b605a4ad5a1dc0427a1068f33ed0210a
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060766"
---
# <a name="azure-databricks-architecture-overview"></a>Azure Databricks 体系结构概述

Databricks 统一数据分析平台由 Apache Spark 的原创人员倾力打造。数据团队可以使用该平台进行协作，以解决世界上最棘手的问题。

## <a name="high-level-architecture"></a><a id="architecture"> </a><a id="high-level-architecture"> </a>概要体系结构

Azure Databricks 设计用来实现安全的跨职能团队协作，同时将大量的后端服务留给 Azure Databricks 进行管理，因此你可以专注于数据科学、数据分析和数据工程任务。

尽管体系结构可能因自定义配置而异（例如，当你将 Azure Databricks 工作区部署到自己的虚拟网络（也称为 [VNet 注入](../administration-guide/cloud-configurations/azure/vnet-inject.md)）时就是如此），但下面的体系结构图示代表了 Azure Databricks 的最常见结构和数据流。

> [!div class="mx-imgBorder"]
> ![Databricks 体系结构](../_static/images/getting-started/databricks-architecture-azure.png)

Azure Databricks 在控制平面和数据平面上运行。

控制平面包括 Azure Databricks 在其自己的 Azure 帐户中管理的后端服务。 你运行的任何命令都将存在于控制平面中，并且你的代码会完全加密。 保存的命令驻留在数据平面中。

数据平面由你的 Azure 帐户管理，是你的数据所在的位置。 这也是对数据进行处理的位置。 此图示假设数据已引入到 Azure Databricks 中，但你可以从外部数据源引入数据，例如事件数据、流式处理数据、IoT 数据，等等。 你还可以使用 Azure Databricks 连接器连接到用于存储的 Azure 帐户外部的外部数据源。

你的数据始终驻留在 Azure 帐户的数据平面中，而不是在控制平面中，因此，你始终可以在不锁定的情况下保持对数据的完全控制和所有权。

有关体系结构的详细信息，请参阅[管理虚拟网络](../administration-guide/cloud-configurations/azure/index.md)。