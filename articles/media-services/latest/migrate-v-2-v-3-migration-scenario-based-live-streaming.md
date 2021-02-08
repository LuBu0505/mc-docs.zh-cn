---
title: 基于媒体服务 v2 到 v3 迁移场景的实时传送视频流指南 | Microsoft Docs
description: 本文提供基于实时传送视频流场景的指南，可帮助你从 Azure 媒体服务 v2 迁移到 v3。
services: media-services
author: WenJason
manager: digimobile
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
origin.date: 1/14/2020
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: fa0398920a984bcc7356a53873eb0f101322c06b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060974"
---
# <a name="live-streaming-scenario-based-migration-guidance"></a>基于实时传送视频流场景的迁移指南

![迁移指南徽标](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![迁移步骤 2](./media/migration-guide/steps-4.svg)

Azure 门户现在支持直播活动设置和管理。  欢迎在测试 V2 到 V3 的迁移时试用。

## <a name="test-the-v3-live-event-workflow"></a>测试 V3 直播活动工作流

> [!NOTE]
> 使用 V3 api 来管理通过 v2 创建的频道和节目（映射到 v3 中的直播活动和实时输出）。 本指南介绍当你方便停止现有 V2 通道时，如何切换到 V3 直播活动和实时输出。 目前不支持将连续运行的全天候实时频道无缝迁移到新的 V3 直播活动。 因此，需要将维护停机时间协调在受众和企业都方便的时候。

在将内容从 V2 迁移到 V3 之前，测试通过媒体服务交付直播活动的新方式。 下面是要使用并考虑迁移的 V3 功能。

- 创建新的 v3 [直播活动](live-events-outputs-concept.md#live-events)进行编码。 可以启用 [1080P 和 720P 编码预设](live-event-types-comparison.md#system-presets)。
- 使用[实时输出](live-events-outputs-concept.md#live-outputs)实体而不是节目
- 创建[创建流定位器](streaming-locators-concept.md)。
- 请考虑是否需要 [HLS 和 DASH](dynamic-packaging-overview.md) 实时传送视频流。
- 如果需要快速启动直播活动，请了解新的[备用模式](live-events-outputs-concept.md#standby-mode)功能。
- 如果需要较长的流式传输持续时间，请在 v3 中创建 24x7x365 直播活动。
- 使用[事件网格](monitor-events-portal-how-to.md)来监视直播活动。

有关具体步骤，请参阅以下直播活动概念、教程和操作指南。

## <a name="live-events-concepts-tutorials-and-how-to-guides"></a>直播活动概念、教程和操作指南

### <a name="concepts"></a>概念

- [使用 Azure 媒体服务 v3 实时传送视频流](live-streaming-overview.md)
- [媒体服务中的实时事件和实时输出](live-events-outputs-concept.md)
- [经过验证的本地实时传送视频流编码器](recommended-on-premises-live-encoders.md)
- [使用时移和实时输出创建点播视频](live-event-cloud-dvr.md)
- [实时事件类型比较](live-event-types-comparison.md)
- [直播活动状态和计费](live-event-states-billing.md)
- [直播活动低延迟设置](live-event-latency.md)
- [媒体服务直播活动错误代码](live-event-error-codes.md)

### <a name="tutorials-and-quickstarts"></a>教程和快速入门

- [教程：使用媒体服务进行实时流式传输](stream-live-tutorial-with-api.md)
- [使用 OBS 创建 Azure 媒体服务实时传送流](live-events-obs-quickstart.md)
- [快速入门：使用门户上传、编码和流式传输内容](manage-assets-quickstart.md)
- [快速入门：使用 Wirecast 创建 Azure 媒体服务实时传送流](live-events-wirecast-quickstart.md)

## <a name="samples"></a>示例

还可[将代码示例中的 V2 和 V3 代码进行比较](migrate-v-2-v-3-migration-samples.md)。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
