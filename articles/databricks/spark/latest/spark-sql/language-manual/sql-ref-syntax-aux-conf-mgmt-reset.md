---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: RESET - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 RESET 语法。
ms.openlocfilehash: 7396aa08a7c8447c5226998c4d7fecb128984043
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060633"
---
# <a name="reset"></a>RESET

重置特定于当前会话的已通过 [SET](sql-ref-syntax-aux-conf-mgmt-set.md) 命令设置为默认值的运行时配置。

## <a name="syntax"></a>语法

```sql
RESET;
```

## <a name="parameters"></a>参数

* **(none)**

  重置特定于当前会话的已通过 [SET](sql-ref-syntax-aux-conf-mgmt-set.md) 命令设置为默认值的任何运行时配置。

## <a name="examples"></a>示例

```sql
-- Reset any runtime configurations specific to the current session which were set via the SET command to your default values.
RESET;
```

## <a name="related-statements"></a>相关语句

* [SET](sql-ref-syntax-aux-conf-mgmt-set.md)