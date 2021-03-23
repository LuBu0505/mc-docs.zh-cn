---
title: Azure 文件常见问题解答 (FAQ) | Microsoft Docs
description: 获取 Azure 文件存储常见问题的解答。 你可以在云或本地的 Windows、Linux 和 macOS 部署上同时装载 Azure 文件共享。
author: WenJason
ms.service: storage
origin.date: 02/23/2020
ms.date: 03/22/2021
ms.author: v-jay
ms.subservice: files
ms.topic: conceptual
ms.openlocfilehash: 64fa33aaefcc66b58b9cdfc451c1a81304f3a9b6
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766217"
---
# <a name="frequently-asked-questions-faq-about-azure-files"></a>有关 Azure 文件的常见问题解答 (FAQ)
[Azure 文件](storage-files-introduction.md)在云端提供完全托管的文件共享，这些共享项可通过行业标准的[服务器消息块 (SMB) 协议](https://docs.microsoft.com/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview)进行访问。 你可以在云或 Windows、Linux 和 macOS 的本地部署同时装载 Azure 文件共享。 另外，你也可以使用 Azure 文件同步在 Windows Server 计算机上缓存 Azure 文件共享，以在靠近使用数据的位置实现快速访问。

## <a name="general"></a>常规
* <a id="why-files-useful"></a>
  **Azure 文件如何发挥作用？**  
   你可以使用 Azure 文件在云中创建文件共享，而无需应对物理服务器、设备或装置的开销。 诸如应用操作系统更新和更换损坏的磁盘等琐碎枯燥的工作，我们可为你逐一代劳。 若要详细了解 Azure 文件适用的应用场景，请参阅[为何 Azure 文件很有用](storage-files-introduction.md#why-azure-files-is-useful)。

* <a id="file-access-options"></a>
  **访问 Azure 文件中的文件有哪些不同方式？**  
    可以使用 SMB 3.0 协议将 SMB 文件共享装载在本地计算机上，也可以使用[存储资源管理器](https://storageexplorer.com/)之类的工具访问文件共享中的文件。 在应用程序中，可以使用存储客户端库、REST API、PowerShell 或 Azure CLI 来访问 Azure 文件共享中的文件。


* <a id="files-versus-blobs"></a>
  **相对于 Azure Blob 存储，我为什么要对数据使用 Azure 文件共享？**  
    Azure 文件和 Azure Blob 存储均提供在云中存储大量数据的方法，但是用途略有不同。 
    
    Azure Blob 存储适用于需要存储非结构化数据且具有大规模缩放性的云本机应用程序。 为了更大程度地提升性能和可缩放性，相对于真实的文件系统而言，Azure Blob 存储是更简单的存储抽象。 此外，只可通过基于 REST 的客户端库访问 Azure Blob 存储（或直接通过基于 REST 的协议访问）。

    Azure 文件是一个专门的文件系统， 具有你在使用本地操作系统多年来所熟知和喜爱的所有文件抽象。 例如 Azure Blob 存储，Azure 文件提供了一个 REST 接口和基于 REST 的客户端库。 与 Azure Blob 存储不同的是，Azure 文件提供了对 Azure 文件共享的 SMB 访问权限。 文件共享可以直接装载到本地或云 VM 中的 Windows、Linux 或 macOS，无需编写任何代码，也无效为文件系统附加任何特殊驱动程序。 
   
    有关 Azure 文件和 Azure Blob 存储之间差异的深入描述，请参阅[核心 Azure 存储服务简介](../common/storage-introduction.md)。 若要了解有关 Azure Blob 存储的详细信息，请参阅 [Blob 存储简介](../blobs/storage-blobs-introduction.md)。

* <a id="files-versus-disks"></a>**相对于 Azure 磁盘，我为什么要使用 Azure 文件共享？**  
    Azure 磁盘中的磁盘只是一个磁盘。 若要充分利用 Azure 磁盘，必须将其与在 Azure 中运行的虚拟机相关联。 Azure 磁盘可用于在本地服务器上使用磁盘的所有内容。 你可将其用作操作系统磁盘、操作系统的交换空间，或者应用程序的专用存储空间。 Azure 磁盘其中一个有趣的用途是，可在云中创建一个文件服务器，以在可能使用 Azure 文件共享的相同位置使用。 当需要 Azure 文件存储当前不支持的部署选项时，在 Azure 虚拟机中部署文件服务器则是一种非常行之有效的获取 Azure 中文件存储的方法。 

    但是，相比使用 Azure 文件共享，通过将 Azure 磁盘作为后端存储来运行文件服务器的方式，由于多方面的原因，其经济成本通常会更高。 首先，除了为磁盘存储付费之外，还必须为运行一个或多个 Azure VM 的成本付费。 其次，你还必须管理用于运行文件服务器的 VM。 例如，负责操作系统升级。 最后，如果你最终需要在本地缓存数据，则还要自行安装和管理复制技术（例如，分布式文件系统复制 (DFSR)）来实现此目的。


    有关在 Azure 中设置高性能和高可用性文件服务器的选项的信息，请参阅 [Deploying IaaS VM guest clusters in Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)（在 Azure 中部署 IaaS VM 来宾群集）。 有关 Azure 文件和 Azure 磁盘之间的差异的深入描述，请参阅[核心 Azure 存储服务简介](../common/storage-introduction.md)。 若要详细了解 Azure 磁盘，请参阅 [Azure 托管磁盘概述](../../virtual-machines/managed-disks-overview.md)。

* <a id="get-started"></a>
  **如何开始使用 Azure 文件？**  
   开始使用 Azure 文件非常简单。 首先，[创建 SMB 文件共享](storage-how-to-create-file-share.md)，然后再将其装载到首选操作系统中： 

  * [在 Windows 中装载 SMB 共享](storage-how-to-use-files-windows.md)
  * [在 Linux 中装载 SMB 共享](storage-how-to-use-files-linux.md)
  * [在 macOS 中装载 SMB 共享](storage-how-to-use-files-mac.md)

    有关部署 Azure 文件共享以替换组织中生产文件共享的详细指南，请参阅[规划 Azure 文件部署](storage-files-planning.md)。

* <a id="redundancy-options"></a>
  **Azure 文件支持哪些存储冗余选项？**  
    当前，Azure 文件支持本地冗余存储 (LRS) 和异地冗余存储 (GRS)。 将来我们计划支持读取访问权限异地冗余存储 (RA-GRS)，但目前还没有可分享的日程表。

* <a id="tier-options"></a>
  **Azure 文件支持哪些存储层？**  
    Azure 文件支持两种存储层：高级存储层和标准存储层。 标准文件共享是在常规用途（GPv1 或 GPv2）存储帐户中创建的，高级文件共享是在 FileStorage 存储帐户中创建的。 详细了解如何创建[标准文件共享](storage-how-to-create-file-share.md)和[高级文件共享](./storage-how-to-create-file-share.md)。 
    
    > [!NOTE]
    > 你无法使用 Blob 存储帐户或高级常规用途（GPv1 或 GPv2）存储帐户创建 Azure 文件共享。 标准 Azure 文件共享必须只能在标准常规用途帐户中创建，高级 Azure 文件共享必须只能在 FileStorage 存储帐户中创建。 *高级* 常规用途（GPv1 和 GPv2）存储帐户仅适用于高级页 Blob。 

## <a name="security-authentication-and-access-control"></a>安全性、身份验证和访问控制
* <a id="ad-support"></a>
**Azure 文件是否支持访问控制？**  
    
    Azure 文件还提供了另外一种方法来管理访问控制：

    - 你可以使用共享访问签名 (SAS) 生成在指定时间间隔内有效的具有特定权限的令牌。 例如，可以生成在 10 分钟后到期、对特定文件具有只读访问权限的令牌。 只要拥有此有效令牌，就可以在 10 分钟内拥有对给定文件的只读访问权限。 仅通过 REST API 或客户端库支持共享的访问签名密钥。 你必须使用存储帐户密钥通过 SMB 装载 Azure 文件共享。

当前，Azure 文件不直接支持 Active Directory。

* <a id="encryption-at-rest"></a>
**如何确保已静态加密 Azure 件共享？**  

    是的。 有关详细信息，请参阅 [Azure 存储服务加密](../common/storage-service-encryption.md?toc=%2fstorage%2ffiles%2ftoc.json)。

* <a id="access-via-browser"></a>
**如何使用 Web 浏览器提供对特定文件的访问权限？**  

    你可以使用共享访问签名生成在指定时间间隔内有效的、具有特定权限的令牌。 例如，可以生成一个在设定时段内对特定文件具有只读访问权限的令牌。 只要拥有此 URL，就可以在令牌有效期间，直接通过任意 Web 浏览器访问文件。 你可以从类似存储资源管理器的 UI 轻松生成共享的访问签名密钥。

* <a id="file-level-permissions"></a>
**能否对共享中的文件夹指定只读或只写权限？**  

    如果使用 SMB 装载文件共享，则不具有文件夹级的控制权限。 但是，如果使用 REST API 或客户端库创建共享访问签名，则可以在共享内的文件夹上指定只读或只写权限。

* <a id="ip-restrictions"></a>
**是否对 Azure 文件共享实现 IP 限制？**  

    是的。 可以在存储帐户级别对 Azure 文件共享的权限进行限制。 有关详细信息，请参阅[配置 Azure 存储防火墙和虚拟网络](../common/storage-network-security.md?toc=%2fstorage%2ffiles%2ftoc.json)。

* <a id="data-compliance-policies"></a>
**Azure 文件支持哪些数据符合性策略？**  

   Azure 文件所依据的存储体系结构与 Azure 存储中的其他存储服务使用的相同。 Azure 文件实施的数据符合性策略也与其他 Azure 存储服务使用的相同。 有关 Azure 存储数据符合性的详细信息，可以参阅 [Azure 信任中心](https://www.azure.cn/support/trust-center/)。

* <a id="file-auditing"></a>
**如何审核 Azure 文件存储中的文件访问和更改？**

  可以使用 [Azure 存储日志](/storage/common/monitor-storage?tabs=azure-powershell#analyzing-log-data)来跟踪文件更改和用户访问权限。 这些日志可用于故障排除，系统会尽最大努力记录请求。

## <a name="on-premises-access"></a>本地访问

* <a id="port-445-blocked"></a>
**我的 ISP 或 IT 阻止了端口 445，这导致 Azure 文件装载失败。我该怎样做？**

    可在此处了解[解决端口 445 受阻问题的各种方法](./storage-troubleshoot-windows-file-connection-problems.md#cause-1-port-445-is-blocked)。 Azure 文件只允许来自区域或数据中心外部的使用 SMB 3.0（带加密支持）的连接。 SMB 3.0 协议引入了许多安全功能，包括通道加密，在 Internet 上使用非常安全。 但是，由于在较低 SMB 版本中发现漏洞的历史原因，端口 445 可能已被阻止。 理想情况下，应仅针对 SMB 1.0 流量阻止该端口，并且应在所有客户端上关闭 SMB 1.0。

* <a id="expressroute-not-required"></a>
**必须使用 Azure ExpressRoute 才能在本地连接到 Azure 文件或使用 Azure 文件同步吗？**  

    不是。 ExpressRoute 不是访问 Azure 文件共享的必要条件。 如果要直接在本地装载 Azure 文件共享，则只需打开端口 445（TCP 出站）即可进行 Internet 访问（这是 SMB 用于进行通信的端口）。 如果正在使用 Azure 文件同步，则只需端口 443（TCP 出站）即可进行 HTTPS 访问（无需 SMB）。 但是，你可以将 ExpressRoute 与这些访问选项中任意一项一起使用。

* <a id="mount-locally"></a>
**如何才能在本地计算机上装载 Azure 文件共享？**  

    可以使用 SMB 协议装载文件共享，只要端口 445（TCP 出站）处于打开状态，且客户端支持 SMB 3.0 协议（例如，如果使用的是 Windows 10 或 Windows Server 2016）。 如果端口 445 被组织的策略或 ISP 阻止，则可使用 Azure 文件同步访问 Azure 文件共享。

## <a name="backup"></a>备份
* <a id="backup-share"></a>
**如何备份我的 Azure 文件共享？**  
    可以使用定期[共享快照](storage-snapshots-files.md)来防止意外删除。 此外，也可以使用 AzCopy、RoboCopy 或能够备份已装载文件共享的第三方备份工具。

## <a name="share-snapshots"></a>共享快照

### <a name="share-snapshots-general"></a>共享快照：常规
* <a id="what-are-snaphots"></a>
**什么是文件共享快照？**  
    可以使用 Azure 文件共享快照创建只读版本的文件共享。 另外，也可以使用 Azure 文件将早期版本的内容复制回 Azure 或本地中的同一个共享或备用位置中，以做进一步修改。 若要了解有关共享快照的详细信息，请参阅[共享快照概述](storage-snapshots-files.md)。

* <a id="where-are-snapshots-stored"></a>
**共享快照存储在何处？**  
    共享快照与文件共享存储在同一个存储帐户中。

* <a id="snapshot-consistency"></a>
**共享快照是否与应用程序一致？**  
    否，共享快照与应用程序不一致。 执行共享快照前，用户必须将应用程序中的写入刷新到共享中。

* <a id="snapshot-limits"></a>
**对我可使用的共享快照数有限制吗？**  
    是的。 Azure 文件可以最多保留 200 张共享快照。 共享快照不计入共享配额，因此，对所有共享快照使用的总空间没有单独的共享限制。 存储帐户限制仍然适用。 在达到 200 个共享快照之后，必须删除旧的共享快照才可创建新的共享快照。

* <a id="snapshot-cost"></a>
**共享快照的费用是多少？**  
    快照按标准事务和标准存储收费。 快照在本质上是递增的。 基本快照即是共享本身。 所有的后续快照均是递增的，并且只会存储与之前快照的不同之处。 这意味着，如果工作负荷改动极小，则帐单上显示的增量更改也很小。 有关标准 Azure 文件的定价信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage/files/)。 目前，查看共享快照已用大小的方法是比较计费的容量与使用的容量。 我们致力于开发改进报告的工具。

* <a id="ntfs-acls-snaphsots"></a>
**目录和文件上的 NTFS ACL 是否保留在共享快照中？**  
    目录和文件上的 NTFS ACL 会保留在共享快照中。

### <a name="create-share-snapshots"></a>创建共享快照
* <a id="file-snaphsots"></a>
**是否可以创建单个文件的共享快照？**  
    可在文件共享级别上创建共享快照。 你可以从文件共享快照还原单个文件，但是不能创建文件级别的共享快照。 但是，如果你已执行共享级别的共享快照，并且想要列出已更改特定文件的共享快照，则可以在已装载 Windows 的共享上的“以前的版本”下执行此操作。 

* <a id="encrypted-snapshots"></a>
**是否可以创建加密文件共享的共享快照？**  
    可以对已启用静态加密的 Azure 文件共享执行共享快照。 可以从共享快照中将文件还原到加密文件共享中。 如果共享已加密，则共享快照也会加密。

* <a id="geo-redundant-snaphsots"></a>
**共享快照具有异地冗余吗？**  
    共享快照与捕获它们的 Azure 文件共享具有相同的冗余。 如果已为帐户选择异地冗余存储，则共享快照也会在配对区域中进行冗余存储。

### <a name="manage-share-snapshots"></a>管理共享快照
* <a id="browse-snapshots-linux"></a>
**是否可以在 Linux 中浏览共享快照？**  
    可以使用 Azure CLI 在 Linux 中创建、列出、浏览和还原共享快照。

* <a id="copy-snapshots-to-other-storage-account"></a>
**是否可以将共享快照复制到不同的存储帐户？**  
    可以将文件从共享快照复制到其他位置，但不能复制到共享快照本身。

### <a name="restore-data-from-share-snapshots"></a>从共享快照还原数据
* <a id="promote-share-snapshot"></a>
**是否可以将共享快照提升到基本共享中？**  
    只能将数据从共享快照复制到任何其他目标位置， 不能将共享快照提升到基本共享中。

* <a id="restore-snapshotted-file-to-other-share"></a>
**是否可以将数据从共享快照还原到不同的存储帐户？**  
    是的。 可以将共享快照文件复制到原始位置或备用位置，其中包括位于同一区域或不同区域的相同/不同的存储帐户。 你还可以将文件复制到本地位置或任何其他云。    
  
### <a name="clean-up-share-snapshots"></a>清除共享快照
* <a id="delete-share-keep-snapshots"></a>
**是否可以删除共享，而不删除共享快照？**  
    如果共享上有活动的共享快照，则无法删除此共享。 你可以使用一个 API 将共享快照连同共享一起删除。 此外，也可以在 Azure 门户中删除共享快照和共享。

* <a id="delete-share-with-snapshots"></a>
**如果删除存储帐户，共享快照会怎样？**  
    删除存储帐户后，共享快照也将被删除。

## <a name="billing-and-pricing"></a>计费和定价
* <a id="vm-file-share-network-traffic"></a>
**Azure VM 与 Azure 文件共享之间的网络流量是否作为外部带宽计入订阅费用？**  
    如果文件共享和 VM 位于同一 Azure 区域，则它们之间的流量是不会额外收费的。 如果文件共享和 VM 位于不同的区域，则它们之间的流量会作为外部带宽收费。

* <a id="share-snapshot-price"></a>
**共享快照的费用是多少？**  
     在预览版期间，共享快照容量可免费使用， 但会收取标准存储出口和事务费用。 公开发布后，共享快照的容量和事务均将收费。
     
     共享快照在本质上是递增的。 基本共享快照即是共享本身。 所有的后续共享快照均是递增的，并且只会存储与之前共享快照的不同之处。 你只需为更改的内容付费。 如果你的共享包含 100 GiB 数据，但自执行上次共享快照以来只更改了 5 GiB 数据，则共享快照只额外使用了 5 GiB 数据，而你要为 105 GiB 付费。 有关事务和标准出口费用的更多信息，请参阅[定价页](https://azure.cn/pricing/details/storage/files/)。

## <a name="scale-and-performance"></a>缩放和性能
* <a id="files-scale-limits"></a>
**Azure 文件存在哪些缩放限制？**  
    有关 Azure 文件的可伸缩性和性能目标的信息，请参阅 [Azure 文件可伸缩性和性能目标](storage-files-scale-targets.md)。

* <a id="need-larger-share"></a>
**Azure 文件共享的可用大小有哪些？**  
    Azure 文件共享大小（高级和标准）最多可扩展到 100 TiB。 请参阅规划指南的[加入到较大文件共享（标准层）](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib)部分，以获取加入到标准层较大文件共享的说明。

* <a id="open-handles-quota"></a>
**多少个客户端可以同时访问同一文件？**    
    单个文件有 2000 个打开句柄配额。 当你拥有 2000 个打开句柄时，会显示一条错误消息，指示已达到此配额。

* <a id="zip-slow-performance"></a>
**解压 Azure 文件中的文件时性能较慢。我该怎样做？**  
    若要将大量文件传输到 Azure 文件，建议使用 AzCopy（Windows 版；Linux 和 Unix 预览版）或 Azure Powershell。 这些工具已针对网络传输进行了优化。

* <a id="slow-perf-windows-81-2012r2"></a>
**为什么在 Windows Server 2012 R2 或 Windows 8.1 上装载了 Azure 文件共享后性能较慢？**  
    在 Windows Server 2012 R2 和 Windows 8.1 上装载 Azure 文件共享后，有一个已知的问题。 在 2014 年 4 月的 Windows 8.1 和 Windows Server 2012 R2 累积更新中已修补了此问题。 请确保 Windows Server 2012 R2 和 Windows 8.1 的所有实例均应用了此修补程序，以获得最佳性能。 （你应始终通过 Windows 更新获取 Windows 修补程序。）有关详细信息，请查看相关的 Azure 识库文章：[从 Windows 8.1 或 Server 2012 R2 访问 Azure 文件时性能降低](https://support.microsoft.com/kb/3114025)。

## <a name="features-and-interoperability-with-other-services"></a>功能以及与其他服务的互操作性
* <a id="cluster-witness"></a>
**是否可以将 Azure 文件共享作为 Windows 服务器故障转移群集的文件共享见证？**  
    Azure 文件共享目前不支持此配置。 有关如何为 Azure Blob 存储设置此服务的详细信息，请参阅[部署故障转移群集的云见证](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。


* <a id="rest-rename"></a>
**REST API 中是否有重命名操作？**  
    目前没有。

* <a id="nested-shares"></a>
**是否可以设置嵌套共享？也就是说，能否在共享下使用共享？**  
    不是。 文件共享是可以装载的虚拟驱动程序，因此不支持嵌套共享。

* <a id="ibm-mq"></a>
**如何将 Azure 文件与 IBM MQ 配合使用？**  
    IBM 已发布相关文档，帮助 IBM MQ 客户通过 IBM 服务配置 Azure 文件。 有关详细信息，请参阅 [How to set up an IBM MQ multi-instance queue manager with  Azure Files service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)（如何通过 Azure 文件服务设置 IBM MQ 多实例队列管理器）。

## <a name="see-also"></a>另请参阅
* [在 Windows 中排查 Azure 文件问题](storage-troubleshoot-windows-file-connection-problems.md)
* [在 Linux 中排查 Azure 文件问题](storage-troubleshoot-linux-file-connection-problems.md)
