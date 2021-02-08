---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/02/2020
title: 管理 DBFS 文件浏览器 - Azure Databricks
description: 了解如何启用和禁用使用可视化浏览器界面在 Databricks 文件系统中浏览数据的功能。
ms.openlocfilehash: 0815af229192e60be090efe7c12d20fb9ecbd9f2
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060814"
---
# <a name="manage-the-dbfs-file-browser"></a>管理 DBFS 文件浏览器

作为管理员用户，你可以使用[可视化浏览器界面](../../data/databricks-file-system.md#browse-dbfs-using-the-ui)管理用户在 Databricks 文件系统 (DBFS) 中浏览数据的功能。

1. 转到[管理控制台](../admin-console.md)。
2. 单击“高级”  选项卡。
3. 单击“DBFS 文件浏览器”右侧的“禁用”或“启用”按钮  。 默认情况下禁用此选项。

此设置不会控制通过特定方式（例如通过 DBFS 命令行界面）对 Databricks 文件系统进行的[编程访问](../../data/databricks-file-system.md#access-dbfs)。