---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SET TIME ZONE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 SET TIME ZONE 语法。
ms.openlocfilehash: 5898a1ff706f60211bacf566b6f66b8f3e63e9e4
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692366"
---
# <a name="set-time-zone"></a>SET TIME ZONE

设置当前会话的时区。

## <a name="syntax"></a>语法

```sql
SET TIME ZONE LOCAL
SET TIME ZONE 'timezone_value'
SET TIME ZONE INTERVAL interval_literal
```

## <a name="parameters"></a>参数

* **LOCAL**

  将时区设置为 java ``user.timezone`` 属性中指定的时区，如果 ``user.timezone`` 未定义，则将时区设置为环境变量 ``TZ``，如果两个时区都未定义，则设置为系统时区。

* **timezone_value**

  会话本地时区的 ID，其格式为基于区域的区域 ID 或区域偏移。 区域 ID 必须具有“区域/城市”的格式，如“America/Los_Angeles”。 区域偏移的格式必须是“``(+|-)HH``”、“``(+|-)HH:mm``”或“``(+|-)HH:mm:ss``”，如“-08”、“+01:00”或“-13:33:33”。 此外，还支持“UTC”和“Z”作为“+00:00”的别名。 不建议使用其他短名称，因为它们可能不明确。

* **interval_literal**

  [间隔字面量](sql-ref-literals.md#interval-literals)表示会话时区与“UTC”之间的差值。 它必须在 [-18, 18] 小时和最大到第二精度范围内，例如 ``INTERVAL 2 HOURS 30 MINITUES`` 或 ``INTERVAL '15:40:32' HOUR TO SECOND``。

## <a name="examples"></a>示例

```sql
-- Set time zone to the system default.
SET TIME ZONE LOCAL;

-- Set time zone to the region-based zone ID.
SET TIME ZONE 'America/Los_Angeles';

-- Set time zone to the Zone offset.
SET TIME ZONE '+08:00';

-- Set time zone with intervals.
SET TIME ZONE INTERVAL 1 HOUR 30 MINUTES;
SET TIME ZONE INTERVAL '08:30:00' HOUR TO SECOND;
```

## <a name="related-statements"></a>相关语句

* [SET](sql-ref-syntax-aux-conf-mgmt-set.md)