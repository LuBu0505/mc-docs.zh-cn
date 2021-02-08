---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/24/2020
title: RESTORE（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 RESTORE 语法。
ms.openlocfilehash: 4bc4ba71140362107d9574aebd94fc158f9e9175
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060720"
---
# <a name="restore-delta-lake-on-azure-databricks"></a>RESTORE（Azure Databricks 上的 Delta Lake）

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../../release-notes/release-types.md)提供。

> [!NOTE]
>
> 适用于 Databricks Runtime 7.4 及更高版本。

将增量表还原到早期状态。 支持还原到较早的版本号或时间戳。

## <a name="syntax"></a>语法

```
RESTORE [TABLE] table_identifier[TO] <time_travel_version>
```

where

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

```sql
<time_travel_version>  =
  TIMESTAMP AS OF <timestamp_expression> |
  VERSION AS OF <version>
```

如需详细了解 ``RESTORE`` 命令，请参阅[还原 Delta 表](../../../../delta/delta-utility.md#restore-delta-table)。