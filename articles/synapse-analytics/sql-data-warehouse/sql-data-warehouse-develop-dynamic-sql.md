---
title: 使用动态 SQL
description: 有关在 Azure Synapse Analytics 中对专用 SQL 池使用动态 SQL 的开发解决方案技巧。
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
ms.openlocfilehash: 3de64856a53218d84f62e63c2c28572b79068905
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98021652"
---
# <a name="dynamic-sql-for-dedicated-sql-pools-in-azure-synapse-analytics"></a>Azure Synapse Analytics 中专用 SQL 池的动态 SQL

本文提供了在专用 SQL 池中使用动态 SQL 开发解决方案的技巧。

## <a name="dynamic-sql-example"></a>动态 SQL 示例

针对专用 SQL 池开发应用程序代码时，可能需要借助动态 SQL 来提供灵活、通用且模块化的解决方案。 专用 SQL 池目前不支持 blob 数据类型。

不支持 blob 数据类型可能会限制字符串的大小，因为 blob 数据类型包括 varchar(max) 和 nvarchar(max) 类型。

如果已在应用程序代码中使用这些类型构建大型字符串，则需将代码分解成区块，并改用 EXEC 语句。

一个简单的示例：

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

如果字符串较短，则可以像平时一样使用 [sp_executesql](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。

> [!NOTE]
> 作为动态 SQL 执行的语句仍会受所有 T-SQL 验证规则的约束。

## <a name="next-steps"></a>后续步骤

有关更多开发技巧，请参阅 [开发概述](sql-data-warehouse-overview-develop.md)。
