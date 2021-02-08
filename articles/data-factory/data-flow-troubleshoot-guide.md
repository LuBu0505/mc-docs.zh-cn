---
title: 排查映射数据流问题
description: 了解如何排查 Azure 数据工厂中的数据流问题。
services: data-factory
ms.author: v-jay
author: WenJason
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 09/11/2020
ms.date: 02/01/2021
ms.openlocfilehash: fb17a310e066efb8a7d10d2172d64a697a2d0c5f
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060951"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>排查 Azure 数据工厂中的映射数据流问题

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

本文探讨了排查 Azure 数据工厂中的映射数据流问题的常见方法。

## <a name="common-errors-and-messages"></a>常见错误和消息

### <a name="error-code-df-executor-sourceinvalidpayload"></a>错误代码:DF-Executor-SourceInvalidPayload
- **消息**：数据预览、调试和管道数据流执行失败，因为容器不存在
- 原因：当数据集包含存储中不存在的容器时
- **建议**：请确保数据集中引用的容器存在或可访问。

### <a name="error-code-df-executor-systemimplicitcartesian"></a>错误代码:DF-Executor-SystemImplicitCartesian

- **消息**：用于 INNER 联接的隐式笛卡尔乘积不受支持。请改用 CROSS JOIN。 联接中使用的列应为行创建唯一键。
- 原因：用于逻辑计划之间 INNER 联接的隐式笛卡尔乘积不受支持。 如果联接中使用的列创建了唯一键，则需要来自关系双方的至少一列。
- **建议**：对于基于不相等的联接，必须选择使用 CUSTOM CROSS JOIN。

### <a name="error-code-df-executor-systeminvalidjson"></a>错误代码:DF-Executor-SystemInvalidJson

- **消息**：JSON 分析错误、编码不受支持或多行
- 原因：JSON 文件可能存在的问题：编码不受支持、字节损坏或在多个嵌套行上使用 JSON 源作为单文档
- **建议**：请验证 JSON 文件的编码是否受支持。 在使用 JSON 数据集的源转换上，展开“JSON 设置”，然后启用“单文档”。
 
### <a name="error-code-df-executor-broadcasttimeout"></a>错误代码:DF-Executor-BroadcastTimeout

- **消息**：广播联接超时错误。请确保在调试运行中广播流在 60 秒内生成数据，在作业运行中广播流在 300 秒内生成数据
- 原因：在调试运行中，广播的默认超时为 60 秒；在作业运行中，广播的默认超时为 300 秒。 为广播选择的流似乎太大，无法在此限制内生成数据。
- **建议**：请检查数据流转换的“优化”选项卡，以进行 Join、Exists 和 Lookup。 广播的默认选项为“自动”。 如果设置为“自动”，或你要手动将左侧或右侧设置为“固定”下的广播，则可以设置较大的 Azure Integration Runtime 配置，或关闭广播。 为了获得最佳的数据流性能，推荐的方法是允许 Spark 使用“自动”进行广播，并使用内存优化的 Azure IR。

如果要从调试管道运行中的调试测试执行中执行数据流，则可能会更频繁地遇到这种情况。 这是因为为了维持更快的调试体验，ADF 将广播超时限制为 60 秒。 如果要将该时间从触发的运行延长到 300 秒超时，则可以使用“调试”>“使用活动运行时”选项来利用执行数据流管道活动中定义的 Azure IR。

### <a name="error-code-df-executor-conversion"></a>错误代码:DF-Executor-Conversion

- **消息**：无法转换为日期或时间，因为字符无效
- 原因：数据未采用预期格式
- **建议**：请使用正确的数据类型

### <a name="error-code-df-executor-invalidcolumn"></a>错误代码:DF-Executor-InvalidColumn

- **消息**：需要在查询中指定列名称。如果使用的是 SQL 函数，请设置别名
- 原因：未指定列名称
- **建议**：如果使用的是 min()/max() 等 SQL 函数，请设置别名。

 ### <a name="error-code-df-executor-drivererror"></a>错误代码：DF-Executor-DriverError
- **消息**：INT96 是旧式时间戳类型，ADF Dataflow 不支持此类型。 请考虑将列类型升级到最新类型。
- 原因：驱动程序错误
- **建议**：INT96 是旧式时间戳类型，ADF Dataflow 不支持此类型。 请考虑将列类型升级到最新类型。

 ### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>错误代码：DF-Executor-BlockCountExceedsLimitError
- **消息**：未提交的块数不能超过上限值（100,000 个块）。 检查 blob 配置。
- 原因：一个 blob 中最多可以包含 100,000 个未提交的块。
- **建议**：请与 Microsoft 产品团队联系，以获取有关此问题的详细信息

 ### <a name="error-code-df-executor-partitiondirectoryerror"></a>错误代码：DF-Executor-PartitionDirectoryError
