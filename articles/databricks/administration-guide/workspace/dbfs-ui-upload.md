---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/02/2020
title: 管理数据上传 - Azure Databricks
description: 了解如何启用和禁用使用用户界面将数据上传到 Databricks 文件系统的功能。
ms.openlocfilehash: d7868928ef7b85687a279a1addd22c9f9c3eec8b
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058990"
---
# <a name="manage-data-upload"></a><a id="disable-ui-upload"> </a><a id="manage-data-upload"> </a>管理数据上传

作为管理员用户，你可以使用[可视化上传界面](../../data/databricks-file-system.md#file-upload-interface)管理用户将数据上传到 Databricks 文件系统 (DBFS) 的权限：

1. 转到[管理控制台](../admin-console.md)。
2. 单击“高级”  选项卡。
3. 单击“使用 UI 上传数据”右侧的“禁用”或“启用”按钮。 默认情况下会启用此选项。

此设置不会控制通过特定方式（例如通过 DBFS 命令行界面）对 Databricks 文件系统进行的[编程访问](../../data/databricks-file-system.md#access-dbfs)。