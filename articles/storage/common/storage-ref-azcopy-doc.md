---
title: azcopy doc | Microsoft Docs
description: 本文提供有关 azcopy doc 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 07/24/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 21cbca78759bf717f27124b6645d62a046c00aee
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196222"
---
# <a name="azcopy-doc"></a>azcopy doc

以 Markdown 格式生成工具的文档。

## <a name="synopsis"></a>概要

以 Markdown 格式生成工具的文档，并将其存储在指定位置。

默认情况下，这些文件存储在当前目录中名为“doc”的文件夹中。

```azcopy
azcopy doc [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](./storage-use-azcopy-v10.md#transfer-data)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="options"></a>选项

|选项|说明|
|--|--|
|-h、--help|显示 doc 命令的帮助内容。|

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

|选项|说明|
|---|---|
|--cap-mbps float|以兆位/秒为单位限制传输速率。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。|
|--output-type string|命令输出的格式。 选项包括：text、json。 默认值为“text”。|
|--trusted-microsoft-suffixes 字符串   | 指定可在其中发送 Azure Active Directory 登录令牌的其他域后缀。  默认值为“.core.windows.net;.core.chinacloudapi.cn;.core.cloudapi.de;.core.usgovcloudapi.net” 。 此处列出的任何内容都会添加到默认值。 为安全起见，应只在此处放置 Azure 域。 用分号分隔多个条目。|

## <a name="see-also"></a>另请参阅

- [azcopy](storage-ref-azcopy.md)
