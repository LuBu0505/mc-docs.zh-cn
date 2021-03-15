---
title: 创建 Azure 媒体服务帐户
description: 本教程指导你完成创建 Azure 媒体服务帐户的步骤。
services: media-services
author: WenJason
manager: digimobile
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
origin.date: 11/4/2020
ms.date: 12/14/2020
ms.author: v-jay
ms.custom: devx-track-azurecli
ms.openlocfilehash: dd55259673bb990f373b45490176547e0098b9a9
ms.sourcegitcommit: 8f438bc90075645d175d6a7f43765b20287b503b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "103211820"
---
# <a name="create-a-media-services-account"></a>创建媒体服务帐户

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

若要开始加密、编码、管理和流式处理 Azure 中的媒体内容，需要创建媒体服务帐户。 媒体服务帐户需与一个或多个存储帐户相关联。 本文介绍创建新 Azure 媒体服务帐户的步骤。

 可以使用 Azure 门户或 CLI 来创建媒体服务帐户。 为要使用的方法选择选项卡。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

[!INCLUDE [create a media services account in the portal](./includes/task-create-media-services-account-portal.md)]

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

## <a name="set-the-azure-subscription"></a>设置 Azure 订阅

[!INCLUDE [Set the Azure subscription with CLI](./includes/task-set-azure-subscription-cli.md)]

## <a name="create-a-resource-group"></a>创建资源组

[!INCLUDE [Create a resource group with CLI](./includes/task-create-resource-group-cli.md)]

## <a name="create-a-storage-account"></a>创建存储帐户

[!INCLUDE [Create a storage account with CLI](./includes/task-create-storage-account-cli.md)]

## <a name="create-a-media-services-account"></a>创建媒体服务帐户

[!INCLUDE [Create a Media Services account with CLI](./includes/task-create-media-services-account-cli.md)]

---

## <a name="next-steps"></a>后续步骤

[流式传输文件](stream-files-dotnet-quickstart.md)
