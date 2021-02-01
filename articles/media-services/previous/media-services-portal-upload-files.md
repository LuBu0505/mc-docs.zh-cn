---
title: 在 Azure 门户中将文件上传到媒体服务帐户 | Microsoft Docs
description: 本教程详细介绍了在 Azure 门户中将文件上传到媒体服务帐户的步骤。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 04/01/2019
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: 5f75478e0d41f5f7969d814d27d5bf54adb0c66b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059859"
---
# <a name="upload-files-to-a-media-services-account-in-the-azure-portal"></a>在 Azure 门户中将文件上传到媒体服务帐户

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [门户](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 

> [!NOTE]
> 不会向媒体服务 v2 添加任何新特性或新功能。 有关通过门户最新上传的文件，请参阅[使用门户上传、编码和流式传输内容](../latest/manage-assets-quickstart.md)。<br/>另请参阅：[媒体服务 v3](../latest/index.yml)。 另请参阅[从 v2 到 v3 的迁移指南](../latest/migrate-v-2-v-3-migration-introduction.md)

在 Azure 媒体服务中，可以将数字文件上传到资产。 资产可包含视频、音频、图像、缩略图集合、文本轨道和隐藏式字幕文件（以及这些文件的相关元数据）。 上传文件完成后，相关内容即安全地存储在云中供后续处理和流式处理。

媒体服务针对文件处理设置了一个最大文件大小。 若要详细了解文件大小限制，请参阅[媒体服务配额和限制](media-services-quotas-and-limitations.md)。

若要完成本教程，需要一个 Azure 帐户。 有关详细信息，请参阅 [Azure 试用版](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。 

## <a name="upload-files"></a>上传文件
1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 Azure 媒体服务帐户。
2. 选择“设置” > “资产”。 然后选择“上传”按钮。
   
    ![上传文件](./media/media-services-portal-vod-get-started/media-services-upload.png)
   
    此时会显示“上传视频资产”窗口。
   
   > [!NOTE]
   > 媒体服务不会限制上传视频的文件大小。
 
3. 在计算机上，转到要上传的视频。 选择视频，然后选择“确定”。  
   
    上传开始。 可以在文件名下看到进度。  

上传完成后，新资产列在“资产”窗格中。 

## <a name="media-services-learning-paths"></a>媒体服务学习路径
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="next-steps"></a>后续步骤
* 了解如何[对上传的资产进行编码](media-services-portal-encode.md)。

<!--Update_Description:wording update-->

