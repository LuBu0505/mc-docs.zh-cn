---
title: 排查 Azure 数据工厂连接器问题
description: 了解如何排查 Azure 数据工厂中的连接器问题。
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 12/18/2020
ms.date: 01/04/2021
ms.author: v-jay
ms.reviewer: craigg
ms.custom: has-adal-ref
ms.openlocfilehash: 408f25a53cc3cee4a8f01ca6c3d7d2652da3d045
ms.sourcegitcommit: 3f54ab515b784c9973eb00a5c9b4afbf28a930a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97894457"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>排查 Azure 数据工厂连接器问题

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文探讨了常见的 Azure 数据工厂连接器故障排除方法。
  
## <a name="azure-blob-storage"></a>Azure Blob 存储

### <a name="error-code--azurebloboperationfailed"></a>错误代码：AzureBlobOperationFailed

- 消息：`Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **原因：** Blob 存储操作遇到问题。

- **建议**：检查详细信息中的错误。 参阅 Blob 帮助文档： https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codes 。 如需帮助，请联系存储团队。


### <a name="error-code--azureblobservicenotreturnexpecteddatalength"></a>错误代码：AzureBlobServiceNotReturnExpectedDataLength

- 消息：`Error occurred when trying to fetch the blob '%name;'. This could be a transient issue and you may rerun the job. If it fails again continuously, contact customer support.`


### <a name="error-code--azureblobnotsupportmultiplefilesintosingleblob"></a>错误代码：AzureBlobNotSupportMultipleFilesIntoSingleBlob

- 消息：`Transferring multiple files into a single Blob is not supported. Currently only single file source is supported.`


### <a name="error-code--azurestorageoperationfailedconcurrentwrite"></a>错误代码：AzureStorageOperationFailedConcurrentWrite

- 消息：`Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


### <a name="invalid-property-during-copy-activity"></a>执行复制活动期间属性无效

- **消息**：`Copy activity <Activity Name> has an invalid "source" property. The source type is not compatible with the dataset <Dataset Name> and its linked service <Linked Service Name>. Please verify your input against.`

- **原因：** 数据集中定义的类型与复制活动中定义的源/接收器类型不一致。

- **解决方法**：编辑数据集或管道 JSON 定义，使类型一致，然后重新运行部署。


## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>错误消息：请求过大

- **症状**：将数据复制到使用默认写入批大小的 Azure Cosmos DB 时，遇到错误“请求过大”。

- **原因：** Cosmos DB 将单个请求的大小限制为 2 MB。 公式为请求大小 = 单个文档大小 * 写入批大小。 如果文档过大，默认行为会导致请求过大。 可以优化写入批大小。

- **解决方法**：在复制活动接收器中，减小“写入批大小”值（默认值为 10000）。

### <a name="error-message-unique-index-constraint-violation"></a>错误消息：唯一索引约束冲突

- **症状**：将数据复制到 Cosmos DB 时遇到以下错误：

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception = Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **原因：** 有两个可能的原因：

    - 如果使用“插入”作为写入行为，则此错误表示源数据包含具有相同 ID 的行/对象。

    - 如果使用“更新插入”作为写入行为，并设置了容器的另一个唯一键，则此错误表示源数据中的行/对象使用了定义的唯一键的不同 ID，但使用了该键的相同值。

- **解决方法**： 

    - 对于原因 1，请将“更新插入”设置为写入行为。
    - 对于原因 2，请确保每个文档使用定义的唯一键的不同值。

### <a name="error-message-request-rate-is-large"></a>错误消息：请求速率太大

