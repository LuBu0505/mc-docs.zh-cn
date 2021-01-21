---
title: 使用 Jupyter Notebook 分析 Azure 数据资源管理器中的数据
description: 本主题介绍如何使用 Jupyter Notebook 和 kqlmagic 扩展来分析 Azure 数据资源管理器中的数据。
author: orspod
ms.author: v-tawe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: how-to
origin.date: 10/20/2020
ms.date: 01/19/2021
ms.openlocfilehash: 734f3bba756157612ecd458e41739ac383368e83
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611688"
---
# <a name="use-a-jupyter-notebook-and-kqlmagic-extension-to-analyze-data-in-azure-data-explorer"></a>使用 Jupyter Notebook 和 kqlmagic 扩展来分析 Azure 数据资源管理器中的数据

Jupyter Notebook 是一种开源 Web 应用程序，可用于创建和共享包含实时代码、公式、可视化效果和叙述性文本的文档。 使用情况包括数据清理和转换、数值模拟、统计建模、数据可视化和机器学习。
[Jupyter Notebook](https://jupyter.org/) 支持 Magic 函数，这些函数可通过支持其他命令扩展内核的功能。 kqlmagic 是一种命令，可在 Jupyter Notebook 中扩展 Python 内核的功能，以便在本机运行 Kusto 语言查询。 可以轻松地组合使用 Python 和 Kusto 查询语言，通过与 `render` 命令集成的丰富 Plot.ly 库查询和可视化数据。 用于运行查询的数据源受支持。 这些数据源包括 Azure 数据资源管理器（一个用于日志和遥测数据的快速且高度可缩放的数据探索服务），以及 Azure Monitor 日志和 Application Insights。 kqlmagic 还适用于 Azure Data Studio、Jupyter 实验室和 Visual Studio Code Jupyter 扩展。

## <a name="prerequisites"></a>先决条件

- 组织电子邮件帐户是 Azure Active Directory (Azure AD) 的成员。
- 在本地计算机上安装了 Jupyter Notebook，否则请使用 [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/notebooks/notebooks-kqlmagic?view=sql-server-ver15)

## <a name="install-kqlmagic-library"></a>安装 kqlmagic 库

1. 安装 kqlmagic：

    ```python
    !pip install Kqlmagic --no-cache-dir  --upgrade
    ```

1. 加载 kqlmagic：

    ```python
    %reload_ext Kqlmagic
    ```
    > [!NOTE]
    > 通过单击“内核”>“更改内核”>“Python 3.6”将内核版本更改为 Python 3.6
    
## <a name="connect-to-the-azure-data-explorer-help-cluster"></a>连接到 Azure 数据资源管理器 Help 群集

使用以下命令可连接到 Help 群集上托管的 Samples 数据库。 对于非 Microsoft Azure AD 用户，请将租户名称 `Microsoft.com` 替换为你的 Azure AD 租户。

```python
%kql AzureDataExplorer://tenant="Microsoft.com";code;cluster='help';database='Samples'
```

> [!Note]
> 如果使用自己的 ADX 群集，则必须在连接字符串中包含区域，如下所示：   
   ```%kql azuredataexplorer://tenant="youecompany.com";code;cluster='mycluster.chinaeast2';database='mykustodb'```

## <a name="query-and-visualize"></a>查询和可视化

查询数据使用 [render 运算符](kusto/query/renderoperator.md)，而可视化数据使用 ploy.ly 库。 此查询和可视化操作提供了使用本机 KQL 的集成体验。 Kqlmagic 支持大多数图表，但是 `timepivot`、`pivotchart` 和 `ladderchart` 除外。 除 `kind`、`ysplit` 和 `accumulate` 之外的所有属性都支持 Render。 

### <a name="query-and-render-piechart"></a>查询和呈现饼图

```python
%%kql
StormEvents
| summarize statecount=count() by State
| sort by statecount 
| limit 10
| render piechart title="My Pie Chart by State"
```

### <a name="query-and-render-timechart"></a>查询和呈现时间表

```python
%%kql
StormEvents
| summarize count() by bin(StartTime,7d)
| render timechart
```

> [!NOTE]
> 这些图表是交互式的。 选择时间范围可放大到特定时间。

### <a name="customize-the-chart-colors"></a>自定义图表颜色

如果不喜欢默认调色板，可使用调色板选项自定义图表。 在此处可找到可用的调色板：[针对 kqlmagic 查询图表结果选择调色板](https://mybinder.org/v2/gh/Microsoft/jupyter-kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)

1. 对于调色板列表：

    ```python
    %kql --palettes -popup_window
    ```

1. 选择 `cool` 调色板并重新呈现查询：

    ```python
    %%kql -palette_name "cool"
    StormEvents
    | summarize statecount=count() by State
    | sort by statecount
    | limit 10
    | render piechart title="My Pie Chart by State"
    ```

## <a name="parameterize-a-query-with-python"></a>使用 Python 将查询参数化

Kqlmagic 可用于在 Kusto 查询语言与 Python 之间进行简单的交换。 若要了解详细信息，请访问以下链接：[使用 Python 将 kqlmagic 查询参数化](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb)

### <a name="use-a-python-variable-in-your-kql-query"></a>在 KQL 查询中使用 Python 变量

可在查询中使用 Python 变量的值来筛选数据：

```python
statefilter = ["TEXAS", "KANSAS"]
```

```python
%%kql
let _state = statefilter;
StormEvents 
| where State in (_state) 
| summarize statecount=count() by bin(StartTime,1d), State
| render timechart title = "Trend"
```

### <a name="convert-query-results-to-pandas-dataframe"></a>将查询结果转换到 Pandas DataFrame

可在 Pandas DataFrame 中访问 KQL 查询的结果。 通过变量 `_kql_raw_result_` 访问上次执行的查询结果，并轻松地将结果转换到 Pandas DataFrame 中，如下所示：

```python
df = _kql_raw_result_.to_dataframe()
df.head(10)
```

### <a name="example"></a>示例

在很多分析方案中，可能需要创建包含多个查询的可复用笔记本，并将结果从一个查询馈送到后续查询中。 以下示例使用 Python 变量 `statefilter` 来筛选数据。

1. 运行查询以查看最大值为 `DamageProperty` 的前 10 个状态：

    ```python
    %%kql
    StormEvents
    | summarize max(DamageProperty) by State
    | order by max_DamageProperty desc
    | limit 10
    ```

1. 运行查询以提取第一个状态并将其设置到 Python 变量中：

    ```python
    df = _kql_raw_result_.to_dataframe()
    statefilter =df.loc[0].State
    statefilter
    ```

1. 使用 `let` 语句和 Python 变量运行查询：

    ```python
    %%kql
    let _state = statefilter;
    StormEvents 
    | where State in (_state)
    | summarize statecount=count() by bin(StartTime,1d), State
    | render timechart title = "Trend"
    ```

1. 运行 help 命令：

    ```python
    %kql --help "help"
    ```

> [!TIP]
> 若要接收有关所有可用配置的信息，请使用 `%config Kqlmagic`。 若要排查和捕获 Kusto 错误（例如连接问题和不正确的查询），请使用 `%config Kqlmagic.short_errors=False`

## <a name="next-steps"></a>后续步骤

运行 help 命令以浏览下面包含所有支持的功能的笔记本示例：
- [适用于 Azure 数据资源管理器的 kqlmagic 入门](https://mybinder.org/v2/gh/Microsoft/jupyter-kqlmagic/master?filepath=notebooks%2FQuickStart.ipynb) 
- [适用于 Application Insights 的 kqlmagic 入门](https://mybinder.org/v2/gh/Microsoft/jupyter-kqlmagic/master?filepath=notebooks%2FQuickStartAI.ipynb) 
- [适用于 Azure Monitor 日志的 kqlmagic 入门](https://mybinder.org/v2/gh/Microsoft/jupyter-kqlmagic/master?filepath=notebooks%2FQuickStartLA.ipynb) 
- [使用 Python 将 kqlmagic 查询参数化](https://mybinder.org/v2/gh/Microsoft/jupyter-kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb) 
- [针对 kqlmagic 查询图表结果选择调色板](https://mybinder.org/v2/gh/Microsoft/jupyter-kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)
