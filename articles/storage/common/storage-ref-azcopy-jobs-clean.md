---
title: azcopy jobs clean | Microsoft Docs
description: 本文提供有关 azcopy jobs clean 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 07/24/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 0b19fe988ee535af66eb076b7fcfc8ec2d6ebd7c
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196217"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

删除所有作业的所有日志和计划文件

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](./storage-use-azcopy-v10.md#transfer-data)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="examples"></a>示例

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>选项

--help - clean 命令的帮助。

--with-status - 字符串。仅删除具有此状态的作业，可用值：`Canceled`、`Completed`、`Failed`、`InProgress`、`All`（默认值 `All`）

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

--cap-mbps float - 限制传输速率（以兆位/秒为单位）。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。

**--output-type** 字符串   命令输出的格式。 选项包括：text、json。 默认值为“text”。 （默认值为“text”）

--trusted-microsoft-suffixes 字符串指定可向其中发送 Azure Active Directory 登录令牌的其他域后缀。  默认值为“.core.windows.net;.core.chinacloudapi.cn;.core.cloudapi.de;.core.usgovcloudapi.net” 。 此处列出的任何内容都会添加到默认值。 为安全起见，应只在此处放置 Azure 域。 用分号分隔多个条目。

## <a name="see-also"></a>另请参阅

- [azcopy jobs](storage-ref-azcopy-jobs.md)
