---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CACHE TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 CACHE TABLE 语法。
ms.openlocfilehash: 4985e18afe792f8cb0bdc25fbdb2ac9df6cdea99
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060644"
---
# <a name="cache-table"></a>CACHE TABLE

使用给定的存储级别来缓存表的内容或查询的输出。 如果缓存了某个查询，则会为此查询创建临时视图。 这样就会减少未来查询中对原始文件的扫描。

## <a name="syntax"></a>语法

```sql
CACHE [ LAZY ] TABLE table_identifier
    [ OPTIONS ( 'storageLevel' [ = ] value ) ] [ [ AS ] query ]
```

## <a name="parameters"></a>参数

* LAZY

  只在首次使用表时才对表进行缓存，而不是立即缓存。

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

  要缓存的表或视图的名称。 可以选择使用数据库名称来限定表或视图的名称。

* OPTIONS ( ‘storageLevel’ [ = ] value )

  具有 ``storageLevel`` 键值对的 ``OPTIONS`` 子句。 在使用 ``storageLevel`` 以外的键时，将会发出警告。 ``storageLevel`` 的有效选项是：

  * ``NONE``
    * ``DISK_ONLY``
    * ``DISK_ONLY_2``
    * ``MEMORY_ONLY``
    * ``MEMORY_ONLY_2``
    * ``MEMORY_ONLY_SER``
    * ``MEMORY_ONLY_SER_2``
    * ``MEMORY_AND_DISK``
    * ``MEMORY_AND_DISK_2``
    * ``MEMORY_AND_DISK_SER``
    * ``MEMORY_AND_DISK_SER_2``
    * ``OFF_HEAP``

  在为 ``storageLevel`` 设置了无效值时，会引发异常。 如果未使用 ``OPTIONS`` 子句显式设置 ``storageLevel``，则会将默认 ``storageLevel`` 设置为 ``MEMORY_AND_DISK``。

* **查询**

  生成要缓存的行的查询。 可以采用以下格式之一：

  * ``SELECT`` 语句
  * `TABLE` 语句
  * ``FROM`` 语句

## <a name="examples"></a>示例

```sql
CACHE TABLE testCache OPTIONS ('storageLevel' 'DISK_ONLY') SELECT * FROM testData;
```

## <a name="related-statements"></a>相关语句

* [CLEAR CACHE](sql-ref-syntax-aux-cache-clear-cache.md)
* [UNCACHE TABLE](sql-ref-syntax-aux-cache-uncache-table.md)
* [REFRESH TABLE](sql-ref-syntax-aux-cache-refresh-table.md)
* [REFRESH](sql-ref-syntax-aux-cache-refresh.md)
* [REFRESH FUNCTION](sql-ref-syntax-aux-cache-refresh-function.md)