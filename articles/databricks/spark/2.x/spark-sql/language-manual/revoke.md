---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/08/2020
title: 撤销 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 REVOKE 语法。
ms.openlocfilehash: bd2fd2832ecf21b4b2bbd2c2a3241851b39551ea
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060727"
---
# <a name="revoke"></a>撤销

```sql
REVOKE
  privilege_type [, privilege_type ] ...
  ON (CATALOG | DATABASE <database-name> | TABLE <table-name> | VIEW <view-name> | FUNCTION <function-name> | ANONYMOUS FUNCTION | ANY FILE)
  FROM principal

privilege_type
  : SELECT | CREATE | MODIFY | READ_METADATA | CREATE_NAMED_FUNCTION | ALL PRIVILEGES

principal
  : `<user>@<domain-name>` | <group-name>
```

从用户或主体撤销显式授予或拒绝的对某个对象的权限。 ``REVOKE`` 会严格将范围限定为命令中指定的对象，而不会级联到包含的对象。

若要撤销所有用户的权限，请在 ``FROM`` 之后指定关键字 ``users``。

例如，假设存在一个具有 ``t1`` 和 ``t2`` 表的数据库 ``db``。 用户最初被授予对 ``db`` 和 ``t1`` 的 ``SELECT`` 权限。 由于数据库 ``db`` 上存在 ``GRANT``，用户可以访问 ``t2``。

如果管理员撤销对 ``db`` 的 ``SELECT`` 权限，则用户将无法再访问 ``t2``，但仍将能够访问 ``t1``，因为表 ``t1`` 上有显式的 ``GRANT``。

如果管理员改为撤销了表 ``t1`` 上的 ``SELECT``，但仍将 ``SELECT`` 保留在数据库 ``db`` 上，则用户仍然可以访问 ``t1``，因为数据库 ``db`` 上的 ``SELECT`` 隐式授予了对表 ``t1`` 的权限。

## <a name="examples"></a>示例

```sql
REVOKE ALL PRIVILEGES ON DATABASE default FROM `<user>@<domain-name>`
REVOKE SELECT ON <table-name> FROM `<user>@<domain-name>`
```