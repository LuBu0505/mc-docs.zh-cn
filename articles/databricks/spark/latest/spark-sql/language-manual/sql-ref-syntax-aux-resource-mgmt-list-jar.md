---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: LIST JAR - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 LIST JAR 语法。
ms.openlocfilehash: 7efeedb56f7dd5b676817edfe2b72bf8ff5e1f5a
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060708"
---
# <a name="list-jar"></a>LIST JAR

列出由 [ADD JAR](sql-ref-syntax-aux-resource-mgmt-add-jar.md) 添加的 JAR。

## <a name="syntax"></a>语法

```sql
LIST JAR
```

## <a name="examples"></a>示例

```sql
ADD JAR /tmp/test.jar;
ADD JAR /tmp/test_2.jar;
LIST JAR;
-- output for LIST JAR
spark://192.168.1.112:62859/jars/test.jar
spark://192.168.1.112:62859/jars/test_2.jar

LIST JAR /tmp/test.jar /some/random.jar /another/random.jar;
-- output
spark://192.168.1.112:62859/jars/test.jar
```

## <a name="related-statements"></a>相关语句

* [ADD JAR](sql-ref-syntax-aux-resource-mgmt-add-jar.md)
* [ADD FILE](sql-ref-syntax-aux-resource-mgmt-add-file.md)
* [LIST FILE](sql-ref-syntax-aux-resource-mgmt-list-file.md)