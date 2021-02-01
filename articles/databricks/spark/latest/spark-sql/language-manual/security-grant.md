---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/10/2020
title: GRANT - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 GRANT 语法。
ms.openlocfilehash: b2c3717c7c184607f96b0af44735569f857c7d36
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060826"
---
# <a name="grant"></a>GRANT

向用户或主体授予对象权限。 授予对数据库的权限（例如 ``SELECT`` 权限）会隐式授予对该数据库中所有对象的此权限。 授予对目录的特定权限会隐式授予对目录中所有数据库的此权限。

## <a name="syntax"></a>语法

```sql
GRANT
  privilege_type [, privilege_type ] ...
  ON (CATALOG | DATABASE <database-name> | TABLE <table-name> | VIEW <view-name> | FUNCTION <function-name> | ANONYMOUS FUNCTION | ANY FILE)
  TO principal

privilege_type
  : SELECT | CREATE | MODIFY | USAGE | READ_METADATA | CREATE_NAMED_FUNCTION | ALL PRIVILEGES

principal
  : `<user>@<domain-name>` | <group-name>
```

若要授予所有用户某个权限，请在 ``TO`` 之后指定关键字 ``users``。

## <a name="examples"></a>示例

```sql
GRANT USAGE, SELECT ON DATABASE <database-name> TO `<user>@<domain-name>`
GRANT SELECT ON ANONYMOUS FUNCTION TO `<user>@<domain-name>`
GRANT SELECT ON ANY FILE TO `<user>@<domain-name>`
```

## <a name="view-based-access-control"></a>基于视图的访问控制

可以通过授予对包含任意查询的派生视图的访问权限来配置细粒度的访问控制（例如，对符合特定条件的行和列）。

### <a name="examples"></a>示例

```sql
CREATE OR REPLACE VIEW <view-name> AS SELECT columnA, columnB FROM <table-name> WHERE columnC > 1000;
GRANT SELECT ON VIEW <view-name> TO `<user>@<domain-name>`;
```

有关所需表所有权的详细信息，请参阅[常见问题 (FAQ)](../../../../security/access-control/table-acls/object-privileges.md#frequently-asked-questions-faq)。