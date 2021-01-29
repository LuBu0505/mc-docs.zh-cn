---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/07/2021
title: 查询访问控制 - Azure Databricks
description: 了解如何控制对 Azure Databricks SQL Analytics 查询的访问。
ms.openlocfilehash: 856023b9f0d746fa26ae2a21028629bcae42565f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692191"
---
# <a name="query-access-control"></a>查询访问控制

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

使用查询访问控制时，用户的操作能力取决于单个权限。 本文介绍各个权限以及配置查询访问控制的方式。

## <a name="query-permissions"></a>查询权限

查询权限级别分为三个：“无权限”、“可运行”和“可管理”  。 该表列出了每个权限赋予用户的能力。

| 能力                                               | 无权限            | 可运行                   | 可管理                |
|-------------------------------------------------------|---------------------------|---------------------------|---------------------------|
| 查看自己的查询                                      |                           | x                         | x                         |
| 在查询列表中查看                                     |                           | x                         | x                         |
| 查看查询文本                                       |                           | x                         | x                         |
| 查看查询结果                                     |                           | x                         | x                         |
| 刷新查询结果（或选择其他参数） |                           | x                         | x                         |
| 在仪表板中包含查询                      |                           | x                         | x                         |
| 编辑查询文本                                       |                           |                           | x                         |
| 更改数据源                                    |                           |                           | x                         |
| 修改权限                                    |                           |                           | x                         |
| 删除查询                                          |                           |                           | x                         |

> [!NOTE]
>
> 用于执行查询的主体是创建查询的用户，而不是单击“刷新”按钮的用户。

## <a name="manage-query-permissions-using-the-ui"></a>使用 UI 管理查询权限

1. 单击 ![边栏中的](../../../../_static/images/icons/queries-icon.png) “模型”图标。
2. 单击查询。
3. 单击右上角的“共享”按钮。 这会显示“管理权限”对话框。
4. 选择用户或组，再选择一个权限。
5. 单击 **“添加”** 。
6. 关闭对话框。

## <a name="manage-query-permissions-using-the-api"></a>使用 API 管理查询权限

若要使用 API 管理查询权限，请在 ``/2.0/permissions/sql/query/<query-id>`` REST 终结点上调用方法。 例如，若要为用户 ``user@example.com`` 设置“可管理”权限，请运行以下命令：

```bash
curl -u 'token:<token>' https://<databricks-instance>/api/2.0/permissions/sql/query/<query-id> -X PATCH -d '{ "access_control_list" : [ { "user_name": user@example.com", "permission_level": "CAN_MANAGE" } ] }'
```

where

* ``<databricks-instance>`` 是 Azure Databricks 部署的[工作区 URL](../../../../workspace/workspace-details.md#workspace-url)。
* ``<personal-access-token>`` 是[个人访问令牌](../personal-access-tokens.md)。
* ``<query-id>`` 是查询 ID。