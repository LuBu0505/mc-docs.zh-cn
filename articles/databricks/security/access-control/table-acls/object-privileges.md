---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/01/2020
title: 数据对象权限 - Azure Databricks
description: 了解如何在 Azure Databricks 中设置表、数据库、视图、函数及其子集的权限。
ms.openlocfilehash: 8e63cf1fc4dd139a01b4933f3316ced7d58a9641
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058668"
---
# <a name="data-object-privileges"></a>数据对象特权

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。

利用 Azure Databricks 数据治理模型，你可以采用编程方式通过 Spark SQL 授予、拒绝和撤消对数据的访问权限。 利用此模型，你可以控制对安全对象（如表、数据库、视图和函数）的访问权限。 它还允许对从任意查询创建的派生视图设置权限，从而进行精细的访问控制（例如对表的特定子集）。 在启用了表访问控制的群集和 SQL 终结点上，Azure Databricks SQL 查询分析器在运行时强制执行这些访问控制策略。

本文将介绍组成 Azure Databricks 数据治理模型的权限、对象和所有权规则。 还将介绍如何授予、拒绝和撤消对象权限。

## <a name="requirements"></a>要求

管理对象权限的要求取决于你的环境：

* [Azure Databricks 工作区](#azure-databricks-workspace)
* [Azure Databricks SQL Analytics](#azure-databricks-sql-analytics)

### <a name="azure-databricks-workspace"></a>Azure Databricks 工作区

* 管理员必须为工作区[启用并强制执行表访问控制](../../../administration-guide/access-control/table-acl.md)。
* 必须启用群集才能进行[表访问控制](table-acl.md#table-access-control)。

### <a name="azure-databricks-sql-analytics"></a>Azure Databricks SQL Analytics

请参阅[管理员快速入门要求](../../../sql/admin/admin-quickstart.md#requirements)。

## <a name="data-governance-model"></a>数据治理模型

本部分介绍 Azure Databricks 的数据治理模型。 对数据对象的访问权限由权限控制。

### <a name="privileges"></a>权限

* ``SELECT``：授予对对象的只读访问权限。
* ``CREATE``：提供创建对象（例如数据库中的表）的功能。
* ``MODIFY``：提供在对象中添加、删除和修改数据的功能。
* ``USAGE``：不提供任何功能，只是对在数据库对象上执行任何操作的一个附加要求。
* ``READ_METADATA``：提供查看对象及其元数据的功能。
* ``CREATE_NAMED_FUNCTION``：提供在现有目录或数据库中创建命名 UDF 的功能。
* ``MODIFY_CLASSPATH``：提供将文件添加到 Spark 类路径的功能。
* ``ALL PRIVILEGES``：提供所有权限（转换为上述所有权限）。

#### <a name="usage-privilege"></a>``USAGE`` 权限

若要对数据库对象执行某个操作，除了需要具有执行该操作的权限之外，用户还必须对该数据库具有 ``USAGE`` 权限。 以下任何一项都满足 ``USAGE`` 要求：

* 是管理员
* 对数据库具有 ``USAGE`` 权限，或者是对数据库具有 ``USAGE`` 权限的组的成员
* 对 ``CATALOG`` 具有 ``USAGE`` 权限，或者是具有 ``USAGE`` 权限的组的成员
* 是数据库的所有者，或者是拥有该数据库的组的成员

即使是数据库中某个对象的所有者，也必须具有 ``USAGE`` 权限才能使用该对象。

例如，管理员可以定义供其使用的 ``finance`` 组和 ``accounting`` 数据库。
若要设置一个仅供财务团队使用和共享的数据库，管理员可以执行以下命令：

```sql
CREATE DATABASE accounting;
GRANT USAGE ON DATABASE accounting TO finance;
GRANT CREATE ON DATABASE accounting TO finance;
```

有了这些权限，``finance`` 组的成员就可以在 ``accounting`` 数据库中创建表和视图，但不能与对 ``accounting`` 数据库没有 ``USAGE`` 权限的任何主体共享这些表或视图。

##### <a name="azure-databricks-workspace-and-databricks-runtime-version-behavior"></a>Azure Databricks 工作区和 Databricks Runtime 版本行为

* 运行 Databricks Runtime 7.3 LTS 及更高版本的群集会强制实施 ``USAGE`` 权限。
* 运行 Databricks Runtime 7.2 及更低版本的群集不强制实施 ``USAGE`` 权限。
* 为了确保现有工作负荷的功能不变，在引入 ``USAGE`` 之前已使用表访问控制的工作区中，我们向 ``users`` 组授予了对 ``CATALOG`` 的 ``USAGE`` 权限。 如果要利用 ``USAGE`` 权限，则必须根据需要依次运行 ``REVOKE USAGE ON CATALOG FROM users`` 和 ``GRANT USAGE ...``。

#### <a name="privilege-hierarchy"></a>权限层次结构

Azure Databricks 中的 SQL 对象是分层的，权限会被继承。 这意味着授予或拒绝对 ``CATALOG`` 的某个权限时，会自动授予或拒绝对目录中的所有数据库的该权限。 同样，在 ``DATABASE`` 对象上授予的权限会被该数据库中的所有对象继承。

### <a name="objects"></a>对象

权限应用于下列类型的对象：

* ``CATALOG``：控制对整个数据目录的访问。
  * ``DATABASE``：控制对数据库的访问。
    * ``TABLE``：控制对托管表或外部表的访问。
    * ``VIEW``：控制对 SQL 视图的访问。
    * ``FUNCTION``：控制对命名函数的访问。
* ``ANONYMOUS FUNCTION``：控制对[匿名函数或临时函数](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-function.md)的访问。
* ``ANY FILE``：控制对基础文件系统的访问。

  > [!NOTE]
  >
  > 具有此权限的用户可以绕过在表上施加的限制，直接从文件系统进行读取。

### <a name="object-ownership"></a>对象所有权

在群集或 SQL 终结点上启用表访问控制后，创建数据库、表、视图或函数的用户将成为其所有者。 所有者被授予所有权限，并且可以向其他用户授予权限。

组可能拥有对象，在这种情况下，该组的所有成员都会被视为所有者。

所有权决定是否可以将派生对象的权限授予其他用户。
例如，假设用户 A 拥有表 T，并授予用户 B 对表 T 的 ``SELECT`` 权限。即使用户 B 可以从表 T 中进行选择，用户 B 也不能向用户 C 授予对表 T 的 ``SELECT`` 权限，因为用户 A 仍是基础表 T 的所有者。此外，用户 B 不能仅通过在表 T 上创建视图 V，并向用户 C 授予对该视图的权限来规避此限制。当 Azure Databricks 检查用户 C 访问视图 V 的权限时，还会检查 V 和基础表 T 的所有者是否相同。 如果所有者不相同，则用户 C 还必须对基础表 T 具有 ``SELECT`` 权限。

如果在群集上禁用了表访问控制，则创建数据库、表、视图或函数时不会注册所有者。 若要测试某个对象是否具有所有者，请运行 ``SHOW GRANT ON <object-name>``。
如果未看到带有 ``ActionType OWN`` 的条目，则该对象没有所有者。

#### <a name="assign-owner-to-object"></a>为对象分配所有者

管理员可以使用 `` ALTER <object> OWNER TO `<user-name>@<user-domain>.com` `` 命令为对象分配所有者：

```sql
ALTER DATABASE <database-name> OWNER TO `<user-name>@<user-domain>.com`
ALTER TABLE <table-name> OWNER TO `group_name`
ALTER VIEW <view-name> OWNER TO `<user-name>@<user-domain>.com`
```

### <a name="users-and-groups"></a>用户和组

管理员和所有者可以向[用户和组](../../../administration-guide/users-groups/index.md)授予权限。
每个用户都通过其在 Azure Databricks 中的用户名中唯一标识（通常映射到其电子邮件地址）。
所有用户都隐式成为“所有用户”组的一部分，在 SQL 中表示为 ``users``。

> [!NOTE]
>
> 必须将用户规范括在反引号 (``) 中，而不是单引号 (‘’) 中。

### <a name="operations-and-privileges"></a>操作和权限

在 Azure Databricks 中，允许管理员用户执行所有操作。 无论对象的所有者是谁，管理员都可以授予、撤销或拒绝对任何对象的权限。 他们还可以更改任何对象的所有者，对于在未启用表访问控制的群集上创建的对象而言，这可能是必需的。 对象的所有者可以对该对象执行任何操作，还可以向其他主体授予对该对象的权限。 唯一的例外是对象在数据库中的情况；若要对该对象执行任何操作，用户还必须对该数据库具有 ``USAGE`` 权限。

下表将 SQL 操作映射到执行该操作所需的权限。

> [!NOTE]
>
> * 在需要对某个表、视图或函数具有权限的任何位置，还需要对其所在的数据库具有 ``USAGE`` 权限。
> * 不管什么位置，如果可以在命令中引用表，则还可以引用路径。 在这些实例中，需要具有对 ``ANY_FILE`` 的 ``SELECT`` 或 ``MODIFY`` 权限，而不需要具有对数据库的 ``USAGE`` 权限以及对表的其他权限。
> * 对象所有权在此处表示为 ``OWN`` 权限。

| Operation                                                                                                     | 所需的特权                                                                                                                                              |
|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [CLONE](../../../spark/latest/spark-sql/language-manual/delta-clone.md)                                       | 对要克隆的表具有 ``SELECT`` 权限，对数据库具有 ``CREATE`` 权限，并且具有 ``MODIFY`` 权限（如果要替换表）。                                  |
| [COPY INTO](../../../spark/latest/spark-sql/language-manual/delta-copy-into.md)                               | 对 ``ANY_FILE`` 具有 ``SELECT`` 权限（如果从路径复制）、对要将数据复制到其中的表具有 ``MODIFY`` 权限。                                                                    |
| [CREATE BLOOMFILTER INDEX](../../../spark/latest/spark-sql/language-manual/delta-create-bloomfilter-index.md) | 对要编制索引的表具有 ``OWN`` 权限。                                                                                                                              |
| [CREATE DATABASE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-database.md)      | 对 ``CATALOG`` 具有 ``CREATE`` 权限。                                                                                                                                   |
| [CREATE TABLE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-table.md)            | 对数据库具有 ``OWN`` 权限，或者对数据库同时具有 ``USAGE`` 和 ``CREATE`` 权限。                                                                                                 |
| [CREATE VIEW](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-view.md)              | 对数据库具有 ``OWN`` 权限，或者对数据库同时具有 ``USAGE`` 和 ``CREATE`` 权限。                                                                                                 |
| [CREATE FUNCTION](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-function.md)      | 对数据库具有 ``OWN`` 权限，或者对数据库具有 ``USAGE`` 和 ``CREATE_NAMED_FUNCTION`` 权限。 如果指定了资源，则还需要具有对 ``CATALOG`` 的 ``MODIFY_CLASSPATH`` 权限。 |
| [ALTER DATABASE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-alter-database.md)        | 对数据库具有 ``OWN`` 权限。                                                                                                                                         |
| [ALTER TABLE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-alter-table.md)              | 通常需要对表具有 ``OWN`` 权限。 如果仅添加或删除分区，则需要具有 ``MODIFY`` 权限。                                                                                  |
| [ALTER VIEW](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-alter-view.md)                | 对视图具有 ``OWN`` 权限。                                                                                                                                             |
| [DROP BLOOMFILTER INDEX](../../../spark/latest/spark-sql/language-manual/delta-drop-bloomfilter-index.md)     | 对表具有 ``OWN`` 权限。                                                                                                                                            |
| [DROP DATABASE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-drop-database.md)          | 对数据库具有 ``OWN`` 权限。                                                                                                                                         |
| [DROP TABLE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-drop-table.md)                | 对表具有 ``OWN`` 权限。                                                                                                                                            |
| [DROP VIEW](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-drop-view.md)                  | 对视图具有 ``OWN`` 权限。                                                                                                                                             |
| [.DROP FUNCTION](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-drop-function.md)          | 对函数具有 ``OWN`` 权限。                                                                                                                                         |
| [EXPLAIN](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-qry-explain.md)                      | 对表和视图具有 ``READ_METADATA`` 权限。                                                                                                                       |
| [DESCRIBE TABLE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-aux-describe-table.md)        | 对表具有 ``READ_METADATA`` 权限。                                                                                                                                  |
| [DESCRIBE HISTORY](../../../spark/latest/spark-sql/language-manual/delta-describe-history.md)                 | 对表具有 ``OWN`` 权限。                                                                                                                                            |
| [SELECT](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-qry-select.md)                        | 对表具有 ``SELECT`` 权限。                                                                                                                                         |
| [INSERT](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-dml-insert.md)                        | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [RESTORE TABLE](../../../spark/latest/spark-sql/language-manual/delta-restore.md)                             | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [UPDATE](../../../spark/latest/spark-sql/language-manual/delta-update.md)                                     | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [MERGE INTO](../../../spark/latest/spark-sql/language-manual/delta-merge-into.md)                             | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [DELETE FROM](../../../spark/latest/spark-sql/language-manual/delta-delete-from.md)                           | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [TRUNCATE TABLE](../../../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-truncate-table.md)        | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [OPTIMIZE](../../../spark/latest/spark-sql/language-manual/delta-optimize.md)                                 | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [VACUUM](../../../spark/latest/spark-sql/language-manual/delta-vacuum.md)                                     | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [FSCK REPAIR TABLE](../../../spark/latest/spark-sql/language-manual/delta-fsck.md)                            | 对表具有 ``MODIFY`` 权限。                                                                                                                                         |
| [MSCK](../../../spark/latest/spark-sql/language-manual/security-msck.md)                                      | 对表具有 ``OWN`` 权限。                                                                                                                                            |
| [GRANT](../../../spark/latest/spark-sql/language-manual/security-grant.md)                                    | 对对象具有 ``OWN`` 权限。                                                                                                                                           |
| [SHOW GRANT](../../../spark/latest/spark-sql/language-manual/security-show-grant.md)                          | 对对象具有 ``OWN`` 权限。                                                                                                                                           |
| [DENY](../../../spark/latest/spark-sql/language-manual/security-deny.md)                                      | 对对象具有 ``OWN`` 权限。                                                                                                                                           |
| [REVOKE](../../../spark/latest/spark-sql/language-manual/security-revoke.md)                                  | 对对象具有 ``OWN`` 权限。                                                                                                                                           |

> [!IMPORTANT]
>
> 使用表访问控制时，``DROP TABLE`` 语句区分大小写。 如果表名为小写，且 ``DROP TABLE`` 使用混合或大写形式引用表名，则 ``DROP TABLE`` 语句会失败。

## <a name="manage-object-privileges"></a>管理对象权限

可以使用 ``GRANT``、``DENY``、``REVOKE``、``MSCK`` 和 ``SHOW GRANT`` 操作来管理对象权限。

> [!NOTE]
>
> * 对象的所有者或管理员可以执行 ``GRANT``、``DENY``、``REVOKE`` 和 ``SHOW GRANT`` 操作。 但是，管理员不能拒绝所有者的权限或撤消所有者的权限。
> * 不是所有者或管理员的主体只能在授予了所需的权限后才可以执行操作。
> * 若要为所有用户授予、拒绝或撤销权限，请在 ``TO`` 之后指定关键字 ``users``。 例如，
>
>   ```sql
>   GRANT SELECT ON ANY FILE TO users
>   ```

### <a name="examples"></a>示例

```sql
GRANT SELECT ON DATABASE <database-name> TO `<user>@<domain-name>`
GRANT SELECT ON ANONYMOUS FUNCTION TO `<user>@<domain-name>`
GRANT SELECT ON ANY FILE TO `<user>@<domain-name>`

SHOW GRANT `<user>@<domain-name>` ON DATABASE <database-name>

DENY SELECT ON <table-name> TO `<user>@<domain-name>`

REVOKE ALL PRIVILEGES ON DATABASE default FROM `<user>@<domain-name>`
REVOKE SELECT ON <table-name> FROM `<user>@<domain-name>`

GRANT SELECT ON ANY FILE TO users
```

## <a name="dynamic-view-functions"></a>动态视图函数

Azure Databricks 包括了两个用户函数，它们允许你在视图定义的主体中动态地表示列级和行级权限。

* ``current_user()``：返回当前用户名。
* ``is_member()``：确定当前用户是否为特定 Azure Databricks [组](../../../administration-guide/users-groups/groups.md)的成员。

> [!NOTE]
>
> 在 Databricks Runtime 7.3 LTS 及更高版本中可用。 但是，若要在 Databricks Runtime 7.3 LTS 中使用这些函数，必须设置 [Spark 配置](../../../clusters/configure.md#spark-config) ``spark.databricks.userInfoFunctions.enabled true``。

请考虑以下示例，该示例将两个函数组合使用来确定用户是否具有适当的组成员身份：

```sql
-- Return: true if the user is a member and false if they are not
SELECT
  current_user as user,
-- Check to see if the current user is a member of the "Managers" group.
  is_member("Managers") as admin
```

允许管理员在一个视图中为多个用户和组设置细粒度权限的功能既有表现力，又很强大，同时节省了管理开销。

### <a name="column-level-permissions"></a>列级权限

通过动态视图，可以轻松地对特定组或用户能够查看哪些列进行限制。 请考虑以下示例，其中只有属于 ``auditors`` 组的用户才能查看 ``sales_raw`` 表中的电子邮件地址。 在分析时，Spark 会将 ``CASE`` 语句替换为文本 ``'REDACTED'`` 或 ``email`` 列。 此行为允许 Spark 提供的所有常见性能优化。

```sql
-- Alias the field 'email' to itself (as 'email') to prevent the
-- permission logic from showing up directly in the column name results.
CREATE VIEW sales_redacted AS
SELECT
  user_id,
  CASE WHEN
    is_member('auditors') THEN email
    ELSE 'REDACTED'
  END AS email,
  country,
  product,
  total
FROM sales_raw
```

### <a name="row-level-permissions"></a>行级权限

使用动态视图，你可以指定低至行级或字段级的权限。 请考虑以下示例，其中只有属于 ``managers`` 组的用户才能查看大于 $1,000,000.00 的交易金额（``total`` 列）：

```sql
CREATE VIEW sales_redacted AS
SELECT
  user_id,
  country,
  product,
  total
FROM sales_raw
WHERE
  CASE
    WHEN is_member('managers') THEN TRUE
    ELSE total <= 1000000
  END;
```

### <a name="data-masking"></a>数据屏蔽

如前面的示例所示，你可以实施列级屏蔽，以防止用户查看特定的列数据，除非用户在正确的组中。 因为这些视图是标准 Spark SQL，所以你可以通过更复杂的 SQL 表达式执行更高级类型的屏蔽。 以下示例允许所有用户对电子邮件域执行分析，但允许 ``auditors`` 组的成员查看用户的完整电子邮件地址。

```sql
-- The regexp_extract function takes an email address such as
-- user.x.lastname@example.com and extracts 'example', allowing
-- analysts to query the domain name

CREATE VIEW sales_redacted AS
SELECT
  user_id,
  region,
  CASE
    WHEN is_member('auditors') THEN email
    ELSE regexp_extract(email, '^.*@(.*)$', 1)
  END
  FROM sales_raw
```

## <a name="frequently-asked-questions-faq"></a>常见问题解答 (FAQ)

**如何为所有用户授予、拒绝或撤消权限**

在 ``TO`` 或 ``FROM`` 之后指定关键字 ``users``。 例如：

```sql
GRANT SELECT ON TABLE database.table TO users
```

**我创建了一个对象，但现在不能对其进行查询、删除或修改。**

之所以会发生此错误，是因为你在未启用表访问控制的群集或 SQL 终结点上创建了该对象。 如果在群集或 SQL 终结点上禁用了表访问控制，则在创建数据库、表或视图时不会注册所有者。 管理员必须使用以下命令为对象分配所有者：

```sql
ALTER [DATABASE | TABLE | VIEW] <object-name> OWNER TO `<user-name>@<user-domain>.com`;
```

**如何授予全局和本地临时视图的权限？**

不支持对全局和本地临时视图的权限。 本地临时视图仅在同一会话中可见，在 ``global_temp`` 数据库中创建的视图对共享某个群集或 SQL 终结点的所有用户可见。 但是，会强制执行对任何临时视图所引用的基础表和视图的权限。

**如何同时向用户或组授予多个表的权限？**

授予、拒绝或撤销语句一次只能应用于一个对象。 建议通过数据库向主体组织并授予多个表的权限。 如果授予数据库的主体 ``SELECT`` 权限，则会向该数据库中的所有表和视图隐式授予该主体 ``SELECT`` 权限。 例如，如果数据库 D 具有表 t1 和 t2，并且管理员发出以下 ``GRANT`` 命令：

```sql
GRANT USAGE, SELECT ON DATABASE D TO `<user>@<domain-name>`
```

主体 ``<user>@<domain-name>`` 可以从表 t1 和 t2 中进行选择，还可以从将来在数据库 D 中创建的任何表和视图进行选择。

**如何向用户授予除一个表之外的所有表的权限？**

向数据库授予 ``SELECT`` 权限，然后拒绝要限制访问的特定表的 ``SELECT`` 权限。

```sql
GRANT USAGE, SELECT ON DATABASE D TO `<user>@<domain-name>`
DENY SELECT ON TABLE D.T TO `<user>@<domain-name>`
```

主体 ``<user>@<domain-name>`` 可以从 D 中的所有表中选择（D.T 除外）。

**用户对表 T 的视图具有 ``SELECT`` 权限，但当该用户尝试从该视图中 ``SELECT`` 时，他们会收到错误 ``User does not have privilege SELECT on table``。**

此常见错误可能是由以下任一原因所致：

* 表 T 没有已注册的所有者，因为它是使用禁用了表访问控制的群集或 SQL 终结点创建的。
* 表 T 视图上的 ``SELECT`` 权限的授予者不是表 T 的所有者，或者用户在表 T 上也没有 ``SELECT`` 权限。

假设有一个表 T 由 A 拥有。A 拥有 T 上的视图 V1，B 拥有 T 上的视图 V2。

* 如果 A 已授予对视图 V1 的 ``SELECT`` 权限，则用户可以在 V1 上选择。
* 如果 A 已授予对表 T 的 ``SELECT`` 权限，并且 B 已授予对 V2 的 ``SELECT`` 权限，则用户可以在 V2 上选择。

如[对象所有权](#object-ownership)部分中所述，这些条件确保只有对象的所有者才能向其他用户授予对该对象的访问权限。

我尝试在已启用表访问控制的群集上运行 ``sc.parallelize``，但失败了。

在已启用表访问控制的群集上，只能使用 Spark SQL 和 Python 数据帧 API。 出于安全原因，不允许使用 RDD API，因为 Azure Databricks 无法检查和授权 RDD 中的代码。