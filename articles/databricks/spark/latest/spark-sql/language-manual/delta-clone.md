---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/07/2020
title: CLONE（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 CLONE 语法。
ms.openlocfilehash: 02960d803533de01ccb861e4cb17194be6e3f722
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061174"
---
# <a name="clone-delta-lake-on-azure-databricks"></a>CLONE（Azure Databricks 上的 Delta Lake）

> [!NOTE]
>
> 可在 Databricks Runtime 7.2 及更高版本中使用。

将源 Delta 表克隆到特定版本的目标位置。 克隆可以是深层克隆，也可以是浅表克隆：深层克隆会复制源中的数据，而浅表克隆则不复制。

> [!IMPORTANT]
>
> 浅表克隆和深层克隆之间存在重要差异，这也决定了如何更好地使用它们。 请参阅[克隆 Delta 表](../../../../delta/delta-utility.md#clone-delta-table)。

## <a name="syntax"></a>语法

```
CREATE TABLE [IF NOT EXISTS] target_table_identifier
[SHALLOW | DEEP] CLONE source_table_identifier [<time_travel_version>]
[LOCATION 'path']
[TBLPROPERTIES (
  'prop1' = 'value1',
  ...
)]
```

```
[CREATE OR] REPLACE TABLE target_table_identifier
[SHALLOW | DEEP] CLONE source_table_identifier [<time_travel_version>]
[LOCATION 'path']
[TBLPROPERTIES (
  'prop1' = 'value1',
  ...
)]
```

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

其中

```sql
<time_travel_version>  =
  TIMESTAMP AS OF timestamp_expression |
  VERSION AS OF version
```

* 请指定 ``CREATE IF NOT EXISTS`` 以避免在表 ``target_table`` 已存在的情况下创建它。 如果目标位置已存在表，则克隆操作将是一个无操作的操作。
* 请指定 ``CREATE OR REPLACE`` 以在表 ``target_table`` 已存在的情况下替换克隆操作的目标。 在已使用表名的情况下，这将使用新表更新元存储。
* 如果未指定 ``SHALLOW`` 或 ``DEEP``，则默认情况下会创建深层克隆。
* ``LOCATION`` 会创建一个外部表，并将所提供的位置作为存储数据的路径。 如果目标是路径而不是表名，则此操作会失败。

## <a name="examples"></a>示例

可以使用 ``CLONE`` 执行复杂的操作，例如数据迁移、数据存档、机器学习流复现、短期试验、数据共享等。 如需一些示例，请参阅[克隆用例](../../../../delta/delta-utility.md#clone-use-cases)。