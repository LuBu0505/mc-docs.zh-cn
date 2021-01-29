---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/07/2021
title: 仪表板访问控制 - Azure Databricks
description: 了解如何控制对 Azure Databricks SQL Analytics 仪表板的访问。
ms.openlocfilehash: 0adc3e508e385331632894ccc43d3f457edd9c79
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692195"
---
# <a name="dashboard-access-control"></a>仪表板访问控制

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

在使用仪表板访问控制的情况下，用户的操作能力取决于单个权限。 本文介绍各个权限以及配置仪表板访问控制的方式。

## <a name="dashboard-permissions"></a>仪表板权限

仪表板权限级别分为三个：“无权限”、“可运行”和“可管理”  。 该表列出了每个权限赋予用户的能力。

| 能力                                                                 | 无权限            | 可运行                   | 可管理                |
|-------------------------------------------------------------------------|---------------------------|---------------------------|---------------------------|
| 在仪表板列表中查看                                                   |                           | x                         | x                         |
| 查看仪表板和结果                                              |                           | x                         | x                         |
| 在仪表板中刷新查询结果（或选择不同的参数） |                           | x                         | x                         |
| 编辑仪表板                                                          |                           |                           | x                         |
| 修改权限                                                      |                           |                           | x                         |
| 删除仪表板                                                        |                           |                           | x                         |

> [!NOTE]
>
> 用于执行查询的主体是创建查询的用户，而不是单击“刷新”按钮的用户。

## <a name="manage-dashboard-permissions-using-the-ui"></a>使用 UI 管理仪表板权限

1. 单击 ![仪表板图标](../../../../_static/images/icons/dashboards-icon.png) “模型”图标。
2. 单击仪表板。
3. 单击右上角的“共享”按钮。 这会显示“管理权限”对话框。
4. 选择用户或组，再选择一个权限。
5. 单击 **“添加”** 。
6. 关闭对话框。

## <a name="manage-dashboard-permissions-using-the-api"></a>使用 API 管理仪表板权限

若要使用 API 管理仪表板权限，请在 ``/2.0/permissions/sql/dashboard/<dashboard-id>`` REST 终结点上调用方法。 例如，若要为用户 ``user@example.com`` 设置“可管理”权限，请运行以下命令：

```bash
curl -u 'token:<token>' https://<databricks-instance>/api/2.0/permissions/sql/dashboard/<dashboard-id> -X PATCH -d '{ "access_control_list" : [ { "user_name": user@example.com", "permission_level": "CAN_MANAGE" } ] }'
```

where

* ``<databricks-instance>`` 是 Azure Databricks 部署的[工作区 URL](../../../../workspace/workspace-details.md#workspace-url)。
* ``<personal-access-token>`` 是[个人访问令牌](../personal-access-tokens.md)。
* ``<dashboard-id>`` 是仪表板 ID。