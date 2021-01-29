---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 警报访问控制 - Azure Databricks
description: 了解如何控制对 Azure Databricks SQL Analytics 警报的访问。
ms.openlocfilehash: c224f66b84c50e08fa83ddd94247318fd45f35f9
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692296"
---
# <a name="alert-access-control"></a>警报访问控制

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

使用警报访问控制，用户的操作能力取决于单个权限。 本文介绍各个权限以及配置警报访问控制的方式。

## <a name="in-this-section"></a>本节内容：

* [警报权限](#alert-permissions)
* [使用 UI 管理警报权限](#manage-alert-permissions-using-the-ui)
* [使用 API 管理警报权限](#manage-alert-permissions-using-the-api)

## <a name="alert-permissions"></a>警报权限

警报权限级别分为三个：“无权限”、“可运行”和“可管理”  。 该表列出了每个权限赋予用户的能力。

| 能力                    | 无权限            | 可运行                   | 可管理                |
|----------------------------|---------------------------|---------------------------|---------------------------|
| 在警报列表中查看          |                           | x                         | x                         |
| 查看警报和结果      |                           | x                         | x                         |
| 手动触发警报运行 |                           | x                         | x                         |
| 编辑警报                 |                           |                           | x                         |
| 修改权限         |                           |                           | x                         |
| 删除警报               |                           |                           | x                         |
| 订阅通知 |                           | x                         | x                         |

## <a name="manage-alert-permissions-using-the-ui"></a>使用 UI 管理警报权限

1. 单击 ![“警报”图标](../../../../_static/images/icons/alerts-icon.png) “模型”图标。
2. 单击警报。
3. 单击右上角的“共享”按钮。 这会显示“管理权限”对话框。
4. 选择用户或组，再选择一个权限。
5. 单击 **“添加”** 。
6. 关闭对话框。

## <a name="manage-alert-permissions-using-the-api"></a>使用 API 管理警报权限

要使用 API 管理警报权限，请在 ``/2.0/permissions/sql/alert/<alert-id>`` REST 终结点上调用方法。 例如，若要为用户 ``user@example.com`` 设置“可管理”权限，请运行以下命令：

```bash
curl -u 'token:<token>' https://<databricks-instance>/api/2.0/permissions/sql/alert/<alert-id> -X PATCH -d '{ "access_control_list" : [ { "user_name": user@example.com", "permission_level": "CAN_MANAGE" } ] }'
```

where

* ``<databricks-instance>`` 是 Azure Databricks 部署的[工作区 URL](../../../../workspace/workspace-details.md#workspace-url)。
* ``<personal-access-token>`` 是[个人访问令牌](../personal-access-tokens.md)。
* ``<alert-id>`` 是警报 ID。