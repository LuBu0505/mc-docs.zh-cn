---
title: 使用 sqlcmd 进行连接
description: 使用 sqlcmd 命令行实用程序连接并查询 Synapse SQL 池。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
origin.date: 04/17/2018
ms.date: 01/11/2021
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: d93e472863d44b5e49cd8b1abe7238e4d16a3860
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023047"
---
# <a name="connect-to-sql-pool-in-azure-synapse-analytics-with-sqlcmd"></a>使用 sqlcmd 连接到 Azure Synapse Analytics 中的 SQL 池

> [!div class="op_single_selector"]
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

使用 [sqlcmd][sqlcmd] 命令行实用程序连接并查询 SQL 池。  

## <a name="1-connect"></a>1.连接

若要开始使用 [sqlcmd][sqlcmd]，请打开命令提示符并输入“sqlcmd”，后跟 SQL 池的连接字符串。 连接字符串需要以下参数：

* **服务器 (-S)：** 采用 `<`服务器名称`>`.database.chinacloudapi.cn 格式的服务器
* **数据库 (-d)：** SQL 池名称。
* **启用带引号的标识符 (-I)：** 必须启用带引号的标识符才能连接到 SQL 池实例。

若要使用 SQL Server 身份验证，需添加用户名/密码参数：

* **用户 (-U)：** 采用 `<`User`>` 格式的服务器用户
* **密码 (-P)：** 与用户关联的密码。

例如，连接字符串可能如下所示：

```sql
C:\>sqlcmd -S MySqlDw.database.chinacloudapi.cn -d Adventure_Works -U myuser -P myP@ssword -I
```

若要使用 Azure Active Directory 集成身份验证，需添加 Azure Active Directory 参数：

* Azure Active Directory 身份验证 (-G)：使用 Azure Active Directory 进行身份验证 

例如，连接字符串可能如下所示：

```sql
C:\>sqlcmd -S MySqlDw.database.chinacloudapi.cn -d Adventure_Works -G -I
```

> [!NOTE]
> 需 [启用 Azure Active Directory 身份验证](sql-data-warehouse-authentication.md) 才能使用 Active Directory 进行身份验证。

## <a name="2-query"></a>2.查询

连接后，可以对实例发出任何支持的 Transact-SQL 语句。  在此示例中，查询以交互模式进行提交。

```sql
C:\>sqlcmd -S MySqlDw.database.chinacloudapi.cn -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

后续示例演示如何通过使用 -Q 选项或将 SQL 输送到 sqlcmd 从而在批处理模式下运行查询。

```sql
sqlcmd -S MySqlDw.database.chinacloudapi.cn -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.chinacloudapi.cn -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>后续步骤

有关 sqlcmd 中可用选项的详细信息，请参阅 [sqlcmd 文档](https://docs.microsoft.com/sql/tools/sqlcmd-utility?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。
