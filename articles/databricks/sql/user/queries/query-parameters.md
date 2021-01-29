---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/11/2021
title: 查询参数 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的查询参数。
ms.openlocfilehash: c69ad4c05875a7f44480164d9f8e7054c890330f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692265"
---
# <a name="query-parameters"></a>查询参数

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

通过查询参数可在运行时将值替换到查询中。 双大括号 ``{{ }}`` 之间的任何字符串都被视为查询参数。 结果窗格上方会出现一个小组件，你可在其中设置参数值。

> [!div class="mx-imgBorder"]
> ![查询参数](../../../_static/images/sql/parameter-example.png)

有关演示，请查看[查询参数视频](https://www.youtube.com/embed/02iQXd2Ouos)。

## <a name="add-a-query-parameter"></a>添加查询参数

1. 单击“添加新参数”![“新建参数”按钮](../../../_static/images/sql/add-parameter-button.png) 按钮或键入 Cmd + P 。

   参数将插入到文本脱字号处，并显示“添加参数”对话框。

   > [!div class="mx-imgBorder"]
   > ![“新建参数”对话框](../../../_static/images/sql/add-query-parameter.png)

   * **关键字**：表示查询中的参数的关键字。
   * **标题**：显示在小组件上方的标题。 默认情况下，标题与关键字相同。
   * **类型**：支持的类型包括文本、数字、日期、日期和时间、日期和时间（以秒为单位）、下拉列表和基于查询的下拉列表。 默认值为 Text。
2. 输入关键字，可选择覆盖标题，然后选择参数类型。
3. 单击“添加参数”。
4. 在参数小组件中，设置参数值。
5. 单击“应用更改”。
6. 单击“保存” 。

若要使用其他参数值重新运行查询，请在小组件中输入值，然后单击“应用更改”。

## <a name="edit-a-query-parameter"></a>编辑查询参数

若要编辑参数，请单击参数小组件旁边的齿轮图标。 若要防止没有查询的用户更改参数，请单击“仅显示结果”。

> [!div class="mx-imgBorder"]
> ![查询参数](../../../_static/images/sql/parameter-example.png)

“``<Keyword>``”参数对话框会显示：

> [!div class="mx-imgBorder"]
> ![参数设置](../../../_static/images/sql/parameter-type.png)

## <a name="query-parameter-types"></a>查询参数类型

* [Text](#text)
* [数字](#number)
* [日期、日期和时间、日期和时间（以秒为单位）](#date-date-and-time-date-and-time-with-seconds)
* [日期范围、日期和时间范围、日期和时间范围（以秒为单位）](#daterange-date-and-time-range-date-and-time-range-with-seconds)
* [下拉列表](#dropdown-list)
* [基于查询的下拉列表](#query-based-dropdown-list)

### <a name="text"></a>文本

采用字符串作为输入。 反斜杠、单引号和双引号将被转义，Azure Databricks 将向此参数添加引号。 例如，``mr's Li"s`` 之类的字符串会被转换成 ``'mr\'s Li\"s'``。使用此形式的示例可以是

```
SELECT * FROM users WHERE name={{ text_param }}
```

### <a name="number"></a>Number

采用数字作为其输入。 使用此形式的示例可以是

```
SELECT * FROM users WHERE age={{ number_param }}
```

### <a name="date-date-and-time-date-and-time-with-seconds"></a>日期、日期和时间、日期和时间（以秒为单位）

这三个参数非常类似。 唯一的区别是它们的精准率。 它们采用特定的时间 ``(12/11/2020 12:01)``，或采用表示时间 ``(Today, Yesterday)`` 的字符串。 此参数的示例是 ``SELECT * from usage_logs where date='{{ date_param }}'``。  必须向参数添加引号。

日期参数使用熟悉的日历选取界面，可默认为当前日期和时间。 可从 3 个精度级别中进行选择：日期、日期和时间，以及日期和时间（以秒为单位）

> [!NOTE]
>
> 日期参数作为字符串传递给数据库。 必须用单引号 (``'``) 或数据库要求的任何形式将它们括起来，以声明字符串。

### <a name="daterange-date-and-time-range-date-and-time-range-with-seconds"></a>日期范围、日期和时间范围、日期和时间范围（以秒为单位）

日期范围参数采用开始日期和结束日期 ``(12/09/2020 12:01 - 12/11/2020 13:01)``，或者采用表示时间 ``(Last week, Last month)`` 的字符串。 这些参数会插入两个名为 ``.start`` 和 ``.end`` 的标记，表示所选日期范围的开始和结束。 此参数的示例是

```
SELECT year(birthDate) as birthYear, count(*) AS total
FROM default.people10m
WHERE firstName = {{ Name }} AND gender = 'F' and birthDate > '{{ Date Range.start }}' and birthDate < '{{ Date Range.end }}'
GROUP BY birthYear
ORDER BY birthYear
```

必须向参数添加引号。

日期范围参数使用组合小组件来简化范围选择。

> [!div class="mx-imgBorder"]
> ![日期范围参数](../../../_static/images/sql/date-range-picker.png)

#### <a name="dynamic-date-and-date-range-values"></a>动态日期和日期范围值

将日期或日期范围参数添加到查询时，选择小组件将显示蓝色闪电图标。 单击它可显示动态值，例如 ``last month``、``yesterday`` 或 ``last year``。 这些值每天都会动态更新。

> [!div class="mx-imgBorder"]
> ![动态日期](../../../_static/images/sql/quick-date-range.png)

> [!IMPORTANT]
>
> 动态日期和日期范围与计划查询不兼容。

### <a name="dropdown-list"></a>下拉列表

若要在运行查询时限制可能参数值的范围，可使用“下拉列表”参数类型。 例如 ``SELECT * FROM users WHERE name='{{ dropdown_param }}'``。  从“参数设置”面板中选择后，将显示一个文本框，你可在其中输入允许的值，每个值之间用新行分隔。 下拉列表是文本参数，因此如果要在下拉列表中使用日期或日期和时间，应按数据源要求的格式输入。 字符串不会进行转义。 可选择单值或多值下拉列表。

* **单值**：参数两侧需要单引号。
* **多值**：切换“允许多值”选项。 在引用下拉列表中，选择是否用引号将参数引起来，是使用单引号还是双引号。 如果选择引号，则无需在参数两侧添加引号。

  > [!div class="mx-imgBorder"]
  > ![允许多个值](../../../_static/images/sql/multi-select-dropdown.png)

在查询中，将 ``WHERE`` 子句更改为使用 ``IN`` 关键字。

```
SELECT ...
FROM   ...
WHERE field IN ( {{ Multi Select Parameter }} )
```

通过参数多选小组件，可向数据库传递多个值。

> [!div class="mx-imgBorder"]
> ![多选](../../../_static/images/sql/multi-select.png)

### <a name="query-based-dropdown-list"></a>基于查询的下拉列表

将查询结果作为其输入。 它与“下拉列表”参数的行为相同。

1. 单击设置面板中“类型”下的“基于查询的下拉列表” 。
2. 单击“查询”字段并选择一个查询。 如果目标查询返回大量记录，则性能将降低。

如果目标查询返回多个列，SQL Analytics 将使用第一个列。 如果目标查询返回 ``name`` 列和 ``value`` 列，则 SQL Analytics 将使用 ``name`` 列填充参数选择小组件，但使用关联的 ``value`` 列执行查询。

例如，假设以下查询：

```sql
SELECT user_uuid AS 'value', username AS 'name'
FROM users
```

返回此数据：

| 值 | name         |
|-------|--------------|
| 1001  | John Smith   |
| 1002  | Jane Doe     |
| 1003  | Bobby 表 |

下拉列表小组件如下所示：

> [!div class="mx-imgBorder"]
> ![John Smith、Jane Doe 和 Bobby 表](../../../_static/images/sql/dropdown-list-name-value.png)

当 SQL Analytics 执行查询时，传递给数据库的值将是 1001、1002 或 1003。

## <a name="query-parameter-mapping-in-dashboards"></a>仪表板中的查询参数映射

可在仪表板中控制查询参数。 可将不同小组件中的参数链接在一起、设置静态参数值，或者为每个小组件单独选择值。

添加依赖于参数值的仪表板小组件时，可选择参数映射。 基础查询中的每个参数显示在“参数”列表中。

> [!div class="mx-imgBorder"]
> ![参数映射](../../../_static/images/sql/dashboard_parameter_mapping.png)

你还可单击仪表板小组件右上角的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，再单击“编辑参数”来访问参数映射界面。 参数属性会显示：

* **标题**：显示在仪表板上的值选择器旁边的显示名称。 它默认为参数关键字。 若要编辑它，请单击铅笔图标 ![铅笔图标](../../../_static/images/sql/pencil-icon.png)。 静态仪表板参数不显示标题，因为值选择器是隐藏的。 如果选择“静态值”作为值源，则标题字段将灰显。
* **关键字**：基础查询中此参数的字符串字面量。 这有助于在仪表板未返回预期结果的情况下进行调试。
* **默认值**：如果未指定其他值，则使用该值。 若要在查询屏幕中更改此设置，请使用所需的参数值执行查询，然后单击“保存”按钮。
* **值源**：参数值的源。 若要选择源，请单击铅笔图标 ![铅笔图标](../../../_static/images/sql/pencil-icon.png)。
  * **新的仪表板参数**：创建新的仪表板级别参数。 这样，你就可在仪表板上的一个位置设置参数值，并将其映射到多个可视化效果。
  * **现有仪表板参数**：将参数映射到现有仪表板参数。 必须指定预先存在的仪表板参数。
  * **小组件参数**：显示仪表板小组件中的值选择器。 对于不在小组件之间共享的一次性参数，这非常有用。
  * **静态值**：为小组件选择一个静态值，而不考虑在其他小组件上使用的值。 静态映射的参数值不会在更紧凑的仪表板上的任何位置显示值选择器。 这使你能够利用查询参数的灵活性，当某些参数预计不会频繁更改时，让仪表板上的用户界面不因此混乱。

## <a name="frequently-asked-questions-faq"></a>常见问题 (FAQ)

* [能否在单个查询中多次重用同一个参数？](#can-i-reuse-the-same-parameter-multiple-times-in-a-single-query)
* [能否在单个查询中使用多个参数？](#can-i-use-multiple-parameters-in-a-single-query)

### <a name="can-i-reuse-the-same-parameter-multiple-times-in-a-single-query"></a>能否在单个查询中多次重用同一个参数？

是。 请在大括号中使用同一个标识符。 此示例使用 ``{{org_id}}`` 参数两次。

```
SELECT {{org_id}}, count(0)
FROM queries
WHERE org_id = {{org_id}}
```

### <a name="can-i-use-multiple-parameters-in-a-single-query"></a>能否在单个查询中使用多个参数？

是。 请为每个参数使用唯一的名称。 此示例使用两个参数：``{{org_id}}`` 和 ``{{start_date}}``。

```
SELECT count(0)
FROM queries
WHERE org_id = {{org_id}} AND created_at > '{{start_date}}'
```