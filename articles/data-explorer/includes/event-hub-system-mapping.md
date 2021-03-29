---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 03/23/2021
ms.author: v-junlch
ms.openlocfilehash: d1bdf4752855a6cd082cc2841cf0cdd6e31c00e4
ms.sourcegitcommit: bed93097171aab01e1b61eb8e1cec8adf9394873
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105609451"
---
> [!NOTE]
> * `json` 和表格格式（例如 `csv`、`tsv`）支持系统属性，而压缩数据不支持它们。 使用不受支持的格式时，仍会引入数据，但会忽略属性。
> * 对于表格数据，仅单记录事件消息支持系统属性。
> * 对于 JSON 数据，多记录事件消息也支持系统属性。 在这种情况下，系统属性仅添加到事件消息的第一条记录中。 
> * 对于 `csv` 映射，属性将按[系统属性](../ingest-data-event-hub-overview.md#system-properties)表中列出的顺序添加到记录的开头。
> * 对于 `json` 映射，将根据[系统属性](../ingest-data-event-hub-overview.md#system-properties)表中的属性名称添加属性。
