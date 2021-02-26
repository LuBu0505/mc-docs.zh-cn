---
title: 命令和查询管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的命令和查询管理。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/08/2021
ms.openlocfilehash: d5306d77df55aae33f4d0528a9e007caa2c7bc6f
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101087633"
---
# <a name="commands-and-queries-management"></a>命令和查询管理

## <a name="show-commands-and-queries"></a>.show commands-and-queries 

`.show` `commands-and-queries` 返回一个表，其中包含已达到最终状态的管理员命令和查询。 这些命令和查询的可用时间为 30 天。

此命令的输出中显示的信息类似于 [`.show` 命令](commands.md)和 [`.show` 查询](queries.md)，但是它实际上上允许你以简单方式联接两个结果集。

**语法**

`.show` `commands-and-queries`
 
**输出**
 
输出架构如下所示：

| ColumnName               | ColumnType |
|--------------------------|------------|
| ClientActivityId         | string     |
| CommandType              | string     |
| 文本                     | string     |
| 数据库                 | string     |
| StartedOn                | datetime   |
| LastUpdatedOn            | datetime   |
| 持续时间                 | timespan   |
| State                    | 字符串     |
| FailureReason            | string     |
| RootActivityId           | GUID       |
| User                     | string     |
| 应用程序              | string     |
| 主体                | string     |
| ClientRequestProperties  | 动态    |
| TotalCpu                 | timespan   |
| MemoryPeak               | long       |
| CacheStatistics          | 动态    |
| ScannedExtentsStatistics | 动态    |
| ResultSetStatistics      | 动态    |
| WorkloadGroup            | 字符串     |

> [!NOTE]
> 对于查询，`CommandType` 的值为 `Query`。
