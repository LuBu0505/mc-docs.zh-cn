---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: SQL 终结点访问控制 - Azure Databricks
description: 了解如何控制对 Azure Databricks SQL 终结点的访问。
ms.openlocfilehash: 20d95f4028a12e2afe4daebec5e0ece4eb650454
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692185"
---
# <a name="sql-endpoint-access-control"></a>SQL 终结点访问控制

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

在使用 SQL 终结点访问控制的情况下，用户的操作能力取决于单个权限。 本文介绍各个权限以及配置 SQL 终结点访问控制的方式。

## <a name="sql-endpoint-permissions"></a>SQL 终结点权限

SQL 终结点权限级别分为三个：“无权限”、“可使用”和“可管理”  。 该表列出了每个权限赋予用户的能力。

| 能力                    | 无权限            | 可以使用                   | 可管理                |
|----------------------------|---------------------------|---------------------------|---------------------------|
| 查看自己的查询           | x                         | x                         | x                         |
| 查看查询详细信息         |                           |                           | x                         |
| 查看所有用户的查询 |                           |                           | x                         |
| 查看终结点详细信息      |                           | x                         | x                         |
| 启动终结点             |                           | x                         | x                         |
| 停止终结点              |                           |                           | x                         |
| 删除终结点            |                           |                           | x                         |
| 编辑终结点              |                           |                           | x                         |
| 修改权限         |                           |                           | x                         |

## <a name="manage-sql-endpoint-permissions-using-the-ui"></a>使用 UI 管理 SQL 终结点权限

1. 单击 ![“终结点”图标](../../../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 单击一个终结点。
3. 单击 **“编辑”** 。
4. 选择“更多选项”。

   这会显示 SQL 终结点权限。 Azure Databricks 管理员具有“可管理”权限。

5. 选择用户或组，再选择一个权限。

   > [!div class="mx-imgBorder"]
   > ![添加权限](../../../../_static/images/sql/add-endpoint-permission.png)

6. 单击“添加”  。
7. 单击“ **保存**”。

## <a name="manage-sql-endpoint-permissions-using-the-api"></a>使用 API 管理 SQL 终结点权限

若要使用 API 管理 SQL 终结点权限，请在 ``/2.0/permissions/sql/endpoints/<endpoint-id>`` REST 终结点上调用方法。 例如，若要为用户 ``user@example.com`` 设置“可管理”权限，请运行以下命令：

```bash
curl -u 'token:<token>' https://<databricks-instance>/api/2.0/permissions/sql/endpoints/<endpoint-id> -X PATCH -d '{ "access_control_list" : [ { "user_name": user@example.com", "permission_level": "CAN_MANAGE" } ] }'
```

where

* ``<databricks-instance>`` 是 Azure Databricks 部署的[工作区 URL](../../../../workspace/workspace-details.md#workspace-url)。
* ``<personal-access-token>`` 是[个人访问令牌](../personal-access-tokens.md)。
* ``<endpoint-id>`` 是终结点 ID。