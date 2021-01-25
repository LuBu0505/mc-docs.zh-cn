---
title: 从 SQL 到 Azure Monitor 日志查询备忘单 | Azure Docs
description: 帮助熟悉 SQL 的用户在 Azure Monitor 中编写日志查询。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
origin.date: 08/21/2018
ms.date: 01/12/2021
ms.openlocfilehash: f77c8185cc90d0480ecc9d886e1ee22f7439fb80
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98231082"
---
# <a name="sql-to-azure-monitor-log-query-cheat-sheet"></a>从 SQL 到 Azure Monitor 日志查询备忘单 

下表可帮助熟悉 SQL 的用户学习 Kusto 查询语言，用以在 Azure Monitor 中编写日志查询。 让我们看一下用于解决常见场景的 T-SQL 命令以及 Azure Monitor 日志查询中的等效命令。

## <a name="sql-to-azure-monitor"></a>从 SQL 到 Azure Monitor

说明 |SQL 查询 | Azure Monitor 日志查询
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
选择表中的所有数据 |`SELECT * FROM dependencies` |<code>dependencies</code>
选择表中的特定列 |`SELECT name, resultCode FROM dependencies` |<code>dependencies <br>&#124; project name, resultCode</code>
选择表中的 100 条记录 |`SELECT TOP 100 * FROM dependencies` |<code>dependencies <br>&#124; take 100</code>
Null 评估 |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL` |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
字符串比较：相等 |`SELECT * FROM dependencies WHERE name = "abcde"` |<code>dependencies <br>&#124; where name == "abcde"</code>
字符串比较：子字符串 |`SELECT * FROM dependencies WHERE name like "%bcd%"` |<code>dependencies <br>&#124; where name contains "bcd"</code>
字符串比较：通配符 |`SELECT * FROM dependencies WHERE name like "abc%"` |<code>dependencies <br>&#124; where name startswith "abc"</code>
日期比较：上 1 天 |`SELECT * FROM dependencies WHERE timestamp > getdate()-1` |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
日期比较：日期范围 |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'` |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
布尔比较 |`SELECT * FROM dependencies WHERE !(success)` |<code>dependencies <br>&#124; where success == "False" </code>
排序 |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc` |<code>dependencies <br>&#124; order by timestamp asc </code>
Distinct |`SELECT DISTINCT name, type  FROM dependencies` |<code>dependencies <br>&#124; summarize by name, type </code>
分组，聚合 |`SELECT name, AVG(duration) FROM dependencies GROUP BY name` |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
列别名，扩展 |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name` |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
按度量值排序的前 n 条记录 |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc` |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Union |`SELECT * FROM dependencies UNION SELECT * FROM exceptions` |<code>union dependencies, exceptions</code>
联合：带条件 |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions WHERE value < 5` |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Join |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>后续步骤

- 通读有关[在 Azure Monitor 中编写日志查询](get-started-queries.md)的课程。

