---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 标识符 - Azure Databricks
description: 了解 Azure Databricks 支持的 SQL 语言构造中的 SQL 标识符。
ms.openlocfilehash: 6ce7b51e6d7af1a5570c996767f2098b7d393134
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060649"
---
# <a name="identifiers"></a>标识符

标识符是用于标识数据库对象（例如表、视图、架构或列）的字符串。 Spark SQL 具有常规标识符和分隔标识符，它们均使用反引号引起来。 常规标识符和分隔标识符均不区分大小写。

## <a name="syntax"></a>语法

### <a name="regular-identifiers"></a>常规标识符

```
{ letter | digit | '_' } [ , ... ]
```

> [!NOTE]
>
> 如果 ``spark.sql.ansi.enabled`` 设置为 ``true``，则不能将 ANSI SQL 保留关键字用作标识符。 有关详细信息，请参阅 [ANSI 合规性](sql-ref-ansi-compliance.md)。

### <a name="delimited-identifiers"></a>分隔标识符

```sql
`c [ , ... ]`
```

## <a name="parameters"></a>参数

* **字母**：A-Z 或 a-z 中的任何字母。
* **数字**：0 到 9 的任意数字。
* **c**：字符集中的任何字符。 使用 `` ` `` 来转义特殊字符（例如 `` `.` ``）。

## <a name="examples"></a>示例

```sql
-- This CREATE TABLE fails with ParseException because of the illegal identifier name a.b
CREATE TABLE test (a.b int);
org.apache.spark.sql.catalyst.parser.ParseException:
no viable alternative at input 'CREATE TABLE test (a.'(line 1, pos 20)

-- This CREATE TABLE works
CREATE TABLE test (`a.b` int);

-- This CREATE TABLE fails with ParseException because special character ` is not escaped
CREATE TABLE test1 (`a`b` int);
org.apache.spark.sql.catalyst.parser.ParseException:
no viable alternative at input 'CREATE TABLE test (`a`b`'(line 1, pos 23)

-- This CREATE TABLE works
CREATE TABLE test (`a``b` int);
```