---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 查询片段 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的查询片段。
ms.openlocfilehash: 620806da31fe54d78be293ef28af576061113cbf
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692203"
---
# <a name="query-snippets"></a>查询代码片段

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

通常，复制之前的内容进行修改比从头编写内容要简单得多。 对常见的 ``JOIN`` 语句或复杂的 ``CASE`` 表达式来说尤其如此。 SQL Analytics 中的查询列表在不断扩充，因此很难记住哪些查询包含你需要的语句。

查询片段是可使用自动补全功能进行共享和触发的查询段。

> [!div class="mx-imgBorder"]
> ![查询片段](../../../_static/images/sql/snippet.png)

下面是一个简单的片段示例：

```
JOIN organizations org ON org.id = ${1:table}.org_id
```

## <a name="create-a-query-snippet"></a>创建查询片段

1. 单击边栏底部的 ![用户设置图标](../../../_static/images/icons/user-settings-icon.png) 图标，然后选择“设置”。
2. 单击“查询片段”选项卡。
3. 单击“+新建查询片段”。
4. 在“触发器”字段中，输入片段触发器。
5. （可选）输入说明。
6. 在“片段”字段中，输入片段。
7. 单击 **“创建”** 。

## <a name="insertion-points"></a>插入点

``${1:table}`` 是带有占位符文本的插入点。 当 SQL Analytics 显示片段时，将删除美元符号 ``$`` 和大括号 ``{}``，并突出显示单词 ``table`` 进行替换。

> [!NOTE]
>
> 在运行时，可使用占位符文本作为理想的默认值。

可使用一个美元符号加大括号 ``${}`` 将整数 Tab 键顺序括起来，从而指定插入点。 前面带有一个冒号 ``:`` 的文本占位符是可选的，但对不熟悉你的片段的用户来说很有用。

当 SQL Analytics 显示此片段时：

```sql
AND (invoices.complete IS NULL OR invoices.complete <> '${2}')
AND (invoices.canceled IS NULL OR invoices.canceled <> '${1}')
AND (invoices.modified IS NULL OR invoices.modified_date <> '${0: this_date}')
```

文本插入点会跳到引号 ``''`` 之间的第二行。 按 Tab 时，插入点将向后跳到第一行。 再次按 Tab 时，插入点将跳到第三行，并且突出显示 ``this_date`` 以提示输入所需的值。

> [!NOTE]
>
> 零 ``${0}`` 的插入点始终是 Tab 键顺序中最后一个点。

## <a name="insert-a-query-snippet"></a>插入查询片段

如果[已启用自动补全](queries.md#auto-complete)，可键入在查询片段编辑器中定义的触发词，从查询编辑器调用你的片段。 自动补全功能会像数据库中的其他任何关键字一样建议片段。

> [!NOTE]
>
> 如果自动补全功能已被禁用，你仍可按“Ctrl + 空格”，并键入查询片段的触发词来调用查询片段。 如果架构超过 5,000 令牌，则必须这样做。

下面是有关片段的其他一些建议：

* 常用的 ``JOIN`` 语句
* 复杂子句，例如 ``WITH`` 或 ``CASE``。
* [条件格式](https://discuss.redash.io/t/conditional-formatting-general-text-formatting/1706/1)