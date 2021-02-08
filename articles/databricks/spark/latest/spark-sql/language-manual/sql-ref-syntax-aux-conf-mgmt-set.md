---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SET - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SET 语法。
ms.openlocfilehash: c8ffa7b5dfaeb7a0f9e4420ee8f9f2a51a546d0f
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060629"
---
# <a name="set"></a>SET

设置属性，返回现有属性的值，或返回所有具有值和含义的 SQLConf 属性。

## <a name="syntax"></a>语法

```sql
SET
SET [ -v ]
SET property_key[ = property_value ]
```

## <a name="parameters"></a>参数

* **-v**

  输出现有 SQLConf 属性的键、值和含义。

* **property_key**

  返回指定属性键的值。

* **property_key=property_value**

  设置给定属性键的值。 如果给定的属性键具有旧值，则新值将替代该值。

## <a name="examples"></a>示例

```sql
-- Set a property.
SET spark.sql.variable.substitute=false;

-- List all SQLConf properties with value and meaning.
SET -v;

-- List all SQLConf properties with value for current session.
SET;

-- List the value of specified property key.
SET spark.sql.variable.substitute;
+-----------------------------+-----+
|                          key|value|
+-----------------------------+-----+
|spark.sql.variable.substitute|false|
+-----------------------------+-----+
```

## <a name="related-statements"></a>相关语句

* [RESET](sql-ref-syntax-aux-conf-mgmt-reset.md)