- **消息**：指定的源路径要么包含多个分区目录（例如 <Source Path>/<Partition Root Directory 1>/a=10/b=20、<Source Path>/<Partition Root Directory 2>/c=10/d=30）或具有其他文件的分区目录，要么包含非分区目录（例如 <Source Path>/<Partition Root Directory 1>/a=10/b=20, <Source Path>/Directory 2/file1），从源路径中删除分区根目录并通过单独的源转换读取该目录。
- 原因：源路径要么包含多个分区目录或具有其他文件的分区目录，要么包含非分区目录。
- **建议**：从源路径中删除分区根目录，并通过单独的源转换读取该目录。

 ### <a name="error-code-df-executor-outofmemoryerror"></a>错误代码：DF-Executor-OutOfMemoryError
- **消息**：群集在执行期间遇到内存不足问题，请使用具有更大核心数和/或内存优化计算类型的集成运行时重试
- 原因：群集内存不足
- **建议**：调试群集仅用于开发目的。 利用数据采样、适当的计算类型和大小来运行有效负载。 若要进行优化以实现最佳性能，请参阅[映射数据流性能指南](concepts-data-flow-performance.md)。

 ### <a name="error-code-df-executor-illegalargument"></a>错误代码：DF-Executor-illegalArgument
- **消息**：请确保链接服务中的访问密钥正确
- 原因：帐户名或访问密钥不正确
- **建议**：请确保链接服务中指定的帐户名或访问密钥正确。 

 ### <a name="error-code-df-executor-invalidtype"></a>错误代码：DF-Executor-InvalidType
- **消息**：请确保参数类型与传入的值类型匹配。 当前不支持从管道传递 float 参数。
- 原因：声明的类型和实际参数值之间的数据类型不兼容
- **建议**：请检查传递到数据流中的参数值是否与声明的类型匹配。

 ### <a name="error-code-df-executor-columnunavailable"></a>错误代码：DF-Executor-ColumnUnavailable
- **消息**：表达式中使用的列名不可用或无效
- 原因：表达式中使用的列名无效或不可用
- **建议**：请检查表达式中使用的列名

 ### <a name="error-code-df-executor-parseerror"></a>错误代码：DF-Executor-ParseError
- **消息**：无法分析表达式
- 原因：表达式因格式设置而出现分析错误
- **建议**：检查表达式中的格式设置

### <a name="error-code-getcommand-outputasync-failed"></a>错误代码:GetCommand OutputAsync 失败

- **消息**：在数据流调试和数据预览期间：GetCommand OutputAsync 失败，出现...
- 原因：这是后端服务错误。 可以重试此操作，并重启调试会话。
- **建议**：如果重试操作和重启会话无法解决问题，请联系客户支持。

### <a name="error-code-hit-unexpected-exception-and-execution-failed"></a>错误代码:遇到了意外异常，执行失败

- **消息**：在数据流活动执行期间：遇到了意外异常，执行失败。
- 原因：这是后端服务错误。 可以重试此操作，并重启调试会话。
- **建议**：如果重试操作和重启会话无法解决问题，请联系客户支持。

### <a name="error-code-debug-data-preview-no-output-data-on-join"></a>错误代码：调试数据预览联接时无输出数据

- **消息**：存在大量的 null 值或缺失值，这可能是因为采样的行太少。 尝试更新调试行限制并刷新数据。
- 原因：连接条件与任何行都不匹配，或者在数据预览期间生成大量的 NULL 值。
- **建议**：转到“调试”设置，然后增加源行限制中的行数。 确保选择的 Azure IR 的数据流群集足够大，可以处理更多数据。

### <a name="error-code-validation-error-at-source-with-multiline-csv-files"></a>错误代码：验证多行 CSV 文件是否存在源错误 

- **消息**：可能会显示以下错误消息之一：
   - 最后一列为 NULL 或缺失。
   - 源上的架构验证失败。
   - 架构导入无法在 UX 中正确显示，并且最后一列的名称中包含换行符。
- 原因：在映射数据流中，多行 CSV 源目前不使用 \r\n 作为行分隔符。 有时，回车时多余的行会中断源值。 
- **建议**：使用 \n（而不是 \r\n）作为行分隔符在源中生成文件， 或使用“复制活动”将带有 \r\n 的 CSV 文件转换为 \n 作为行分隔符。

## <a name="general-troubleshooting-guidance"></a>一般故障排除指南

1. 检查数据集连接状态。 在每个源转换和接收器转换中，访问要使用的每个数据集的链接服务，然后测试连接。
1. 在数据流设计器中，检查文件和表连接的状态。 打开“调试”并单击源转换上的“数据预览”，以确保你能够访问数据。
1. 如果数据预览中的一切正常，请转到管道设计器，并将数据流置于管道活动中。 调试管道，以进行端到端测试。

