---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: ADD FILE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 ADD FILE 语法。
ms.openlocfilehash: de825cd51c6e72a1af85a0fc1464a3983f515835
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060711"
---
# <a name="add-file"></a>ADD FILE

向资源列表添加一个文件和一个目录。 可以使用 [LIST FILE](sql-ref-syntax-aux-resource-mgmt-list-file.md) 列出已添加的资源。

## <a name="syntax"></a>语法

```sql
ADD FILE resource_name
```

## <a name="parameters"></a>参数

* resource_name

  要添加的文件或目录的名称。

## <a name="examples"></a>示例

```sql
ADD FILE /tmp/test;
ADD FILE "/path/to/file/abc.txt";
ADD FILE '/another/test.txt';
ADD FILE "/path with space/abc.txt";
ADD FILE "/path/to/some/directory";
```

## <a name="related-statements"></a>相关语句

* [LIST FILE](sql-ref-syntax-aux-resource-mgmt-list-file.md)
* [LIST JAR](sql-ref-syntax-aux-resource-mgmt-list-jar.md)
* [ADD JAR](sql-ref-syntax-aux-resource-mgmt-add-jar.md)