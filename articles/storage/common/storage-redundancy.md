---
title: 数据冗余
titleSuffix: Azure Storage
description: 了解 Azure 存储中的数据冗余。 复制 Azure 存储帐户中的数据，实现持久性和高可用性。
services: storage
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 01/08/2021
ms.date: 01/18/2021
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: b181f3111edb5cdbf2b37684a1c3c9f56b02a15c
ms.sourcegitcommit: f086abe8bd2770ed10a4842fa0c78b68dbcdf771
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98163213"
---
# <a name="azure-storage-redundancy"></a>Azure 存储冗余

Azure 存储始终会存储数据的多个副本，以防范各种计划内和计划外的事件，包括暂时性的硬件故障、网络中断或断电、大范围自然灾害等。 冗余可确保即使遇到故障，存储帐户也能达到其可用性和持久性目标。

在确定最适合自己方案的冗余选项时，请考虑如何在较低成本与较高可用性之间做出取舍。 可帮助你确定应选择哪种冗余选项的因素包括：  

- 如何在主要区域中复制数据
- 是否要将你的数据复制到地理上距主要区域较远的另一个区域，以防范区域性灾难
- 应用程序是否要求在主要区域出于任何原因而不可用时，能够对次要区域中复制的数据进行读取访问

## <a name="redundancy-in-the-primary-region"></a>主要区域中的冗余

Azure 存储帐户中的数据始终在主要区域中复制三次：

**本地冗余存储 (LRS)** 在主要区域中的单个物理位置内同步复制数据三次。 LRS 是成本最低的复制选项，但对于需要高可用性的应用程序，不建议使用此选项。
### <a name="locally-redundant-storage"></a>本地冗余存储

本地冗余存储 (LRS) 在主要区域中的单个物理位置内复制数据三次。 LRS 可在一年中提供至少 99.999999999%（11 个 9）的对象持久性。

