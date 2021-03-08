---
title: 使用 Azure Desired State Configuration 扩展版本历史记录
description: 本文分享了 Azure Desired State Configuration (DSC) 扩展的版本历史记录信息。
origin.date: 02/17/2021
ms.date: 02/22/2021
keywords: dsc, powershell, azure, 扩展
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.openlocfilehash: 06d99ee90bcb06e217dbb262c32b793ae1be3fdd
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697084"
---
# <a name="work-with-azure-desired-state-configuration-extension-version-history"></a>使用 Azure Desired State Configuration 扩展版本历史记录

已根据需要更新 Azure Desired State Configuration (DSC) VM [扩展](../virtual-machines/extensions/dsc-overview.md)，以支持 Azure、Windows Server 和 Windows Management Framework (WMF)（包含 Windows PowerShell）提供的增强功能和新功能。

本文介绍了 Azure DSC VM 扩展的各个版本及其支持的环境，并说明了新功能或变化。

## <a name="latest-version"></a>最新版本

### <a name="version-283"></a>版本 2.83

- **发布日期：**
  - 2021 年 2 月
- **OS 支持：**
  - Windows Server 2019
  - Windows Server 2016
  - Windows Server 2012 R2
  - Windows Server 2012
  - Windows Server 2008 R2 SP1
  - Windows 客户端 7/8.1/10
  - Nano Server
- **WMF 支持：**
  - WMF 5.1
  - WMF 5.0 RTM
  - WMF 4.0 更新
  - WMF 4.0
- **环境：**
  - Azure 中国世纪互联

### <a name="version-280"></a>版本 2.80

- **发布日期：**
  - 2020 年 7 月 6 日（Azure 中国世纪互联）
- **OS 支持：**
  - Windows Server 2019
  - Windows Server 2016
  - Windows Server 2012 R2
  - Windows Server 2012
  - Windows Server 2008 R2 SP1
  - Windows 客户端 7/8.1/10
  - Nano Server
- **WMF 支持：**
  - WMF 5.1
  - WMF 5.0 RTM
  - WMF 4.0 更新
  - WMF 4.0
- **环境：**
  - Azure 中国世纪互联
- **备注：** 此版本中不包含任何新增功能。

### <a name="version-276"></a>版本 2.76

- **发布日期：**
  - 2018 年 6 月 21 日（Azure 中国世纪互联）
- **OS 支持：**
  - Windows Server 2016
  - Windows Server 2012 R2
  - Windows Server 2012
  - Windows Server 2008 R2 SP1
  - Windows 客户端 7/8.1/10
  - Nano Server
- **WMF 支持：**
  - WMF 5.1
  - WMF 5.0 RTM
  - WMF 4.0 更新
  - WMF 4.0
- **环境：**
  - Azure 中国世纪互联
- **备注：** 此版本使用 Windows Server 2016 中随附的 DSC；对于其他 Windows OS，它会安装 [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/)（安装 WMF 需要重启）。 对于 Nano Server，DSC 角色安装在 VM 上。
- **新功能：**
  - 子状态的扩展元数据改进和其他次要 bug 修复。

## <a name="supported-versions"></a>支持的版本

> [!WARNING]
> 版本 2.4 至 2.13 使用 WMF 5.0 公共预览版，其签名证书在 2016 年 8 月过期。
> 有关此问题的详细信息，请参阅以下[博客文章](https://devblogs.microsoft.com/powershell/azure-dsc-extension-versions-2-4-up-to-2-13-will-retire-in-august/)。

### <a name="version--219"></a>版本 2.19

- **发布日期：** 2016 年 6 月 3 日
- **OS 支持：** Windows Server 2016 Technical Preview、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 SP1
- **WMF 支持：** WMF 5.0 RTM、WMF 4.0 更新、WMF 4.0
- **环境：** Azure 中国世纪互联
- **备注：** 此版本使用 Windows Server 2016 Technical Preview 中随附的 DSC；对于其他 Windows OS，它会安装 [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/)（安装 WMF 需要重启）。

## <a name="next-steps"></a>后续步骤

- 有关 PowerShell DSC 的详细信息，请查看 [PowerShell 文档中心](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview)。
- 查看[适用于 DSC 扩展的资源管理器模板](../virtual-machines/extensions/dsc-template.md)。
- 若要查看可使用 PowerShell DSC 进行管理的其他功能和资源，请浏览 [PowerShell 库](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0)。
- 有关将敏感参数传入配置的详细信息，请参阅[使用 DSC 扩展处理程序安全管理凭据](../virtual-machines/extensions/dsc-credentials.md)。
