---
title: 将数据导出到外部表 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的“将数据导出到外部表”功能。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/30/2020
ms.date: 01/22/2021
ms.openlocfilehash: fb23be9a0934a5e6b3a2c5def0473e4bc263ce98
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611645"
---
# <a name="export-data-to-an-external-table"></a>将数据导出到外部表

可以通过定义[外部表](../external-table-commands.md)并将数据导出到其中来导出数据。
表属性在[创建外部表](../external-tables-azurestorage-azuredatalake.md#create-or-alter-external-table)时指定。
export 命令按名称引用外部表。

该命令需要[表管理员或数据库管理员权限](../access-control/role-based-authorization.md)。

## <a name="syntax"></a>语法

`.export` [`async`] `to` `table` ExternalTableName <br>
[`with` `(`*PropertyName* `=` *PropertyValue*`,`...`)`] <| *Query*

**参数**

* ExternalTableName：要将内容导出到其中的外部表的名称。
* Query：导出查询。
* Properties：支持使用以下属性作为可将内容导出到外部表的命令的组成部分： 
    * `sizeLimit`、`parquetRowGroupSize`、`distributed` - 有关这些属性的详细信息，请参阅[导出到存储](export-data-to-storage.md)部分。
    * `spread`、`concurrency` - 用于减少/增加写入操作并发度的属性。 有关详细信息，请参阅 [partition 运算符](../../query/partitionoperator.md)。 只有在导出到由字符串分区进行分区的外部表时，这些属性才有意义。 默认情况下，以并发方式进行导出的节点的数量将会是介于 64 和群集节点数之间的最小值。


## <a name="output"></a>输出

|输出参数 |类型 |说明
|---|---|---
|ExternalTableName  |String |外部表的名称。
|`Path`|String|输出路径。
|NumRecords|String| 导出到路径的记录数。

## <a name="notes"></a>注释

* 导出查询输出架构必须与外部表的架构（包括分区定义的所有列）匹配。 例如，如果表按 DateTime 分区，则查询输出架构必须具有与 TimestampColumnName 匹配的 Timestamp 列。 此列名在外部表分区定义中定义。

* 不能使用 export 命令重写外部表属性。
 例如，不能将 Parquet 格式的数据导出到数据格式为 CSV 的外部表。

* 如果外部表已分区，则导出的项目将根据分区定义写入到各自的目录，如[已分区外部表示例](#partitioned-external-table-example)中所示。
  * 如果分区值为 NULL 或为空，或者是无效的目录值，则根据目标存储的定义，分区值将替换为默认值 `__DEFAULT_PARTITION__`。

* 有关解决导出命令过程中出现的存储错误的建议，请参阅[导出命令过程中的失败](export-data-to-storage.md#failures-during-export-commands)。

### <a name="number-of-files"></a>文件的数目

每个分区写入的文件数取决于设置：

 * 如果外部表只包含日期/时间分区或者根本不包含分区，则每个分区（如果存在）写入的文件数应该与群集中的节点数相似（如果达到 `sizeLimit`，则需增加该数目）。 分发导出操作后，群集中的所有节点将同时导出。 若要禁用分发，以便仅单个节点执行写操作，请将 `distributed` 设为 false。 此过程会创建较少的文件，但会降低导出性能。

* 如果外部表通过一个字符串列包含一个分区，则导出的文件数应为每个分区一个文件（如果达到 `sizeLimit`，则需增加该数目）。 所有节点仍参与导出（操作已分发），但每个分区将分配给特定节点。 将 `distributed` 设为 false 会导致只有单个节点进行导出，但行为将保持不变（每个分区写入一个文件）。

## <a name="examples"></a>示例

### <a name="non-partitioned-external-table-example"></a>未分区外部表示例

ExternalBlob 是未分区外部表。 

```kusto
.export to table ExternalBlob <| T
```

|ExternalTableName|`Path`|NumRecords|
|---|---|---|
|ExternalBlob|http://storage1.blob.core.chinacloudapi.cn/externaltable1cont1/1_58017c550b384c0db0fea61a8661333e.csv|10|

### <a name="partitioned-external-table-example"></a>已分区外部表示例

PartitionedExternalBlob 是一个外部表，定义如下： 

```kusto
.create external table PartitionedExternalBlob (Timestamp:datetime, CustomerName:string) 
kind=blob
partition by (CustomerName:string=CustomerName, Date:datetime=startofday(Timestamp))   
pathformat = ("CustomerName=" CustomerName "/" datetime_pattern("yyyy/MM/dd", Date))   
dataformat=csv
( 
   h@'http://storageaccount.blob.core.chinacloudapi.cn/container1;secretKey'
)
```

```kusto
.export to table PartitionedExternalBlob <| T
```

|ExternalTableName|`Path`|NumRecords|
|---|---|---|
|ExternalBlob|http://storageaccount.blob.core.chinacloudapi.cn/container1/CustomerName=customer1/2019/01/01/fa36f35c-c064-414d-b8e2-e75cf157ec35_1_58017c550b384c0db0fea61a8661333e.csv|10|
|ExternalBlob|http://storageaccount.blob.core.chinacloudapi.cn/container1/CustomerName=customer2/2019/01/01/fa36f35c-c064-414d-b8e2-e75cf157ec35_2_b785beec2c004d93b7cd531208424dc9.csv|10|

如果命令是异步执行的（通过使用 `async` 关键字），则可以使用 [show operation details](../operations.md#show-operation-details) 命令获得输出。