与其他选项相比，LRS 是成本最低的冗余选项，但提供的持久性也最低。 LRS 可以保护数据，使其不受服务器机架和驱动器故障影响。 但是，如果数据中心发生火灾或洪灾等灾难，使用 LRS 的存储帐户的所有副本可能会丢失或不可恢复。 为了减轻此风险，Azure 建议使用[异地冗余存储](#geo-redundant-storage) (GRS)。

对使用 LRS 的存储帐户的写入请求将以同步方式发生。 只有将数据写入到所有三个副本后，写入操作才会成功返回。

对于以下场景，LRS 是不错的选项：

- 如果应用程序存储着在发生数据丢失时可轻松重构的数据，则可以选择 LRS。
- 如果出于数据监管要求，应用程序限制为只能在一个区域中复制数据，则可以选择 LRS。 在某些情况下，要在其中异地复制数据的配对区域可位于另一个国家/地区。 有关配对区域的详细信息，请参阅 [Azure 区域](/best-practices-availability-paired-regions)。

## <a name="redundancy-in-a-secondary-region"></a>次要区域中的冗余

对于需要高可用性的应用程序，可以选择将存储帐户中的数据另外复制到距离主要区域数百英里以外的次要区域。 如果存储帐户已复制到次要区域，则即使遇到区域性服务完全中断或导致主要区域不可恢复的灾难，数据也能持久保存。

创建存储帐户时，可以为帐户选择主要区域。 配对的次要区域是根据主要区域确定的且无法更改。 有关 Azure 支持的区域的详细信息，请参阅 [Azure 区域](https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=all)。

Azure 存储提供一个将数据复制到次要区域的选项：

**异地冗余存储 (GRS)** 使用 LRS 在主要区域中的单个物理位置内同步复制数据三次。 然后，它将数据异步复制到次要区域中的单个物理位置。

如果主要区域不可用，可以选择故障转移到次要区域。 故障转移完成后，次要区域将成为主要区域，你便可以再次读取和写入数据。 有关灾难恢复以及如何故障转移到次要区域的详细信息，请参阅[灾难恢复和存储帐户故障转移](storage-disaster-recovery-guidance.md)。

> [!IMPORTANT]
> 由于数据以异步方式复制到次要区域，因此如果无法恢复主要区域，则影响主要区域的故障可能会导致数据丢失。 最近对主要区域的写入与最近对次要区域的写入之间的时间间隔称为恢复点目标 (RPO)。 RPO 指示可以将数据恢复到的时间点。 Azure 存储的 RPO 通常小于 15 分钟，但目前没有有关将数据复制到次要区域所用的时长的 SLA。

### <a name="geo-redundant-storage"></a>异地冗余存储

异地冗余存储 (GRS) 使用 LRS 在主要区域中的单个物理位置内同步复制数据三次。 然后，它将数据异步复制到距离主要区域数百英里以外的次要区域中的单个物理位置。 GRS 在给定一年中提供至少 99.99999999999999%（16 个 9）的 Azure 存储数据对象持久性。

首先会将写入操作提交到主要位置，并使用 LRS 复制该操作。 然后会以异步方式将更新复制到次要区域。 将数据写入次要位置后，还会使用 LRS 在该位置复制数据。

## <a name="read-access-to-data-in-the-secondary-region"></a>对次要区域中数据的读取访问

异地冗余存储将数据复制到次要区域中的另一个物理位置，以防范区域性服务中断。 但是，仅当客户或 Azure 发起了从主要区域到次要区域的故障转移时，数据才可供读取。 启用对次要区域的读取访问时，数据可供随时读取（包括主要区域不可用的情况）。 若要对次要区域进行读取访问，请启用读取访问异地冗余存储 (RA-GRS)。

> [!NOTE]
> Azure 文件存储不支持读取访问异地冗余存储 (RA-GRS)。

### <a name="design-your-applications-for-read-access-to-the-secondary"></a>设计应用程序以便能够对次要区域进行读取访问

如果存储帐户已配置为对次要区域进行读取访问，则你可以设计自己的应用程序，以便在主要区域出于任何原因而不可用时，能够无缝转换为从次要区域读取数据。 

启用 RA-GRS 后，次要区域可用于读取访问，因此你可以预先测试应用程序，以确保在发生服务中断时可以从次要区域正确读取数据。 有关如何设计应用程序以实现高可用性的详细信息，请参阅[使用异地冗余设计高度可用的应用程序](geo-redundant-design.md)。

启用对次要区域的读取访问后，应用程序可以从次要终结点以及主要终结点读取数据。 次要终结点在帐户名的后面追加了后缀 *-secondary*。 例如，如果 Blob 存储的主要终结点是 `myaccount.blob.core.chinacloudapi.cn`，则次要终结点是 `myaccount-secondary.blob.core.chinacloudapi.cn`。 存储帐户的帐户访问密钥对于主要终结点和次要终结点是相同的。

### <a name="check-the-last-sync-time-property"></a>检查“上次同步时间”属性

由于数据以异步方式复制到次要区域，因此次要区域通常位于主要区域的后面。 如果主要区域发生故障，对主要区域的所有写入有可能尚未复制到次要区域。

若要确定哪些写入操作已复制到次要区域，应用程序可以检查存储帐户的“上次同步时间”属性。 在上次同步时间之前写入到主要区域的所有写入操作已成功复制到次要区域，这意味着，可以从次要区域读取这些操作。 在上次同步时间之后写入到主要区域的任何写入操作不一定已复制到次要区域，这意味着，它们不一定可供读取操作使用。

可以使用 Azure PowerShell、Azure CLI 或某个 Azure 存储客户端库查询“上次同步时间”属性的值。 “上次同步时间”属性是一个 GMT 日期/时间值。 有关详细信息，请参阅[检查存储帐户的“上次同步时间”属性](last-sync-time-get.md)。

## <a name="summary-of-redundancy-options"></a>冗余选项摘要

以下各部分中的表总结了可用于 Azure 存储的冗余选项

### <a name="durability-and-availability-parameters"></a>持久性和可用性参数

下表描述了每个冗余选项的关键参数：

| 参数 | LRS | GRS/RA-GRS |
|:-|:-|:-|
| 给定一年内的对象持久性百分比 | 至少 99.999999999%（11 个 9） | 至少 99.99999999999999%（16 个 9） |
| 读取请求的可用性 | 至少为 99.9%（冷访问层为 99%） | GRS 至少为 99.9%（冷访问层为 99%）<br /><br />RA-GRS 至少为 99.99%（冷访问层为 99.9%） |
| 写入请求的可用性 | 至少为 99.9%（冷访问层为 99%） | 至少为 99.9%（冷访问层为 99%） |

### <a name="durability-and-availability-by-outage-scenario"></a>持久性和可用性（按中断方案）

下表指示数据是否持久且在给定方案中可用，具体取决于对存储帐户启用的冗余类型：

| 中断方案 | LRS | GRS/RA-GRS |
|:-|:-|:-|
| 数据中心内的某个节点不可用 | 是 | 是 |
| 整个数据中心（区域性或非区域性）不可用 | 否 | 是<sup>1</sup> |
| 主要区域发生区域范围的服务中断 | 否 | 是<sup>1</sup> |
| 如果主区域变得不可用，可对次要区域进行读取访问 | 否 | 是（通过 RA-GRS） |

<sup>1</sup> 如果主要区域变为不可用，则需要执行帐户故障转移来恢复写入可用性。 有关详细信息，请参阅[灾难恢复和存储帐户故障转移](storage-disaster-recovery-guidance.md)。

### <a name="supported-storage-account-types"></a>支持的存储帐户类型

下表显示了每种存储帐户类型都支持哪些冗余选项。 有关存储帐户类型，请参阅[存储帐户概述](storage-account-overview.md)。

| LRS | GRS/RA-GRS |
|:-|:-|
| 常规用途 v2<br /> 常规用途 v1<br /> 块 Blob 存储<br /> Blob 存储<br /> 文件存储 | 常规用途 v2<br /> 常规用途 v1<br /> Blob 存储 |

所有存储帐户的所有数据的复制均基于存储帐户的冗余选项。 复制的对象包括块 Blob、追加 Blob、页 Blob、队列、表和文件。 将复制所有层（包括存档层）中的数据。 有关 blob 层的详细信息，请参阅 [Azure Blob 存储：热、冷和存档访问层](../blobs/storage-blob-storage-tiers.md)。

有关每个冗余选项的定价信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/)。

> [!NOTE]
> Azure 高级磁盘存储目前仅支持本地冗余存储 (LRS)。 块 Blob 存储帐户在特定区域支持本地冗余存储 (LRS)。

## <a name="data-integrity"></a>数据完整性

Azure 存储使用循环冗余检查 (CRC) 定期验证存储的数据的完整性。 如果检测到数据损坏，则使用冗余数据进行修复。 Azure 存储还对所有网络流量计算校验和，以检测存储或检索数据时数据包损坏的情况。

## <a name="see-also"></a>另请参阅

- [检查存储帐户的“上次同步时间”属性](last-sync-time-get.md)
- [更改存储帐户的冗余选项](redundancy-migration.md)
- [使用异地冗余设计高度可用的应用程序](geo-redundant-design.md)
- [灾难恢复和存储帐户故障转移](storage-disaster-recovery-guidance.md)
