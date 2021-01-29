---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: DENY - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 DENY 语法。
ms.openlocfilehash: 32ab6224d06ad03f628064d50e4b2487e7abad77
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692141"
---
# <a name="deny"></a>DENY

拒绝用户或主体对对象的权限。 拒绝对数据库的权限（例如 ``SELECT`` 权限）会隐式拒绝对该数据库中所有对象的该权限。 拒绝对目录的特定权限会隐式拒绝对目录中所有数据库的该权限。

## <a name="syntax"></a>语法

```sql
DENY
  privilege_type [, privilege_type ] ...
  ON [CATALOG | DATABASE <database-name> | TABLE <table-name> | VIEW <view-name> | FUNCTION <function-name> | ANONYMOUS FUNCTION | ANY FILE]
  TO principal

privilege_type
  : SELECT | CREATE | MODIFY | USAGE | READ_METADATA | CREATE_NAMED_FUNCTION | ALL PRIVILEGES

principal
  : `<user>@<domain-name>` | <group-name>
```

若要拒绝所有用户的某个权限，请在 ``TO`` 之后指定关键字 ``users``。

``DENY`` 可用于确保用户或主体不能访问指定对象，即使存在任何隐式或显式的 ``GRANTs``。 访问对象时，Databricks 首先检查对象上是否存在任何显式或隐式的 ``DENYs``，然后再检查是否存在任何显式或隐式的 ``GRANTs``。

例如，假设存在一个具有 ``t1`` 和 ``t2`` 表的数据库 ``db``。 用户最初被授予对 ``db`` 的 ``SELECT`` 权限。 由于数据库 ``db`` 上存在 ``GRANT``，用户可以访问 ``t1`` 和 ``t2``。

如果管理员对表 ``t1`` 发出 ``DENY``，用户将无法再访问 ``t1``。
如果管理员对数据库 ``db`` 发出 ``DENY``，用户将无法访问 ``db`` 中的任何表，即使这些表上存在显式的 ``GRANT``。 也就是说，``DENY`` 始终会取代 ``GRANT``。

## <a name="example"></a>示例

```sql
DENY SELECT ON <table-name> TO `<user>@<domain-name>`;
```