---
title: Azure 文件可伸缩性和性能目标
description: 了解 Azure 文件的可伸缩性和性能目标信息，包括容量、请求速率以及入站和出站带宽限制。
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 02/12/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.subservice: files
ms.openlocfilehash: f53eecd8a6c0dd3cb1dce50dac6ff77e544cc79f
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196345"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure 文件可伸缩性和性能目标
[Azure 文件存储](storage-files-introduction.md)在云中提供可通过 SMB 文件系统协议访问的完全托管式文件共享。 本文讨论了 Azure 文件的可伸缩性和性能目标。

此处列出的可伸缩性和性能目标是高端目标，但可能会受部署中的其他变量影响。 例如，除了受限于承载着 Azure 文件共享的服务器之外，针对文件的吞吐量还可能会受限于你的可用网络带宽。 强烈建议你对使用模式进行测试，以确定 Azure 文件的可伸缩性和性能是否满足你的要求。 随着时间的推移，我们也一直在努力提高这些限制。 

## <a name="azure-files-scale-targets"></a>Azure 文件规模目标
Azure 文件共享将部署到存储帐户。存储帐户是代表存储共享池的顶级对象。 此存储池可用于部署多个文件共享。 因此，需要考虑三个类别：存储帐户、Azure 文件共享和文件。

### <a name="storage-account-scale-targets"></a>存储帐户缩放目标
对于客户的不同存储方案，Azure 支持多种类型的存储帐户，但对于 Azure 文件存储，有两个主要类型的存储帐户。 需要创建的存储帐户类型取决于你是要创建标准文件共享还是要创建高级文件共享： 

- **常规用途版本 2 (GPv2) 存储帐户**：使用 GPv2 存储帐户可以在标准的/基于硬盘（基于 HDD）的硬件上部署 Azure 文件共享。 除了存储 Azure 文件共享以外，GPv2 存储帐户还可以存储其他存储资源，例如 Blob 容器、队列或表。

- **FileStorage 存储帐户**：使用 FileStorage 存储帐户可以在高级/基于固态磁盘（基于 SSD）的硬件上部署 Azure 文件共享。 FileStorage 帐户只能用于存储 Azure 文件共享；其他存储资源（Blob 容器、队列、表等）都不能部署在 FileStorage 帐户中。

| 属性 | GPv2 存储帐户（标准） | FileStorage 存储帐户（高级） |
|-|-|-|
| 每个订阅每个区域的存储帐户数 | 250 | 250 |
| 最大存储帐户容量 | 5 PiB<sup>1</sup> | 100 TiB（预配值） |
| 最大文件共享数 | 无限制 | 无限制，所有共享的总预配大小必须小于最大存储帐户容量 |
| 最大并发请求速率 | 20,000 IOPS<sup>1</sup> | 100,000 IOPS |
| 最大流入量 | LRS：10 Gbp/秒<sup>1</sup>，GRS：5 Gbp/秒<sup>1</sup> | 4,136 MiB/秒 |
| 最大流出量 | 50 Gbp/秒<sup>1</sup> | 6,204 MiB/秒 |
| 虚拟网络规则的数目上限 | 200 | 200 |
| IP 地址规则的数目上限 | 200 | 200 |
| 管理读取操作数目 | 每 5 分钟 800 次 | 每 5 分钟 800 次 |
| 管理写入操作数目 | 每秒 10 次/每小时 1200 次 | 每秒 10 次/每小时 1200 次 |
| 管理列出操作数目 | 每 5 分钟 100 次 | 每 5 分钟 100 次 |

<sup>1</sup> 常规用途版本 2 存储帐户根据请求支持更高的容量上限和更高的流入量上限。

### <a name="azure-file-share-scale-targets"></a>Azure 文件共享缩放目标
| 属性 | 标准文件共享 | 高级文件共享 |
|-|-|-|
| 文件共享的最小大小 | 无最小值 | 100 GiB（预配值） |
| 预配大小增加/减少单位 | 不可用 | 1 GiB |
| 文件共享的最大大小 | <ul><li>100 TiB，启用了大型文件共享功能<sup>1</sup></li><li>5 TiB，默认值</li></ul> | 100 TiB |
| 文件共享中的文件数上限 | 无限制 | 无限制 |
| 最大请求速率（最大 IOPS） | <ul><li>10,000，启用了大型文件共享功能<sup>1</sup></li><li>每 100 毫秒 1,000 或 100 个请求，默认值</li></ul> | <ul><li>基线 IOPS：400 + 每 GiB 1 IOPS，最高 100,000</li><li>IOPS 突发：Max (4000, 每 GiB 3x IOPS)，最高 100,000</li></ul> |
| 单个文件共享的最大流入量 | <ul><li>最高 300 MiB/秒，启用了大型文件共享功能<sup>1</sup></li><li>最高 60 MiB/秒，默认值</li></ul> | 40 MiB/秒 + 0.04 * 预配 GiB |
| 单个文件共享的最大流出量 | <ul><li>最高 300 MiB/秒，启用了大型文件共享功能<sup>1</sup></li><li>最高 60 MiB/秒，默认值</li></ul> | 60 MiB/秒 + 0.06 * 预配 GiB |
| 共享快照的最大数目 | 200 个快照 | 200 个快照 |
| 最大对象（目录和文件）名称长度 | 2,048 个字符 | 2,048 个字符 |
| 最大路径名组成部分（在路径 \A\B\C\D 中，每个字母是一个组成部分） | 255 个字符 | 255 个字符 |
| SMB 多路通道的最大数量 | 不适用 | 4 |
| 每个文件共享的存储的访问策略的最大数目 | 5 | 5 |

<sup>1</sup> 标准文件共享的默认值为 5 TiB。若要详细了解如何将标准文件共享纵向扩展到 100 TiB，请参阅[启用和创建大型文件共享](./storage-files-how-to-create-large-file-share.md)。

### <a name="file-scale-targets"></a>文件缩放目标
| 属性 | 标准文件共享中的文件  | 高级文件共享中的文件  |
|-|-|-|
| 文件大小上限 | 4 TiB | 4 TiB |
| 最大并发请求速率 | 1,000 IOPS | 最高 8,000<sup>1</sup> |
| 文件的最大流入量 | 60 MiB/秒 | 200 MiB/秒 |
| 文件的最大流出量 | 60 MiB/秒 | 300 MiB/秒 |
| 最大并发句柄数 | 2,000 个句柄 | 2,000 个句柄  |

<sup>1 适用于读取和写入 IO（通常，较小的 IO 大小小于或等于64 KiB）。读取和写入操作之外的元数据操作数目可能更低。</sup>

## <a name="see-also"></a>另请参阅
- [规划 Azure 文件部署](storage-files-planning.md)
