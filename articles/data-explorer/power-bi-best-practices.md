---
title: 有关使用 Power BI 查询和可视化 Azure 数据资源管理器数据的最佳做法
description: 本文提供有关使用 Power BI 查询和可视化 Azure 数据资源管理器数据的最佳做法。
author: orspod
ms.author: v-tawe
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: how-to
origin.date: 09/26/2019
ms.date: 01/19/2021
ms.openlocfilehash: d2ec90b324e1de3c47b376cca90ae1fc61835a85
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611297"
---
# <a name="best-practices-for-using-power-bi-to-query-and-visualize-azure-data-explorer-data"></a>有关使用 Power BI 查询和可视化 Azure 数据资源管理器数据的最佳做法

Azure 数据资源管理器是一项快速且高度可缩放的数据探索服务，适用于日志和遥测数据。 [Power BI](https://docs.microsoft.com/power-bi/) 是一种业务分析解决方案，可以用来可视化数据，并在组织内共享结果。 Azure 数据资源管理器提供三个选项用于连接到 Power BI 中的数据。 使用[内置连接器](power-bi-connector.md)、[将 Azure 数据资源管理器中的查询导入 Power BI](power-bi-imported-query.md)，或使用 [SQL 查询](power-bi-sql-query.md)。 本文提供有关使用 Power BI 查询和可视化 Azure 数据资源管理器数据的提示。 

## <a name="best-practices-for-using-power-bi"></a>有关使用 Power BI 的最佳做法

在处理 TB 量级的全新原始数据时，请遵循以下准则，使 Power BI 仪表板和报表保持简洁并包含最新的数据：

* **轻便** - 只在 Power BI 中引入报表所需的数据。 若要进行深入的交互式分析，请使用 [Azure 数据资源管理器 Web UI](web-query-data.md)，此界面已经过优化，适合通过 Kusto 查询语言进行即席浏览。

* **复合模型** - 使用 [复合模型](https://docs.microsoft.com/power-bi/desktop-composite-models)将顶级仪表板的聚合数据与筛选的操作原始数据合并到一起。 可以明确定义何时使用原始数据，何时使用聚合视图。 

* **导入模式与 DirectQuery 模式** - 使用“导入”模式来与小型数据集交互。  对于频繁更新的大型数据集，请使用“DirectQuery”模式。  例如，使用“导入”模式创建维度表，因为这些表较小且不经常更改。  根据预期的数据更新频率设置刷新间隔。 使用“DirectQuery”模式创建事实数据表，因为这些表较大且包含原始数据。  使用这些表可以通过 Power BI [钻取](https://docs.microsoft.com/power-bi/desktop-drillthrough)呈现筛选的数据。

* **并行度** - Azure 数据资源管理器是可线性缩放的数据平台，因此，可以通过提高端到端流的并行度来改善高仪表板的呈现性能，如下所示：

   * 增加 [Power BI 中 DirectQuery 的并发连接](https://docs.microsoft.com/power-bi/desktop-directquery-about#maximum-number-of-connections-option-for-directquery)数目。

  * 使用[弱一致性提高并行度](kusto/concepts/queryconsistency.md)。 这可能会影响数据的新鲜度。

* **有效切片器** – 使用 [同步切片器](https://docs.microsoft.com/power-bi/visuals/power-bi-visualization-slicers#sync-and-use-slicers-on-other-pages)来防止报表在你准备就绪之前加载数据。 构建数据集、放置所有视觉对象并标记所有切片器之后，可以选择同步切片器，以便仅加载所需的数据。

* **使用筛选器** - 尽量使用最多的 Power BI 筛选器，以便在 Azure 数据资源管理器中重点搜索相关的数据分片。

* **有效视觉对象** – 为数据选择性能最高的视觉对象。


## <a name="tips-for-using-the-azure-data-explorer-connector-for-power-bi-to-query-data"></a>有关使用 Power BI 的 Azure 数据资源管理器连接器查询数据的提示

以下部分包含有关在 Power BI 中使用 Kusto 查询语言的提示和技巧。 使用 [Power BI 的 Azure 数据资源管理器连接器](power-bi-connector.md)可视化数据

### <a name="complex-queries-in-power-bi"></a>Power BI 中的复杂查询

与 Power Query 相比，在 Kusto 中可以更轻松地表达复杂查询。 这些查询应作为 [Kusto 函数](kusto/query/functions/index.md)实现，并在 Power BI 中调用。 在 Kusto 查询中结合 `let` 语句使用 **DirectQuery** 时，必须采用此方法。 由于 Power BI 联接两个查询，而 `let` 语句不能与 `join` 运算符结合使用，因此可能会出现语法错误。 请将每个联接部分保存为 Kusto 函数，并允许 Power BI 将这两个函数联接在一起。

### <a name="how-to-simulate-a-relative-date-time-operator"></a>如何模拟相对日期时间运算符

Power BI 不包含 `ago()` 之类的相对日期时间运算符。 
若要模拟 `ago()`，请结合使用 `DateTime.FixedLocalNow()` 和 `#duration` Power BI 函数。

不要编写如下所示的使用 `ago()` 运算符的查询：

```kusto
    StormEvents | where StartTime > (now()-5d)
    StormEvents | where StartTime > ago(5d)
``` 

使用以下等效查询：

```m
let
    Source = AzureDataExplorer.Contents("help", "Samples", "StormEvents", []),
    #"Filtered Rows" = Table.SelectRows(Source, each [StartTime] > (DateTime.FixedLocalNow()-#duration(5,0,0,0)))
in
    #"Filtered Rows"
```

### <a name="configuring-azure-data-explorer-connector-options-in-m-query"></a>在 M 查询中配置 Azure 数据资源管理器连接器选项

可以从使用 M 查询语言的 PBI 高级编辑器配置 Azure 数据资源管理器连接器的选项。 借助这些选项，便可以控制要发送到 Azure 数据资源管理器群集的已生成查询。

```m
let
    Source = AzureDataExplorer.Contents("help", "Samples", "StormEvents", [<options>])
in
    Source
```

可以在 M 查询中使用以下任意选项：

| 选项 | 示例 | 说明
|---|---|---
| MaxRows | `[MaxRows=300000]` | 将 `truncationmaxrecords` set 语句添加到查询中。 替代查询可以返回给调用方的默认最大记录数（截断）。
| MaxSize | `[MaxSize=4194304]` | 将 `truncationmaxsize` set 语句添加到查询中。 替代允许查询返回给调用方的默认最大数据大小（截断）。
| NoTruncate | `[NoTruncate=true]` | 将 `notruncation` set 语句添加到查询中。 启用禁止截断返回给调用方的查询结果的功能。
| AdditionalSetStatements | `[AdditionalSetStatements="set query_datascope=hotcache"]` | 将提供的 set 语句添加到查询中。 这些语句用于设置查询持续时间的查询选项。 查询选项控制查询的执行方式并返回结果。
| CaseInsensitive | `[CaseInsensitive=true]` | 使连接器生成不区分大小写的查询；在比较值时，查询将使用 `=~` 运算符而不是 `==` 运算符。
| ForceUseContains | `[ForceUseContains=true]` | 在使用文本字段时，使连接器生成使用 `contains`（而不是默认的 `has`）的查询。 虽然 `has` 的性能要高很多，但它不会处理子字符串。 若要详细了解这两个运算符之间的差异，请参阅[字符串运算符](./kusto/query/datatypes-string-operators.md)。
| 超时 | `[Timeout=#duration(0,10,0,0)]` | 将查询的客户端和服务器超时都配置为提供的持续时间。

> [!NOTE]
> 可将多种选项组合在一起来实现所需的行为：`[NoTruncate=true, CaseInsensitive=true]`

### <a name="reaching-kusto-query-limits"></a>达到 Kusto 查询限制

默认情况下，如[查询限制](kusto/concepts/querylimits.md)中所述，Kusto 查询最多返回 500,000 行或 64 MB 的结果。 可以使用“Azure 数据资源管理器(Kusto)”连接窗口中的“高级选项”来替代这些默认值：

![高级选项](media/power-bi-best-practices/advanced-options.png)

这些选项会连同查询一起发出 [set 语句](kusto/query/setstatement.md)，以更改默认查询限制：

* “限制查询结果记录数”生成 `set truncationmaxrecords`
* “限制查询结果数据大小(字节)”生成 `set truncationmaxsize`
* “禁用结果集截断”生成 `set notruncation`

### <a name="case-sensitivity"></a>事例敏感性

默认情况下，在比较字符串值时，连接器会生成使用区分大小写的 `==` 运算符的查询。 如果数据不区分大小写，则不是所需的行为。 若要更改生成的查询，请使用 `CaseInsensitive` 连接器选项：

```m
let
    Source = AzureDataExplorer.Contents("help", "Samples", "StormEvents", [CaseInsensitive=true]),
    #"Filtered Rows" = Table.SelectRows(Source, each [State] == "aLaBama")
in
    #"Filtered Rows"
```

### <a name="using-query-parameters"></a>使用查询参数

可以使用[查询参数](kusto/query/queryparametersstatement.md)来动态修改查询。 

#### <a name="using-a-query-parameter-in-the-connection-details"></a>在连接详细信息中使用查询参数

使用查询参数来筛选查询中的信息并优化查询性能。
 
在“编辑查询”窗口中，选择“主页” > “高级编辑器”

1. 找到以下查询节：

    ```m
    Source = AzureDataExplorer.Contents("<Cluster>", "<Database>", "<Query>", [])
    ```

   例如：

    ```m
    Source = AzureDataExplorer.Contents("Help", "Samples", "StormEvents | where State == 'ALABAMA' | take 100", [])
    ```

1. 将查询的相关部分替换为你自己的参数。 将查询拆分为多个部分，并使用“与”符号 (&) 以及参数重新将其连接到一起。

   例如，在以上查询中，我们取 `State == 'ALABAMA'` 部分，将其拆分为 `State == '` 和 `'`，并在它们之间放置 `State` 参数：

    ```kusto
    "StormEvents | where State == '" & State & "' | take 100"
    ```

1. 如果查询包含引号，请正确对其编码。 例如，以下查询：

   ```kusto
   "StormEvents | where State == "ALABAMA" | take 100"
   ```

   在“高级编辑器”中显示时包含两个引号，如下所示：

   ```kusto
    "StormEvents | where State == ""ALABAMA"" | take 100"
   ```

   应将它替换为包含三个引号的查询，如下所示：

   ```kusto
   "StormEvents | where State == """ & State & """ | take 100"
   ```

#### <a name="use-a-query-parameter-in-the-query-steps"></a>在查询步骤中使用查询参数

可以在支持查询参数的任何查询步骤中使用查询参数。 例如，根据某个参数的值筛选结果。

![使用参数筛选结果](media/power-bi-best-practices/filter-using-parameter.png)

### <a name="use-valuenativequery-for-azure-data-explorer-features"></a>使用 Value.NativeQuery 获取 Azure 数据资源管理器功能

若要使用 Power BI 不支持的 Azure 数据资源管理器功能，请使用 M 中的 [Value.NativeQuery()](/powerquery-m/value-nativequery) 方法。此方法用于在生成的查询中插入 Kusto 查询语言片段，还可用于更好地控制执行的查询。

以下示例演示如何在 Azure 数据资源管理器中使用 `percentiles()` 函数：

```m
let
    StormEvents = AzureDataExplorer.Contents(DefaultCluster, DefaultDatabase){[Name = DefaultTable]}[Data],
    Percentiles = Value.NativeQuery(StormEvents, "| summarize percentiles(DamageProperty, 50, 90, 95) by State")
in
    Percentiles
```

### <a name="dont-use-power-bi-data-refresh-scheduler-to-issue-control-commands-to-kusto"></a>不要使用 Power BI 数据刷新计划程序向 Kusto 发出控制命令。

Power BI 包含可以定期对数据源发出查询的数据刷新计划程序。 不应使用此机制在 Kusto 中计划控制命令，因为 Power BI 假设所有查询都是只读的。

### <a name="power-bi-can-send-only-short-lt2000-characters-queries-to-kusto"></a>Power BI 只能向 Kusto 发送短查询（&lt; 2000 字符）

如果在 Power BI 中运行查询导致以下错误：“DataSource.Error:Web.Contents 无法从 ... 获取内容”，原因是查询长度可能超过了 2000 个字符。 Power BI 使用 **PowerQuery** 并通过发出 HTTP GET 请求来查询 Kusto，而该请求会将查询编码为正在检索的 URI 的一部分。 因此，Power BI 发出的 Kusto 查询不能超过请求 URI 的最大长度（2000 字符减去小偏移量）。 解决方法之一是在 Kusto 中定义一个[存储函数](kusto/query/schema-entities/stored-functions.md)，并让 Power BI 在查询中使用该函数。

## <a name="next-steps"></a>后续步骤

[使用 Power BI 的 Azure 数据资源管理器连接器直观显示数据](power-bi-connector.md)
