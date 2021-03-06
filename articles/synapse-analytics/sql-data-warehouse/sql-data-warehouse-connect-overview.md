---
title: 连接到 Azure Synapse 中的 SQL 池
description: 连接到 SQL 池。
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
ms.custom: azure-synapse, seo-lt-2019, devx-track-csharp
ms.openlocfilehash: ae45207b7ea3ed1d5fca73984a18c3c7abe1b1fc
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98021933"
---
# <a name="connect-to-a-sql-pool-in-azure-synapse"></a>连接到 Azure Synapse 中的 SQL 池 

连接到 Azure Synapse 中的 SQL 池。

## <a name="find-your-server-name"></a>查找服务器名称

以下示例中的服务器名称是“sqlpoolservername.database.chinacloudapi.cn”。 若要查找完全限定的服务器名称，请执行以下操作：

1. 转到 [Azure 门户](https://portal.azure.cn)。
2. 单击“Azure Synapse Analytics”  。
3. 单击要连接到的 SQL 池。
4. 找到完整的服务器名称。

   ![完整服务器名称](media/sql-data-warehouse-connect-overview/server-connect.PNG)

## <a name="supported-drivers-and-connection-strings"></a>支持的驱动程序和连接字符串

SQL 池支持 [ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)、[ODBC](https://docs.microsoft.com/sql/connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)、[PHP](https://docs.microsoft.com/sql/connect/php/overview-of-the-php-sql-driver?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [JDBC](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。 单击前述的某个驱动程序，查找最新版本和文档。

若要自动生成通过 Azure 门户使用的驱动程序的连接字符串，请单击前述示例中的“显示数据库连接字符串”  。 下面也是一些示例，说明了每个驱动程序的连接字符串的样式。

> [!NOTE]
> 请考虑将连接超时值设置为 300 秒，以便连接可以经受住短时间内不可用。

### <a name="adonet-connection-string-example"></a>ADO.NET 连接字符串示例

```csharp
Server=tcp:{your_server}.database.chinacloudapi.cn,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC 连接字符串示例

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.chinacloudapi.cn,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP 连接字符串示例

```PHP
Server: {your_server}.database.chinacloudapi.cn,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.chinacloudapi.cn,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.chinacloudapi.cn,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>JDBC 连接字符串示例

```Java
jdbc:sqlserver://yourserver.database.chinacloudapi.cn:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.chinacloudapi.cn;loginTimeout=30;
```

## <a name="connection-settings"></a>连接设置

SQL 池在连接和创建对象期间会标准化一些设置。 这些设置不能重写，其中包括：

| SQL 池设置 | 值 |
|:--- |:--- |
| [ANSI_NULLS](https://docs.microsoft.com/sql/t-sql/statements/set-ansi-nulls-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |ON |
| [QUOTED_IDENTIFIERS](https://docs.microsoft.com/sql/t-sql/statements/set-quoted-identifier-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |ON |
| [DATEFORMAT](https://docs.microsoft.com/sql/t-sql/statements/set-dateformat-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |mdy |
| [DATEFIRST](https://docs.microsoft.com/sql/t-sql/statements/set-datefirst-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |7 |

## <a name="next-steps"></a>后续步骤

若要使用 Visual Studio 进行连接和查询，请参阅[使用 Visual Studio 进行查询](sql-data-warehouse-query-visual-studio.md)。 若要详细了解身份验证选项，请参阅[向 Azure Synapse Analytics 进行身份验证](sql-data-warehouse-authentication.md)。
