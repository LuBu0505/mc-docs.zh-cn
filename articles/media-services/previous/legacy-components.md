---
title: Azure 媒体服务旧组件 | Microsoft Docs
description: 本主题介绍 Azure 媒体服务旧组件。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/21/2020
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: ab2c584e105158a43254e795d0ad8990b7d26f24
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060184"
---
# <a name="azure-media-services-legacy-components"></a>Azure 媒体服务旧组件

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

随着时间的推移，我们会增强媒体服务组件并停用旧组件。 本文可帮助你将应用程序从旧组件迁移到当前组件。
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>旧组件的停用计划和迁移指南

Windows Azure 媒体编码器 (WAME) 和 Azure 媒体编码器 (AME) 媒体处理器已弃用。

* [从 Windows Azure 媒体编码器迁移到 Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [从 Azure 媒体编码器迁移到 Media Encoder Standard](migrate-azure-media-encoder.md)

以下媒体分析媒体处理器已弃用或即将弃用：

  
 
| **媒体处理器名称** | **停用日期** | **其他说明** |
| --- | --- | ---|
| Azure Media Indexer | 2023 年 3 月 1 日 | 此媒体处理器将替换为[媒体服务 v3 AudioAnalyzerPreset 基本模式](../latest/analyzing-video-audio-files-concept.md)。 |
| 动作检测 | 2020 年 6 月 1 日|目前无替换计划。 |
| 视频摘要 |2020 年 6 月 1 日|目前无替换计划。|

## <a name="next-steps"></a>后续步骤

[有关从媒体服务 v2 迁移到 v3 的指导](../latest/migrate-v-2-v-3-migration-introduction.md)
