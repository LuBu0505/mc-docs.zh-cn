---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/19/2021
title: 快速入门 - 设置用户以运行查询 - Azure Databricks
description: Azure Databricks SQL Analytics 中的管理任务快速入门。
ms.openlocfilehash: 5f4bafeec088fd1db597a7eb3d5d416ea9778e54
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692266"
---
# <a name="quickstart-set-up-a-user-to-run-a-query"></a>快速入门：设置用户以运行查询

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

本快速入门介绍了如何添加用户、创建 SQL 终结点，以及如何为用户配置数据集访问权限。 SQL Analytics [用户快速入门](../user/user-quickstart.md)需要用到此功能。

## <a name="requirements"></a>要求

* [高级计划](https://databricks.com/product/azure-pricing)上的 Azure Databricks 帐户。
* 启动工作区。 可使用现有工作区，也可创建新的工作区。 若要了解如何创建工作区，请查看[快速入门：使用 Azure 门户在 Azure Databricks 上运行 Spark 作业](/databricks/scenarios/quickstart-create-databricks-workspace-portal?tabs=azure-portal)。

## <a name="step-1-add-a-user"></a><a id="add-a-user"> </a><a id="step-1-add-a-user"> </a>步骤 1：添加用户

1. 转到[管理控制台](../../administration-guide/admin-console.md)。
2. 在“用户”选项卡上，单击“添加用户”。
3. 输入用户电子邮件地址 ID。 可以添加属于 Azure Databricks 工作区的 Azure Active Directory 租户的任何用户。

   > [!div class="mx-imgBorder"]
   > ![添加用户](../../_static/images/admin-settings/users-email-azure.png)

4. 单击“确定”。

用户已添加到工作区。

> [!div class="mx-imgBorder"]
> ![已添加用户](../../_static/images/admin-settings/add-user.png)

尽管未选中“工作区访问”和“SQL Analytics”复选框，但用户将以 ``users`` 组成员的身份继承这些权利，其中该组具有权利。 工作区管理员可从 ``users`` 组中删除权利，然后在“用户”页面上将其分别分配给用户。 若要了解 SQL Analytics 访问权利，请查看[管理用户和组](users-groups.md)。

## <a name="step-2-create-and-start-a-sql-endpoint"></a>步骤 2：创建并启动 SQL 终结点

1. 在边栏底部选择 ![应用切换器图标](../../_static/images/icons/app-switcher-icon.png) **>** ![应用切换器 - SQL Analytics](../../_static/images/icons/app-sql.png)
2. 单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。
3. 单击“+新建 SQL 终结点”。

   > [!div class="mx-imgBorder"]
   > ![创建终结点](../../_static/images/sql/create-endpoint-azure.png)

4. 在“名称”字段中，输入 ``QS Endpoint``。
5. 将“自动停止”设置切换为“开” 。
6. 选择“更多选项”。
7. 在“权限”选项卡中，单击 ![向下箭头图标](../../_static/images/sql/down-arrow-icon.png) 图标。 选择“所有用户”主体和“可使用”权限 。
8. 单击 **“添加”** 。

   > [!div class="mx-imgBorder"]
   > ![快速入门终结点](../../_static/images/sql/qs-endpoint-azure.png)

9. 单击 **“创建”** 。
10. 在终结点列表中，在筛选器框中键入 ``QS``。

    > [!div class="mx-imgBorder"]
    > ![端点](../../_static/images/sql/endpoints.png)

    QS 终结点应显示有状态 ![正在启动](../../_static/images/sql/endpoint-starting.png) Starting。

    请稍候，直到状态为 ![运行](../../_static/images/sql/endpoint-running.png) 正在运行。

## <a name="step-3-configure-access-to-the-default-database"></a>步骤 3：配置 ``default`` 数据库的访问权限

1. 单击 ![边栏中的](../../_static/images/icons/queries-icon.png) “模型”图标。
2. 单击“+新建查询”。 此时会显示查询编辑器。
3. 选择“QS 终结点”终结点。
4. 启用在[步骤 1](#add-a-user) 中创建的用户，以访问[用户快速入门](../user/user-quickstart.md)中使用的 ``default`` 数据库。 逐一进入以下查询：

   ```sql
   REVOKE ALL PRIVILEGES ON DATABASE default FROM `user@example.com`;

   GRANT USAGE ON DATABASE default TO `user@example.com`;

   GRANT SELECT ON DATABASE default TO `user@example.com`;

   GRANT READ_METADATA on DATABASE default TO `user@example.com`;

   SHOW GRANT `user@example.com` ON DATABASE default;
   ```

   每次查询后，按 Ctrl/Cmd + Enter 或单击“执行”按钮 。 最后一次查询后，应会显示：

   ```
   +------------------+---------------+------------+-----------+
   | principal        | ActionType    | ObjectType | ObjectKey |
   +------------------+---------------+------------+-----------+
   | user@example.com | READ_METADATA | DATABASE   | default   |
   +------------------+---------------+------------+-----------+
   | user@example.com | SELECT        | DATABASE   | default   |
   +------------------+---------------+------------+-----------+
   | user@example.com | USAGE         | DATABASE   | default   |
   +------------------+---------------+------------+-----------+
   ```