- **症状**：将数据复制到 Cosmos DB 时遇到以下错误：

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **原因：** 使用的请求单位大于 Cosmos DB 中配置的可用 RU。 在[此处](../cosmos-db/request-units.md#request-unit-considerations)了解 Cosmos DB 如何计算 RU。

- **解决方法**：下面是两种解决方法：

    1. **增加容器 RU**，使之大于 Cosmos DB 中的值，这可以提高复制活动的性能，不过会增大 Cosmos DB 的费用。 

    2. 将 **writeBatchSize** 减至更小的值（例如 1000），并将 **parallelCopies** 设置为更小的值（例如 1），这会导致复制运行性能比当前更糟，但不会增大 Cosmos DB 的费用。

### <a name="column-missing-in-column-mapping"></a>列映射中缺少列

- **症状**：导入用于列映射的 Cosmos DB 的架构时缺少某些列。 

- **原因：** ADF 从前 10 个 Cosmos DB 文档推断架构。 如果某些列/属性在这些文档中没有值，则它们不会被 ADF 检测到，因此也就不会显示。

- **解决方法**：可按如下所示优化查询，以强制在结果集中显示带有空值的列：（假设前 10 个文档中缺少“impossible”列）。 或者，可以手动添加要映射的列。

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>错误消息：读取器的 GuidRepresentation 为 CSharpLegacy

- **症状**：从包含 UUID 字段的 Cosmos DB MongoAPI/MongoDB 复制数据时遇到以下错误：

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **原因：** 可通过两种方式表示 BSON 中的 UUID - UuidStardard 和 UuidLegacy。 默认使用 UuidLegacy 来读取数据。 如果 MongoDB 中的 UUID 数据是 UuidStandard，则会出现错误。

- **解决方法**：在 MongoDB 连接字符串中，添加选项 "**uuidRepresentation=standard**"。 有关详细信息，请参阅 [MongoDB 连接字符串](connector-mongodb.md#linked-service-properties)。
            

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="error-code--adlsgen2operationfailed"></a>错误代码：AdlsGen2OperationFailed

- 消息：`ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **原因：** ADLS Gen2 引发了指明操作失败的错误。

- **建议**：检查 ADLS Gen2 引发的详细错误消息。 如果这是由暂时性故障导致的，请重试。 如果需要进一步的帮助，请联系 Azure 存储支持，并提供错误消息中的请求 ID。

- **原因：** 如果错误消息包含“被禁止”，则你使用的服务主体或托管标识可能没有足够的权限来访问 ADLS Gen2。

- **建议**：参阅帮助文档： https://docs.azure.cn/data-factory/connector-azure-data-lake-storage#service-principal-authentication 。

- **原因：** 当错误消息包含“InternalServerError”时，错误是由 ADLS Gen2 返回的。

- **建议**：这可能是由暂时性失败导致的，请重试。 如果此问题仍然存在，请联系 Azure 存储支持部门，并提供错误消息中的请求 ID。


### <a name="error-code--adlsgen2invalidurl"></a>错误代码：AdlsGen2InvalidUrl

- 消息：`Invalid url '%url;' provided, expecting http[s]://<accountname>.dfs.core.chinacloudapi.cn.`


### <a name="error-code--adlsgen2invalidfolderpath"></a>错误代码：AdlsGen2InvalidFolderPath

- **消息**：`The folder path is not specified. Cannot locate the file '%name;' under the ADLS Gen2 account directly. Please specify the folder path instead.`


### <a name="error-code--adlsgen2operationfailedconcurrentwrite"></a>错误代码：AdlsGen2OperationFailedConcurrentWrite

- 消息：`Error occurred when trying to upload a file. It's possible because you have multiple concurrent copy activities runs writing to the same file '%name;'. Check your ADF configuration.`


### <a name="error-code-adlsgen2timeouterror"></a>错误代码：AdlsGen2TimeoutError

- **消息**：`Request to ADLS Gen2 account '%account;' met timeout error. It is mostly caused by the poor network between the Self-hosted IR machine and the ADLS Gen2 account. Check the network to resolve such error.`


### <a name="request-to-adls-gen2-account-met-timeout-error"></a>向 ADLS Gen2 帐户发出请求时出现超时错误

- **消息**：错误代码 = `UserErrorFailedBlobFSOperation`，错误消息 = `BlobFS operation failed for: A task was canceled`。

- **原因：** 此问题是由 ADLS Gen2 接收器超时错误引起的，该错误大多发生在自承载 IR 计算机上。

- **建议**： 

    1. 如果可能，请将自承载 IR 计算机和目标 ADLS Gen2 帐户置于同一区域中。 这可以避免随机超时错误，并获得更好的性能。

    1. 检查是否有任何特殊的网络设置（例如 ExpressRoute），并确保网络具有足够的带宽。 建议在总体带宽较低时降低自承载 IR 并发作业数设置，这样可以避免多个并发作业之间的网络资源争用。

    1. 如果文件大小适中或较小，对于非二进制复制，请使用较小的块大小以减轻此类超时错误。 请参阅 [Blob 存储放置块](https://docs.microsoft.com/rest/api/storageservices/put-block)。

       若要指定自定义块大小，可以在 .json 编辑器中编辑此属性：
    ```
    "sink": {
        "type": "DelimitedTextSink",
        "storeSettings": {
            "type": "AzureBlobFSWriteSettings",
            "blockSizeInMB": 8
        }
    }
    ```

## <a name="azure-synapse-analyticsazure-sql-databasesql-server"></a>Azure Synapse Analytics/Azure SQL 数据库/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>错误代码：SqlFailedToConnect

- 消息：`Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **原因：** 如果错误消息包含“SqlException”，则 SQL 数据库将引发错误，指示某个特定操作失败。

- **建议**：如需了解更多详细信息，请按 SQL 错误代码在以下参考文档中进行搜索： https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors 。 如需进一步的帮助，请联系 Azure SQL 支持。

- **原因：** 如果错误消息包含“具有 IP 地址的客户端‘...’不允许访问服务器”，并且你正在尝试连接到 Azure SQL 数据库，这通常是由 Azure SQL 数据库防火墙问题导致的。

- **建议**：在逻辑 SQL Server 防火墙配置中，启用“允许 Azure 服务和资源访问此服务器”选项。 参考文档： https://docs.azure.cn/sql-database/sql-database-firewall-configure 。


### <a name="error-code--sqloperationfailed"></a>错误代码：SqlOperationFailed

- 消息：`A database operation failed. Please search error to get more details.`

- **原因：** 如果错误消息包含“SqlException”，则 SQL 数据库将引发错误，指示某个特定操作失败。

- **建议**：如果 SQL 错误不明确，请尝试将数据库更改为最新兼容级别“150”。 它可能会引发最新版本 SQL 错误。 请参阅[详细信息文档](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level#backwardCompat)。

    若要排查 SQL 错误，请根据 SQL 错误代码在此参考文档中搜索以了解更多详细信息： https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors 。 如需进一步的帮助，请联系 Azure SQL 支持。

- **原因：** 如果错误消息包含“PdwManagedToNativeInteropException”，这通常是由于源列和接收器列大小不匹配导致的。

- **建议**：检查源列和接收器列的大小。 如需进一步的帮助，请联系 Azure SQL 支持。

- **原因：** 如果错误消息包含“InvalidOperationException”，这通常是由于输入数据无效导致的。

- **建议**：若要确定哪个行遇到了问题，请在复制活动上启用容错功能，该功能可将有问题的行重定向到存储，以便进一步调查。 参考文档： https://docs.azure.cn/data-factory/copy-activity-fault-tolerance 。



### <a name="error-code--sqlunauthorizedaccess"></a>错误代码：SqlUnauthorizedAccess

- 消息：`Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **原因：** 凭据不正确，或登录帐户无法访问 SQL 数据库。

- **建议**：检查登录帐户是否有足够的权限来访问 SQL 数据库。


### <a name="error-code--sqlopenconnectiontimeout"></a>错误代码：SqlOpenConnectionTimeout

- 消息：`Open connection to database timeout after '%timeoutValue;' seconds.`

- **原因：** 可能是 SQL 数据库暂时性失败。

- **建议**：重试，以使用更大的连接超时值来更新链接服务连接字符串。


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>错误代码：SqlAutoCreateTableTypeMapFailed

- 消息：`Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(column name:'%columnName;') in autocreate table.`

- **原因：** 自动创建表不能满足源要求。

- **建议**：更新“mappings”中的列类型，或者在目标服务器中手动创建接收器表。


### <a name="error-code--sqldatatypenotsupported"></a>错误代码：SqlDataTypeNotSupported

- 消息：`A database operation failed. Check the SQL errors.`

- **原因：** 如果在 SQL 源上出现问题，并且错误与 SqlDateTime 溢出有关，则数据值将超出逻辑类型范围 (1/1/1753 12:00:00 AM - 12/31/9999 11:59:59 PM)。

- **建议**：在源 SQL 查询中将类型转换为字符串，或在复制活动列映射中将列类型更改为“String”。

- **原因：** 如果在 SQL 接收器上出现问题，并且错误与 SqlDateTime 溢出有关，则数据值将超出接收器表中的允许范围。

- **建议**：在接收器表中将相应的列类型更新为“datetime2”类型。


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>错误代码：SqlInvalidDbStoredProcedure

- 消息：`The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **原因：** 指定的存储过程无效。 这可能是因为存储过程不返回任何数据。

- **建议**：通过 SQL 工具验证存储过程。 请确保存储过程可以返回数据。


### <a name="error-code--sqlinvaliddbquerystring"></a>错误代码：SqlInvalidDbQueryString

- 消息：`The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **原因：** 指定的 SQL 查询无效。 这可能是因为查询不返回任何数据。

- **建议**：通过 SQL 工具验证 SQL 查询。 请确保查询可以返回数据。


### <a name="error-code--sqlinvalidcolumnname"></a>错误代码：SqlInvalidColumnName

- 消息：`Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **原因：** 找不到列。 可能存在配置错误。

- **建议**：验证查询中的列、数据集中的“结构”和活动中的“映射”。


### <a name="error-code--sqlcolumnnamemismatchbycasesensitive"></a>错误代码：SqlColumnNameMismatchByCaseSensitive

- 消息：`Column '%column;' in DataSet '%dataSetName;' cannot be found in physical SQL Database. Column matching is case-sensitive. Column '%columnInTable;' appears similar. Check the DataSet(s) configuration to proceed further.`


### <a name="error-code--sqlbatchwritetimeout"></a>错误代码：SqlBatchWriteTimeout

- 消息：`Timeouts in SQL write operation.`

- **原因：** 可能是 SQL 数据库暂时性失败。

- **建议**：重试。 如果问题重现，请联系 Azure SQL 支持人员。


### <a name="error-code--sqlbatchwritetransactionfailed"></a>错误代码：SqlBatchWriteTransactionFailed

- 消息：`SQL transaction commits failed`

- **原因：** 如果异常详细信息持续指出事务超时，则表示集成运行时与数据库之间的网络延迟高于默认阈值（30 秒）。

- **建议**：使用 120 或更大的“连接超时”值更新 SQL 链接服务连接字符串更新，然后重新运行活动。

- **原因：** 如果异常详细信息间歇性指出 sqlconnection 中断，则原因可能只是与暂时性网络故障或 SQL 数据库端的问题有关

- **建议**：重试活动，并审阅 SQL 数据库端指标。


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>错误代码：SqlBulkCopyInvalidColumnLength

- 消息：`SQL Bulk Copy failed due to receive an invalid column length from the bcp client.`

- **原因：** 由于从 BCP 客户端接收到无效的列长度，SQL 批量复制失败。

- **建议**：若要确定哪个行遇到了问题，请在复制活动上启用容错功能，该功能可将有问题的行重定向到存储，以便进一步调查。 参考文档： https://docs.azure.cn/data-factory/copy-activity-fault-tolerance 。


### <a name="error-code--sqlconnectionisclosed"></a>错误代码：SqlConnectionIsClosed

- 消息：`The connection is closed by SQL Database.`

- **原因：** 当高并发运行和服务器终止连接时，SQL 数据库关闭了 SQL 连接。

- **建议**：远程服务器关闭了 SQL 连接。 Retry。 如果问题重现，请联系 Azure SQL 支持人员。


### <a name="error-code--sqlcreatetablefailedunsupportedtype"></a>错误代码：SqlCreateTableFailedUnsupportedType

- 消息：`Type '%type;' in source side cannot be mapped to a type that supported by sink side(column name:'%name;') in autocreate table.`


### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>错误消息：将字符串转换为唯一标识符失败

- **症状**：使用暂存复制和 PolyBase 将表格数据源（例如 SQL Server）中的数据复制到 Azure Synapse Analytics 时，遇到以下错误：

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **原因：** Azure Synapse Analytics PolyBase 无法将空字符串转换为 GUID。

- **解决方法**：在复制活动接收器中的 Polybase 设置下，将“use type default”选项设置为 false。


### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>错误消息：预期的数据类型：DECIMAL(x,x)，违规值

- **症状**：使用暂存复制和 PolyBase 将表格数据源（例如 SQL Server）中的数据复制到 Azure Synapse Analytics 时，遇到以下错误：

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **原因：** Azure Synapse Analytics Polybase 无法将空字符串（null 值）插入十进制列。

- **解决方法**：在复制活动接收器中的 Polybase 设置下，将“use type default”选项设置为 false。


### <a name="error-message-java-exception-message-hdfsbridgecreaterecordreader"></a>错误消息：Java 异常消息：HdfsBridge::CreateRecordReader

- **症状**：使用 PolyBase 将数据复制到 Azure Synapse Analytics 时遇到以下错误：

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **原因：** 可能的原因是架构（总列宽）太大（大于 1 MB）。 通过添加所有列的大小来检查目标 Azure Synapse Analytics 表的架构：

    - Int -> 4 字节
    - Bigint -> 8 字节
    - Varchar(n),char(n),binary(n), varbinary(n) -> n 字节
    - Nvarchar(n), nchar(n) -> n*2 字节
    - Date -> 6 字节
    - Datetime/(2), smalldatetime -> 16 字节
    - Datetimeoffset -> 20 字节
    - Decimal -> 19 字节
    - Float -> 8 字节
    - Money -> 8 字节
    - Smallmoney -> 4 字节
    - Real -> 4 字节
    - Smallint -> 2 字节
    - Time -> 12 字节
    - Tinyint -> 1 字节

- **解决方法**：将列宽缩小至 1 MB 以下

- 或者，通过禁用 Polybase 来使用批量插入方法


### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>错误消息：不满足使用 HTTP 条件标头指定的条件

- **症状**：使用 SQL 查询从 Azure Synapse Analytics 提取数据时遇到以下错误：

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **原因：** Azure Synapse Analytics 在查询 Azure 存储中的外部表时遇到问题。

- **解决方法**：在 SSMS 中运行同一查询，检查是否看到相同的结果。 如果是，请创建 Azure Synapse Analytics 支持票证，并提供 Azure Synapse Analytics 服务器和数据库名称以进一步排查问题。
            

### <a name="low-performance-when-load-data-into-azure-sql"></a>将数据加载到 Azure SQL 时性能较低

- **症状**：将数据复制到 Azure SQL 时速度变慢。

- **原因：** 此问题就根本原因来说主要由 Azure SQL 端的瓶颈触发。 下面是一些可能的原因：

    1. Azure DB 层的等级不够高。

    1. Azure DB DTU 使用率接近 100%。 你可以[监视性能](/azure-sql/database/monitor-tune-overview)并考虑升级 DB 层。

    1. 未正确设置索引。 请在加载数据之前删除所有索引，并在加载完成后重新创建它们。

    1. WriteBatchSize 不够大，无法容纳架构行大小。 若要解决此问题，请尝试增大此属性。

    1. 使用的是存储过程，而不是批量插入，这会使性能更差。 

- **解决方法**：请参阅 TSG 来了解 [复制活动性能](/data-factory/copy-activity-performance-troubleshooting)


### <a name="performance-tier-is-low-and-leads-to-copy-failure"></a>性能层等级较低，导致复制失败

- **症状**：将数据复制到 Azure SQL 时出现以下错误消息：`Database operation failed. Error message from database execution : ExecuteNonQuery requires an open and available Connection. The connection's current state is closed.`

- **原因：** 正在使用 Azure SQL s1，在此情况下达到了 IO 限制。

- **解决方法**：请升级 Azure SQL 性能层以解决此问题。 


### <a name="sql-table-cannot-be-found"></a>找不到 SQL 表 

- **症状**：将数据从混合结构复制到本地 SQL Server 表时出错：`Cannot find the object "dbo.Contoso" because it does not exist or you do not have permissions.`

- **原因：** 当前 SQL 帐户的权限不足，无法执行由 .NET SqlBulkCopy.WriteToServer 发出的请求。

- **解决方法**：请切换到权限更高的一个 SQL 帐户。


### <a name="string-or-binary-data-would-be-truncated"></a>字符串或二进制数据会被截断

- **症状**：将数据复制到本地/Azure SQL Server 表时出错： 

- **原因：** Cx Sql 表架构定义具有一个或多个长度小于预期的列。

- **解决方法**：请尝试以下步骤来解决问题：

    1. 应用[容错](/data-factory/copy-activity-fault-tolerance)（尤其是“redirectIncompatibleRowSettings”）来排查哪些行有问题。

    1. 请仔细将重定向的数据与 SQL 表架构列长度进行核对，以查明哪些列需要更新。

    1. 相应地更新表架构。


## <a name="delimited-text-format"></a>带分隔符的文本格式

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>错误代码：DelimitedTextColumnNameNotAllowNull

- 消息：`The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **原因：** 在活动中设置“firstRowAsHeader”时，第一行将用作列名。 此错误表示第一行包含空值。 例如：“ColumnA, ColumnB”。

- **建议**：检查第一行，如果存在空值，请修复值。


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>错误代码：DelimitedTextMoreColumnsThanDefined

- 消息：`Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %columnCount;.`

- **原因：** 有问题的行的列计数大于第一行的列计数。 原因可能是数据有问题，或者列分隔符/引号字符设置不正确。

- **建议**：获取错误消息中的行计数，检查行的列并修复数据。

- **原因：** 如果错误消息中的预期列计数为“1”，则你可能指定了错误的压缩或格式设置。 因此，ADF 错误地分析了你的文件。

- **建议**：检查格式设置，确保它与源文件相匹配。

- **原因：** 如果源是文件夹，则原因可能是指定的文件夹中的文件采用了不同的架构。

- **建议**：确保给定文件夹中的文件采用相同的架构。


### <a name="error-code--delimitedtextincorrectrowdelimiter"></a>错误代码：DelimitedTextIncorrectRowDelimiter

- 消息：`The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--delimitedtexttoolargecolumncount"></a>错误代码：DelimitedTextTooLargeColumnCount

- 消息：`Column count reaches limitation when deserializing csv file. Maximum size is '%size;'. Check the column delimiter and row delimiter provided. (Column delimiter: '%columnDelimiter;', Row delimiter: '%rowDelimiter;')`


### <a name="error-code--delimitedtextinvalidsettings"></a>错误代码：DelimitedTextInvalidSettings

- 消息：`%settingIssues;`



## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365/Common Data Service/Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>错误代码：DynamicsCreateServiceClientError

- 消息：`This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **原因：** Dynamics 服务器端出现了暂时性问题。

- **建议**：重新运行管道。 如果仍旧失败，请尝试降低并行度。 如果还是失败，请联系 Dynamics 支持人员。


### <a name="columns-are-missing-when-previewingimporting-schema"></a>预览/导入架构时缺少列

- **症状**：导入架构或预览数据时，缺少某些列。 错误消息：`The valid structure information (column name and type) are required for Dynamics source.`

- **原因：** 此问题基本上是设计使然，因为 ADF 无法显示前 10 条记录中没有值的列。 请确保你添加的列采用正确格式。 

- **建议**：在映射选项卡中手动添加列。


## <a name="excel-format"></a>Excel 格式

### <a name="timeout-or-slow-performance-when-parsing-large-excel-file"></a>分析大型 Excel 文件时超时或性能较低

- **症状**：

    1. 创建 Excel 数据集并从连接/存储导入架构、预览数据、列出或刷新工作表时，如果 Excel 文件很大，则可能会出现超时错误。

    1. 使用复制活动将大型 Excel 文件 (>= 100MB) 中的数据复制到其他数据存储时，可能会遇到性能下降或 OOM 问题。

- **原因**： 

    1. 对于导入架构、预览数据以及在 Excel 数据集上列出工作表等操作，超时为 100 秒并且是静态的。 对于大型 Excel 文件，这些操作可能无法在超时值内完成。

    2. ADF 复制活动将整个 Excel 文件读入内存，然后查找指定的工作表和单元格来读取数据。 此行为是由 ADF 使用的基础 SDK 导致的。

- **解决方法**： 

    1. 若要导入架构，你可以生成一个是原始文件子集的较小示例文件，并选择“从示例文件导入架构”而不是“从连接/存储导入架构”。

    2. 若要列出工作表，可以改为在工作表下拉框中单击“编辑”并输入工作表名称/索引。

    3. 若要将大型 Excel 文件 (>100MB) 复制到其他存储，可以使用支持流式读取且性能更好的数据流 Excel 源。


## <a name="hdinsight"></a>HDInsight

### <a name="ssl-error-when-adf-linked-service-using-hdinsight-esp-cluster"></a>ADF 链接服务使用 HDInsight ESP 群集时出现 SSL 错误

- 消息：`Failed to connect to HDInsight cluster: 'ERROR [HY000] [Microsoft][DriverSupport] (1100) SSL certificate verification failed because the certificate is missing or incorrect.`

- **原因：** 此问题很可能与系统信任存储相关。

- **解决方法**：你可以导航到 Microsoft Integration Runtime\4.0\Shared\ODBC Drivers\Microsoft Hive ODBC Driver\lib 路径，并打开 DriverConfiguration64.exe 以更改设置。

    ![取消选中“使用系统信任存储”](./media/connector-troubleshoot-guide/system-trust-store-setting.png)


## <a name="json-format"></a>JSON 格式

### <a name="error-code--jsoninvalidarraypathdefinition"></a>错误代码：JsonInvalidArrayPathDefinition

- 消息：`Error occurred when deserializing source JSON data. Check whether the JsonPath in JsonNodeReference and JsonPathDefintion is valid.`


### <a name="error-code--jsonemptyjobjectdata"></a>错误代码：JsonEmptyJObjectData

- 消息：`The specified row delimiter %rowDelimiter; is incorrect. Cannot detect a row after parse %size; MB data.`


### <a name="error-code--jsonnullvalueinpathdefinition"></a>错误代码：JsonNullValueInPathDefinition

- 消息：`Null JSONPath detected in JsonPathDefinition.`


### <a name="error-code--jsonunsupportedhierarchicalcomplexvalue"></a>错误代码：JsonUnsupportedHierarchicalComplexValue

- 消息：`The retrieved type of data %data; with value %value; is not supported yet. Please either remove the targeted column '%name;' or enable skip incompatible row to skip the issue rows.`


### <a name="error-code--jsonconflictpartitiondiscoveryschema"></a>错误代码：JsonConflictPartitionDiscoverySchema

- 消息：`Conflicting partition column names detected.'%schema;', '%partitionDiscoverySchema;'`


### <a name="error-code--jsoninvaliddataformat"></a>错误代码：JsonInvalidDataFormat

- 消息：`Error occurred when deserializing source JSON file '%fileName;'. Check if the data is in valid JSON object format.`


### <a name="error-code--jsoninvaliddatamixedarrayandobject"></a>错误代码：JsonInvalidDataMixedArrayAndObject

- 消息：`Error occurred when deserializing source JSON file '%fileName;'. The JSON format doesn't allow mixed arrays and objects.`


## <a name="oracle"></a>Oracle

### <a name="error-code-argumentoutofrangeexception"></a>错误代码：ArgumentOutOfRangeException

- 消息：`Hour, Minute, and Second parameters describe an un-representable DateTime.`

- **原因：** ADF 支持 0001-01-01 00:00:00 到 9999-12-31 23:59:59 范围内的 DateTime 值。 但是，Oracle 支持范围更广的 DateTime 值（例如公元前世纪或大于 59 的分钟/秒），这在 ADF 中会导致失败。

- **建议**： 

    请运行 `select dump(<column name>)` 以检查 Oracle 中的值是否在 ADF 的范围内。 

    如果你希望知道结果中的字节序列，请查看 https://stackoverflow.com/questions/13568193/how-are-dates-stored-in-oracle 。


## <a name="parquet-format"></a>Parquet 格式

### <a name="error-code--parquetjavainvocationexception"></a>错误代码：ParquetJavaInvocationException

- 消息：`An error occurred when invoking java, message: %javaException;.`

- **原因：** 如果错误消息包含“java.lang.OutOfMemory”、“Java 堆空间”和“doubleCapacity”，则原因通常与旧版集成运行时中的内存管理问题有关。

- **建议**：如果使用的是自承载集成运行时，且版本低于 3.20.7159.1，建议升级到最新版本。

- **原因：** 如果错误消息包含“java.lang.OutOfMemory”，则原因是集成运行时不能提供足够的资源来处理文件。

- **建议**：限制集成运行时中的并发运行。 对于自承载集成运行时，请扩展到具有 8 GB 或更大内存的强大计算机。

- **原因：** 如果错误消息包含“NullPointerReference”，则原因可能是出现了暂时性错误。

- **建议**：重试。 如果问题仍然出现，请联系支持人员。


### <a name="error-code--parquetinvalidfile"></a>错误代码：ParquetInvalidFile

- 消息：`File is not a valid Parquet file.`

- **原因：** Parquet 文件问题。

- **建议**：检查输入是否为有效的 Parquet 文件。


### <a name="error-code--parquetnotsupportedtype"></a>错误代码：ParquetNotSupportedType

- 消息：`Unsupported Parquet type. PrimitiveType: %primitiveType; OriginalType: %originalType;.`

- **原因：** Azure 数据工厂不支持 Parquet 格式。

- **建议**：反复检查源数据。 参阅文档： https://docs.azure.cn/data-factory/supported-file-formats-and-compression-codecs 。


### <a name="error-code--parquetmisseddecimalprecisionscale"></a>错误代码：ParquetMissedDecimalPrecisionScale

- 消息：`Decimal Precision or Scale information is not found in schema for column: %column;.`

- **原因：** 尝试分析数字的精度和小数位数，但系统未提供此类信息。

- **建议**：“Source”不会返回正确的精度和小数位数。 检查问题列的精度和小数位数。


### <a name="error-code--parquetinvaliddecimalprecisionscale"></a>错误代码：ParquetInvalidDecimalPrecisionScale

- 消息：`Invalid Decimal Precision or Scale. Precision: %precision; Scale: %scale;.`

- **原因：** 架构无效。

- **建议**：检查问题列的精度和小数位数。


### <a name="error-code--parquetcolumnnotfound"></a>错误代码：ParquetColumnNotFound

- 消息：`Column %column; does not exist in Parquet file.`

- **原因：** 源架构与接收器架构不匹配。

- **建议**：检查“activity”中的“mappings”。 确保源列可映射到正确的接收器列。


### <a name="error-code--parquetinvaliddataformat"></a>错误代码：ParquetInvalidDataFormat

- 消息：`Incorrect format of %srcValue; for converting to %dstType;.`

- **原因：** 数据无法转换为 mappings.source 中指定的类型

- **建议**：反复检查源数据，或者在复制活动列映射中指定此列的正确数据类型。 参阅文档： https://docs.azure.cn/data-factory/supported-file-formats-and-compression-codecs 。


### <a name="error-code--parquetdatacountnotmatchcolumncount"></a>错误代码：ParquetDataCountNotMatchColumnCount

- 消息：`The data count in a row '%sourceColumnCount;' does not match the column count '%sinkColumnCount;' in given schema.`

- **原因：** 源列计数和接收器列计数不匹配

- **建议**：反复检查源列计数是否与“mapping”中的接收器列计数相同。


### <a name="error-code--parquetdatatypenotmatchcolumntype"></a>错误代码：ParquetDataTypeNotMatchColumnType

- **消息**：数据类型 %srcType; 与列“% columnIndex;”的给定列类型 %dstType; 不匹配。

- **原因：** 源中的数据无法转换为接收器中定义的类型

- **建议**：在 mapping.sink 中指定正确的类型。


### <a name="error-code--parquetbridgeinvaliddata"></a>错误代码：ParquetBridgeInvalidData

- 消息：`%message;`

- **原因：** 数据值超出限制

- **建议**：重试。 如果问题仍然出现，请联系我们。


### <a name="error-code--parquetunsupportedinterpretation"></a>错误代码：ParquetUnsupportedInterpretation

- 消息：`The given interpretation '%interpretation;' of Parquet format is not supported.`

- **原因：** 不支持的方案

- **建议**：“ParquetInterpretFor”不应为“sparkSql”。


### <a name="error-code--parquetunsupportfilelevelcompressionoption"></a>错误代码：ParquetUnsupportFileLevelCompressionOption

- 消息：`File level compression is not supported for Parquet.`

- **原因：** 不支持的方案

- **建议**：删除有效负载中的“CompressionType”。


### <a name="error-code--usererrorjniexception"></a>错误代码：UserErrorJniException

- 消息：`Cannot create JVM: JNI return code [-6][JNI call failed: Invalid arguments.]`

- **原因：** 由于设置了一些非法（全局）参数，因此无法创建 JVM。

- **建议**：登录到承载着你的自承载 IR 的每个节点的计算机。 检查是否正确设置了系统变量，如下所示：`_JAVA_OPTIONS "-Xms256m -Xmx16g" with memory bigger than 8 G`。 重启所有 IR 节点，然后重新运行该管道。


### <a name="arithmetic-overflow"></a>算术溢出

- **症状**：复制 Parquet 文件时出现错误消息：`Message = Arithmetic Overflow., Source = Microsoft.DataTransfer.Common`

- **原因：** 将文件从 Oracle 复制到 Parquet 时，当前仅支持精度 <= 38 且整数部分的长度 <= 20 的十进制数。 

- **解决方法**：你可以将存在此类问题的列转换为 VARCHAR2，这是一种解决方法。


### <a name="no-enum-constant"></a>无枚举常量

- **症状**：将数据复制为 Parquet 格式时出现以下错误消息：`java.lang.IllegalArgumentException:field ended by &apos;;&apos;` 或 `java.lang.IllegalArgumentException:No enum constant org.apache.parquet.schema.OriginalType.test`。

- **原因**： 

    此问题可能是由列名中的空格或不受支持的字符（例如 ,;{}()\n\t=）导致的，因为 Parquet 不支持此类格式。 

    例如，contoso(test) 之类的列名会分析[代码](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/MessageTypeParser.java) `Tokenizer st = new Tokenizer(schemaString, " ;{}()\n\t");` 中的括号中的类型。 将引发错误，因为不存在这样的“test”类型。

    若要查看支持的类型，可以在[此处](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/OriginalType.java)查看它们。

- **解决方法**： 

    1. 仔细检查接收器列名中是否有空格。

    1. 仔细检查是否将包含空格的第一行用作了列名。

    1. 仔细检查是否支持类型 OriginalType。 尽量避免使用这些特殊符号：`,;{}()\n\t=`。 


## <a name="rest"></a>REST

### <a name="unexpected-network-response-from-rest-connector"></a>来自 REST 连接器的意外网络响应

- **症状**：终结点有时从 REST 连接器收到意外响应 (400/401/403/500)。

- **原因：** 构造 HTTP 请求时，REST 源连接器将来自链接服务/数据集/复制源的 URL 和 HTTP 方法/标头/正文用作参数。 此问题很可能是由一个或多个指定参数中的某些错误引起的。

- **解决方法**： 
    - 在 cmd 窗口中使用“curl”检查参数是否是问题的原因（Accept 和 User-Agent 标头应始终包括在内 ）：
        ```
        curl -i -X <HTTP method> -H <HTTP header1> -H <HTTP header2> -H "Accept: application/json" -H "User-Agent: azure-data-factory/2.0" -d '<HTTP body>' <URL>
        ```
      如果命令返回相同的意外响应，请使用“curl”修复以上参数，直到返回预期的响应。 

      也可以使用“curl --help”来更高级地使用命令。

    - 如果只有 ADF REST 连接器返回意外响应，请联系 Azure 支持人员，以进一步进行故障排除。
    
    - 请注意，“curl”可能不适合重现 SSL 证书验证问题。 在某些情况下，“curl”命令已成功执行，但没有遇到任何 SSL 证书验证问题。 但是，当在浏览器中执行相同的 URL 时，客户端实际上不会首先返回任何 SSL 证书来建立与服务器的信任。

      对于上述情况，建议使用 Postman 和 Fiddler 之类的工具 。


## <a name="sftp"></a>SFTP

### <a name="invalid-sftp-credential-provided-for-sshpublickey-authentication-type"></a>为“SSHPublicKey”身份验证类型提供的 SFTP 凭据无效

- **症状**：使用了 SSH 公钥身份验证，但为“'SshPublicKey”身份验证类型提供了无效的 SFTP 凭据。

- **原因：** 此错误可能是由以下三个可能的原因导致的：

    1. 从 AKV/SDK 中提取了私钥内容，但该内容未正确编码。

    1. 选择了错误的密钥内容格式。

    1. 凭据或私钥内容无效。

- **解决方法**： 

    1. 对于原因 1：

       如果私钥内容来自 AKV，并且客户将原始密钥文件直接上传到 SFTP 链接服务，则原始密钥文件可以正常使用。

       请参阅 https://docs.azure.cn/data-factory/connector-sftp#using-ssh-public-key-authentication 。privateKey 内容是 Base64 编码的 SSH 私钥内容。

       请使用 Base64 编码将原始私钥文件的整个内容编码，并将编码后的字符串存储到 AKV 中。 如果单击“从文件上传”，则原始私钥文件是可以在 SFTP 链接服务上使用的文件。

       下面是用于生成字符串的一些示例：

       - 使用 C# 代码：
       ```
       byte[] keyContentBytes = File.ReadAllBytes(Private Key Path);
       string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
       ```

       - 使用 Python 代码：
       ```
       import base64
       rfd = open(r'{Private Key Path}', 'rb')
       keyContent = rfd.read()
       rfd.close()
       print base64.b64encode(Key Content)
       ```

       - 使用第三方 Base64 转换工具

         建议使用 https://www.base64encode.org/ 之类的工具。

    1. 对于原因 2：

       如果使用的是 PKCS#8 格式的 SSH 私钥

       当前不支持使用 PKCS#8 格式的 SSH 私钥（开头为“-----BEGIN ENCRYPTED PRIVATE KEY-----”）访问 ADF 中的 SFTP 服务器。 

       请运行以下命令，将密钥转换为传统的 SSH 密钥格式（开头为“-----BEGIN RSA PRIVATE KEY-----”）：

       ```
       openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file
       chmod 600 traditional_format_key_file
       ssh-keygen -f traditional_format_key_file -p
       ```
    1. 对于原因 3：

       请使用 WinSCP 之类的工具仔细检查，看密钥文件或密码是否正确。


### <a name="incorrect-linked-service-type-is-used"></a>使用了错误的链接服务类型

- **症状**：无法访问 FTP/SFTP 服务器。

- **原因：** 为 FTP 或 SFTP 服务器使用了错误的链接服务类型，例如，使用 FTP 链接服务连接到 SFTP 服务器或进行反向连接。

- **解决方法**：请检查目标服务器的端口。 默认情况下，FTP 使用端口 21，SFTP 使用端口 22。


### <a name="sftp-copy-activity-failed"></a>SFTP 复制活动失败

- **症状**：错误代码：UserErrorInvalidColumnMappingColumnNotFound。 错误消息：`Column &apos;AccMngr&apos; specified in column mapping cannot be found in source data.`

- **原因：** 源不包含名为“AccMngr”的列。

- **解决方法**：通过映射目标数据集列来仔细检查你的数据集的配置情况，以确认是否存在这样的“AccMngr”列。


### <a name="sftp-server-connection-throttling"></a>SFTP 服务器连接限制

- **症状**：服务器响应不包含 SSH 协议标识，并且无法复制。

- **原因：** ADF 会创建多个连接，以从 SFTP 服务器进行并行下载，有时会达到 SFTP 服务器限制。 实际上，不同的服务器在达到限制时会返回不同的错误。

- **解决方法**： 

    请将 SFTP 数据集的最大并发连接数指定为 1，然后重新运行复制。 如果成功，则可确定这是由限制导致的。

    若要提升吞吐量，请联系 SFTP 管理员以提高并发连接计数限制，或将下面的 IP 添加到允许列表：

    - 如果你使用的是托管 IR，请添加 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=57062)。
      或者，如果不想向 SFTP 服务器允许列表中添加大型的 IP 范围列表，则可以安装自承载 IR。

    - 如果使用自承载 IR，请将安装了 SHIR 的计算机 IP 添加到允许列表。


### <a name="error-code-sftprenameoperationfail"></a>错误代码：SftpRenameOperationFail

- **症状**：管道无法将数据从 Blob 复制到 SFTP，出现以下错误：`Operation on target Copy_5xe failed: Failure happened on 'Sink' side. ErrorCode=SftpRenameOperationFail,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException`。

- **原因：** 在复制数据时，选项 useTempFileRename 已设置为 True。 这允许进程使用临时文件。 如果在复制整个数据之前删除了一个或多个临时文件，则会触发此错误。

- **解决方法**：将 useTempFileName 选项设置为 False。


## <a name="general-copy-activity-error"></a>常规复制活动错误

### <a name="error-code--jrenotfound"></a>错误代码：JreNotFound

- 消息：`Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **原因：** 自承载集成运行时找不到 Java 运行时。 读取特定的源时需要 Java 运行时。

- **建议**：检查集成运行时环境，并参阅文档： https://docs.azure.cn/data-factory/format-parquet#using-self-hosted-integration-runtime


### <a name="error-code--wildcardpathsinknotsupported"></a>错误代码：WildcardPathSinkNotSupported

- 消息：`Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **原因：** 接收器数据集不支持通配符。

- **建议**：检查接收器数据集，并修复路径（不使用通配符值）。


### <a name="error-code--mappinginvalidpropertywithemptyvalue"></a>错误代码：MappingInvalidPropertyWithEmptyValue

- 消息：`One or more '%sourceOrSink;' in copy activity mapping doesn't point to any data. Choose one of the three properties 'name', 'path' and 'ordinal' to reference columns/fields.`


### <a name="error-code--mappinginvalidpropertywithnamepathandordinal"></a>错误代码：MappingInvalidPropertyWithNamePathAndOrdinal

- 消息：`Mixed properties are used to reference '%sourceOrSink;' columns/fields in copy activity mapping. Please only choose one of the three properties 'name', 'path' and 'ordinal'. The problematic mapping setting is 'name': '%name;', 'path': '%path;','ordinal': '%ordinal;'.`


### <a name="error-code--mappingduplicatedordinal"></a>错误代码：MappingDuplicatedOrdinal

- 消息：`Copy activity 'mappings' has duplicated ordinal value "%Ordinal;". Fix the setting in 'mappings'.`


### <a name="error-code--mappinginvalidordinalforsinkcolumn"></a>错误代码：MappingInvalidOrdinalForSinkColumn

- 消息：`Invalid 'ordinal' property for sink column under 'mappings' property. Ordinal: %Ordinal;.`

### <a name="fips-issue"></a>FIPS 问题

- **症状**：复制活动在启用了 FIPS 的自承载 Integration Runtime 计算机上失败，并出现以下错误消息：`This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.`。 使用 Azure Blob、SFTP 等连接器复制数据时可能会发生此问题。

- **原因：** FIPS（Federal Information Processing Standards，美国联邦信息处理标准）定义了允许使用的一组特定加密算法。 当计算机上启用了 FIPS 模式时，某些情况下会阻止复制活动所依赖的某些加密类。

- **解决方法**：你可以通过 [此文](https://techcommunity.microsoft.com/t5/microsoft-security-baselines/why-we-8217-re-not-recommending-8220-fips-mode-8221-anymore/ba-p/701037)了解 Windows 中 FIPS 模式的当前情况，并评估是否可以在自承载 Integration Runtime 计算机上禁用 FIPS。

    另一方面，如果你只是想让 Azure 数据工厂绕过 FIPS 并使活动运行成功，则可执行以下步骤：

    1. 打开安装了自承载 Integration Runtime 的文件夹（通常在 `C:\Program Files\Microsoft Integration Runtime\<IR version>\Shared` 下）。

    2. 打开“diawp.exe.config”，将 `<enforceFIPSPolicy enabled="false"/>` 添加到 `<runtime>` 节，如下所示。

        ![禁用 FIPS](./media/connector-troubleshoot-guide/disable-fips-policy.png)

    3. 重启自承载 Integration Runtime 计算机。

## <a name="next-steps"></a>后续步骤

尝试通过以下资源获得故障排除方面的更多帮助：

*  [MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)
