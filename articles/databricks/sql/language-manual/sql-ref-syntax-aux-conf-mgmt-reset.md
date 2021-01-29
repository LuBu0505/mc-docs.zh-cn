---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: RESET - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 RESET 语法。
ms.openlocfilehash: a5350021b0e2c25dba6b7f09cd20c2aedf9ef381
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692367"
---
# <a name="reset"></a>RESET

重置特定于当前会话的运行时配置，该会话使用 [SET](sql-ref-syntax-aux-conf-mgmt-set.md) 命令设置为默认值。

## <a name="syntax"></a>语法

```sql
RESET;
```

## <a name="parameters"></a>参数

* **(none)**

  重置特定于当前会话的任何运行时配置，该会话使用 [SET](sql-ref-syntax-aux-conf-mgmt-set.md) 命令设置为默认值。

## <a name="examples"></a>示例

```sql
-- Reset any runtime configurations specific to the current session which were set using the SET command to your default values.
RESET;
```

## <a name="related-statements"></a>相关语句

* [SET](sql-ref-syntax-aux-conf-mgmt-set.md)