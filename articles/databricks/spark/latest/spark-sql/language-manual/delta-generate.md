---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/06/2020
title: GENERATE（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 GENERATE 语法。
ms.openlocfilehash: 87da4b22d0cf2f2c1e93b4e1504e4fa0a08fdf7f
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061158"
---
# <a name="generate-delta-lake-on-azure-databricks"></a>GENERATE（Azure Databricks 上的 Delta Lake）

在 Delta 表中生成给定模式（指定为字符串）。

## <a name="syntax"></a>语法

```
GENERATE mode FOR TABLE table_identifier
```

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

有关详细信息，请参阅[将 Parquet 表转换为 Delta 表](../../../../delta/delta-utility.md#delta-generate)。