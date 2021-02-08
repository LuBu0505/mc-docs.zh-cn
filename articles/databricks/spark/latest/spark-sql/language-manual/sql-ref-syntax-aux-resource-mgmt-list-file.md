---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: LIST FILE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 LIST FILE 语法。
ms.openlocfilehash: 58e23aebed93bfa32880a98217ae6e3f81a9d7f0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060709"
---
# <a name="list-file"></a>LIST FILE

列出通过 [ADD FILE](sql-ref-syntax-aux-resource-mgmt-add-file.md) 添加的资源。

## <a name="syntax"></a>语法

```sql
LIST FILE
```

## <a name="examples"></a>示例

```sql
ADD FILE /tmp/test;
ADD FILE /tmp/test_2;
LIST FILE;
-- output for LIST FILE
file:/private/tmp/test
file:/private/tmp/test_2

LIST FILE /tmp/test /some/random/file /another/random/file
--output
file:/private/tmp/test
```

## <a name="related-statements"></a>相关语句

* [ADD FILE](sql-ref-syntax-aux-resource-mgmt-add-file.md)
* [ADD JAR](sql-ref-syntax-aux-resource-mgmt-add-jar.md)
* [LIST JAR](sql-ref-syntax-aux-resource-mgmt-list-jar.md)