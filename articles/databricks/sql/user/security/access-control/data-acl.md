---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 数据访问控制 - Azure Databricks
description: 了解如何控制对数据对象的访问。
ms.openlocfilehash: 5df39e905e454ae006226708264c1b512a0ceb8f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692192"
---
# <a name="data-access-control"></a>数据访问控制

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

本文介绍数据对象所有者可使用 Azure Databricks 数据访问控制 SQL 语句进行管理的特权。

数据对象所有者应用 SQL ``GRANT``、``DENY``、``REVOKE`` 和 ``SHOW GRANT`` 命令，管理[用户和组](../../../admin/users-groups.md)对数据对象的访问权限。

有关使用这些命令的详细信息，请参阅[数据对象特权](../../../../security/access-control/table-acls/object-privileges.md)。

有关命令参考，请参阅[安全语句](../../../language-manual/index.md#security-statements)。

## <a name="example"></a>示例

为了使用户能够完成[快速入门：运行查询并将其可视化](../../user-quickstart.md)，请指定以下特权：

```sql
REVOKE ALL PRIVILEGES ON DATABASE default FROM `user@example.com`;

GRANT USAGE ON DATABASE default TO `user@example.com`;

GRANT SELECT ON DATABASE default TO `user@example.com`;

GRANT READ_METADATA on DATABASE default TO `user@example.com`;

SHOW GRANT `user@example.com` ON DATABASE default;

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

在 Azure Databricks SQL Analytics 查询编辑器中运行这些命令时，应会看到：

> [!div class="mx-imgBorder"]
> ![显示授权](../../../../_static/images/sql/show-grant.png)