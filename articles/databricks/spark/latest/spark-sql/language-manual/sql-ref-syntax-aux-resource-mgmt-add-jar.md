---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ADD JAR - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 ADD JAR 语法。
ms.openlocfilehash: 22d535f883cdd122c7239240bbaaf193de2aebae
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060710"
---
# <a name="add-jar"></a>ADD JAR

将 JAR 文件添加到资源列表。 可以使用 [LIST JAR](sql-ref-syntax-aux-resource-mgmt-list-jar.md) 列出已添加的 JAR 文件。

## <a name="syntax"></a>语法

```sql
ADD JAR file_name
```

## <a name="parameters"></a>参数

* file_name

  要添加的 JAR 文件的名称。 文件可以在本地文件系统上，也可以在分布式文件系统上。

## <a name="examples"></a>示例

```sql
ADD JAR /tmp/test.jar;
ADD JAR "/path/to/some.jar";
ADD JAR '/some/other.jar';
ADD JAR "/path with space/abc.jar";
```

## <a name="related-statements"></a>相关语句

* [LIST JAR](sql-ref-syntax-aux-resource-mgmt-list-jar.md)
* [ADD FILE](sql-ref-syntax-aux-resource-mgmt-add-file.md)
* [LIST FILE](sql-ref-syntax-aux-resource-mgmt-list-file.md)