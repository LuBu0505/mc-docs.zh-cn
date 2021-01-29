---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 表 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的表。
ms.openlocfilehash: 2605b8dec4298a45b91da7e702c48d6848caa650
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692252"
---
# <a name="tables"></a>表

对于支持本机查询语法（SQL 或 NOSQL）的数据源，可以通过修改查询来选择数据返回格式、返回的列以及返回的顺序。 但 CSV 文件或 Google Sheets 等源不支持查询语法。 通过 SQL Analytics，你可以手动对表可视化效果中的数据进行重新排序、隐藏和格式设置。

## <a name="visualization-settings"></a>可视化效果设置

若要开始使用，请单击表视图下的“编辑可视化效果”按钮。 此时将显示设置面板，如下所示：

> [!div class="mx-imgBorder"]
> ![“可视化效果”选项](../../../_static/images/sql/table-viz-options.png)

可以执行以下操作：

* 通过使用用黄色突出显示的句柄向上或向下拖动列来重新排序
* 通过切换用绿色突出显示的 ![可见性图标](../../../_static/images/sql/visibility-icon.png) 图标来隐藏列
* 使用用红色突出显示的格式设置来设置列格式

## <a name="format-columns"></a>设置列格式

SQL Analytics 支持大多数数据库通用的数据类型：文本、数字、日期和布尔值。 它还对非标准列类型（如 JSON 文档、图像和链接）提供特殊支持。

> [!NOTE]
>
> SQL Analytics 清理查询结果中的 HTML。 但是，任何保留的 HTML 标记默认不会进行转义。 因此，如果查询结果包含含有 HTML 的字符串字段（例如来自 Web 抓取程序），则可能会出现奇怪的效果。 切换可视化编辑器中的“允许 HTML 内容”设置可转义 HTML 字符。

### <a name="common-data-types"></a>常见数据类型

如果基础数据源未提供类型信息，则 SQL Analytics 会将列呈现为文本。 可使用表可视化编辑器强制它使用任意类型。 这对于不支持类型数据的源（如 SQLite、Google Sheets 或 CSV 文件）特别有用。 例如，可以：

* 显示所有浮点数的小数点后三位
* 仅显示日期列的月份和年份
* 零填充所有整数
* 在数字字段中前面追加或后面追加文本

有关对数字和日期和时间数据类型进行格式设置的参考信息，请参阅：

* [数字数据类型](format-numeric.md)
* [日期和时间](https://momentjs.com/docs/#/displaying/format/)

### <a name="special-data-types"></a>特殊数据类型

SQL Analytics 还支持通用数据库规范之外的数据类型。

* **JSON 文档**：如果基础数据在字段中返回 JSON 格式的文本，则可以指示 SQL Analytics 这样显示它。 这种显示方式可以以清晰的格式折叠和展开元素。 当你使用 [JSON 外部数据源](../../admin/external-data-sources/json-api.md)查询 REST API 时，这特别有用。
* **映像**：如果数据库中的某个字段包含指向某个图像的链接，则 SQL Analytics 可将该图像与表结果进行内联并显示出来。 这对于仪表板特别有用。

   ![包含图像的仪表板](../../../_static/images/sql/dashboard-with-images.png)

  在前面的仪表板中，“客户图像”字段是指向 SQL Analytics 就地显示的图片的 URL。

* **HTML 链接**：与图像一样，可以将仪表板中的 HTML 链接设置为可单击的链接。 只需使用列格式选择器中的“链接”选项。