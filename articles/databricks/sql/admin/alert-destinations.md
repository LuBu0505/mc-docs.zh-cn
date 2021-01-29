---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 警报目标 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的警报目标。
ms.openlocfilehash: 7b6304eb8eb06b398e444a9dfbbae5f28dba41a3
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692144"
---
# <a name="alert-destinations"></a>警报目标

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

每当[警报](../user/alerts/index.md)触发时，它都会将相关数据的 Blob（称为警报模板）发送到其指定的警报目标。 这些目标使用此数据 Blob 发送电子邮件、Slack 消息或自定义 Webhook。

只有 Azure Databricks [管理员](../../administration-guide/users-groups/users.md)可管理警报目标。 目标可供所有用户使用。

任何警报的默认目标都是创建该警报的用户的电子邮件地址。 如果你创建了警报并希望通过电子邮件接收通知，则无需设置新的警报目标， 只需在警报设置屏幕上切换电子邮件地址旁边的开关即可。

## <a name="create-an-alert-destination"></a>创建警报目标

1. 单击边栏底部的![用户设置图标](../../_static/images/icons/user-settings-icon.png)图标，然后选择“设置”。
2. 单击“警报目标”选项卡。
3. 单击“+新建警报目标”。
4. 选择目标类型：
   * 电子邮件
     * [Slack](#slack-destination)
     * WebHook
     * Mattermost
     * ChatWork
     * [PagerDuty](#pagerduty-destination)
     * Google Hangouts Chat

   > [!div class="mx-imgBorder"]
   > ![选择警报目标](../../_static/images/sql/pick-a-destination.png)

5. 根据类型配置目标。
6. 单击 **“创建”** 。

## <a name="pagerduty-destination"></a>PagerDuty 目标

### <a name="step-1-obtain-the-pagerduty-integration-key-from-your-pagerduty-console"></a>步骤 1：从 PagerDuty 控制台获取 PagerDuty 集成密钥

选择“服务”>“服务详细信息”>“集成”。

> [!div class="mx-imgBorder"]
> ![PagerDuty 密钥](../../_static/images/sql/alerts/pagerduty-key-location.png)

如果还没有 API v2 集成，需要创建它。

### <a name="step-2-configure-destination"></a>步骤 2：配置目标

配置必填字段：“名称”和“集成密钥”。

> [!div class="mx-imgBorder"]
> ![PagerDuty 警报目标](../../_static/images/sql/pagerduty.png)

## <a name="slack-destination"></a>Slack 目标

### <a name="step-1-obtain-the-slack-webhook-url"></a>步骤 1：获取 Slack Webhook URL

在[传入 WebHook](https://my.slack.com/services/new/incoming-webhook/) 处创建一个 Webhook URL。

### <a name="step-2--configure-destination"></a>步骤 2：配置目标

设置名称、通道和 Slack Webhook URL。 如果 Webhook 目标是通道字段中的通道，则通道名称前应加上 ``#``（例如 ``#marketing``）。 如果目标是发送给用户的私信，请在其前面加上 ``@``（例如 ``@smartguy``）。

> [!div class="mx-imgBorder"]
> ![Slack 警报目标](../../_static/images/sql/slack-destination.png)