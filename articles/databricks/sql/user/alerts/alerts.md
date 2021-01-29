---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 警报任务 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的警报任务。
ms.openlocfilehash: 9b441d28b8255be506cb2a94b452a91c002fc45d
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692173"
---
# <a name="alert-tasks"></a>警报任务

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

## <a name="create-an-alert"></a>创建警报

1. 单击 ![边栏中的](../../../_static/images/icons/alerts-icon.png) “模型”图标。
2. 单击“+新建警报”。
3. 搜索目标查询。 如果没看到所需的查询，请确保它已发布且未使用参数。

   > [!div class="mx-imgBorder"]
   > ![目标查询](../../../_static/images/sql/new-alert-query-search.png)

4. 在“触发时间”字段中配置警报。
   * “值列”下拉列表控制计算查询结果的哪个字段。
   * “条件”下拉列表控制要应用的逻辑操作。
   * 使用指定的条件将“阈值”文本输入与值列进行比较。

   > [!div class="mx-imgBorder"]
   > ![警报设置](../../../_static/images/sql/alert_settings.png)

   > [!NOTE]
   >
   > 如果目标查询返回多个记录，则 SQL Analytics 警报对第一个记录起作用。 更改值列设置时，其下方会显示顶行中该字段的当前值。

5. 在“触发时发送通知”字段中，选择触发警报时发送的通知数量：
   * **仅一次**：当[警报状态](index.md#view-alerts)从 ``OK`` 更改为 ``TRIGGERED`` 时发送通知。
   * **每次评估警报时**：每当警报状态为 ``TRIGGERED`` 时，无论在上次评估中其状态如何，都发送通知。
   * **最多每次**：每当警报状态在特定时间间隔内为 ``TRIGGERED`` 时发送通知。 此选项可让你避免经常触发的警报遇到通知垃圾邮件。

   无论选择哪种通知设置，只要状态从 ``OK`` 更改为 ``TRIGGERED`` 或从 ``TRIGGERED`` 更改为 ``OK``，你都会收到通知。 如果状态在相邻两次执行中都是 ``TRIGGERED``，则计划设置将影响你收到的通知数量。 有关详细信息，请查看[通知频率](#notification-frequency)。

6. 在“模板”下拉列表中，选择一个模板：
   * **使用默认模板**：警报通知是一条消息，其中有指向警报配置屏幕和查询屏幕的链接。
   * **使用自定义模板**：警报通知包括有关警报的更多特定信息。
     1. 会显示一个框，其中有输入主题和正文的字段。 任何静态内容都有效，你可合并内置模板变量：
        * ``ALERT_STATUS``：已评估的警报状态（字符串）。
        * ``ALERT_CONDITION``：警报条件运算符（字符串）。
        * ``ALERT_THRESHOLD``：警报阈值（字符串或数字）。
        * ``ALERT_NAME``：警报名称（字符串）。
        * ``ALERT_URL``：警报页 URL（字符串）。
        * ``QUERY_NAME``：关联的查询名称（字符串）。
        * ``QUERY_URL``：关联的查询页 URL（字符串）。
        * ``QUERY_RESULT_VALUE``：查询结果值（字符串或数字）。
        * ``QUERY_RESULT_ROWS``：查询结果行（值数组）。
        * ``QUERY_RESULT_COLS``：查询结果列（字符串数组）。

        例如，示例主题可以是：``Alert "{{ALERT_NAME}}" changed status to {{ALERT_STATUS}}``。

     1. 单击“预览”切换按钮来预览呈现的结果。

        > [!IMPORTANT]
        >
        > 若要验证模板变量是否正确呈现，预览功能很有用。 它不是最终通知内容的准确表示形式，因为每个警报目标会以不同方式显示通知。

     1. 单击“保存更改”按钮。
7. 单击“创建警报”。
8. 选择[警报目标](../../admin/alert-destinations.md)。

   > [!IMPORTANT]
   >
   > 如果跳过此步骤，那么在触发警报时，你将不会收到通知。

   > [!div class="mx-imgBorder"]
   > ![警报目标](../../../_static/images/sql/alert_destination.png)

## <a name="notification-frequency"></a>通知频率

每当 SQL Analytics 检测到警报状态已从 ``OK`` 更改为 ``TRIGGERED`` 时（反之亦然），就会将通知发送到你选择的警报目标。
请思考以下示例，其中对计划每天运行一次的查询配置了警报。 下表显示了警报的每日状态。
星期一之前，警报状态为 ``OK``。

| 日期       | 警报状态 |
|-----------|--------------|
| 星期一    | 确定           |
| 星期二   | 确定           |
| 星期三 | 已触发    |
| 星期四  | 已触发    |
| 星期五    | 已触发    |
| 星期六  | 已触发    |
| 星期日    | 确定           |

如果通知频率设置为 ``Just Once``，则当状态从 ``OK`` 更改为 ``TRIGGERED`` 时，SQL Analytics 会在周三发送通知，反之则在周日发送通知。 它不会在周四、周五或周六发送警报，除非你专门将其配置为在这几天发送，因为警报状态在这几天执行期间没有发生变化。

## <a name="configure-alert-permissions"></a>配置警报权限

若要配置可管理和运行警报的人员，请查看[警报访问控制](../security/access-control/alert-acl.md)。