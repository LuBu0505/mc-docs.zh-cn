---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/19/2020
title: LZO 压缩文件 - Azure Databricks
description: 了解如何使用 Azure Databricks 读取 LZO 压缩文件中的数据。
ms.openlocfilehash: 33853f989fda2ae8686c2796c045c8f007e083f1
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059878"
---
# <a name="lzo-compressed-files"></a><a id="lzo"> </a><a id="lzo-compressed-files"> </a>LZO 压缩文件

由于许可限制，默认情况下，Azure Databricks 群集上不提供 LZO 压缩编解码器。 若要读取 LZO 压缩文件，必须在启动时使用 [init 脚本](../../clusters/init-scripts.md)在群集上安装编解码器。

本文包含两个笔记本：

* 初始化 LZO 压缩文件
  * 生成 LZO 编解码器。
  * 创建一个可执行以下操作的 init 脚本：
  * 安装 LZO 压缩库和 `lzop` 命令，并将 LZO 编解码器复制到正确的类路径。
  * 将 Spark 配置为使用 LZO 压缩编解码器。
* 读取 LZO 压缩文件 - 使用通过 init 脚本安装的编解码器。

## <a name="init-lzo-compressed-files-notebook"></a>初始化 LZO 压缩文件的笔记本

[获取笔记本](../../_static/notebooks/init-lzo-compressed-files.html)

## <a name="read-lzo-compressed-files-notebook"></a>读取 LZO 压缩文件的笔记本

[获取笔记本](../../_static/notebooks/read-lzo-compressed-files.html)