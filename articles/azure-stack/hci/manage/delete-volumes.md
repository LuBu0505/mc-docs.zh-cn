---
title: 在 Azure Stack HCI 中删除卷
description: 如何使用 Windows 管理中心和 PowerShell 删除 Azure Stack HCI 中的卷。
author: WenJason
ms.author: v-jay
ms.topic: how-to
origin.date: 07/21/2020
ms.date: 03/22/2021
ms.openlocfilehash: 1152e7478e2bd5552b65decab54f28e6b89a8ba5
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104768150"
---
# <a name="deleting-volumes-in-azure-stack-hci"></a>在 Azure Stack HCI 中删除卷

> 适用于：Azure Stack HCI 版本 20H2；Windows Server 2019

本主题说明如何使用 Windows Admin Center 在 Azure Stack HCI 群集上删除卷。

## <a name="use-windows-admin-center-to-delete-a-volume"></a>使用 Windows Admin Center 删除卷

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。 
2. 在“卷”页上选择“清单”选项卡，然后选择要删除的卷。
3. 在卷详细信息页的顶部，选择“删除”。
4. 在“确认”对话框中，选中复选框以确认要删除该卷，然后选择“删除”。

## <a name="delete-volumes-using-powershell"></a>使用 PowerShell 删除卷

使用 Remove-VirtualDisk cmdlet 删除存储空间直通中的卷。 此 cmdlet 用于删除 VirtualDisk 对象，并将其使用的空间返回给公开了 VirtualDisk 对象的存储池。

首先，在管理电脑上启动 PowerShell，并运行带有 CimSession 参数（即存储空间直通群集或服务器节点的名称，例如 clustername.microsoft.com）的 Get-VirtualDisk cmdlet：

```PowerShell
Get-VirtualDisk -CimSession clustername.microsoft.com
```

这将返回 -FriendlyName 参数的可能值列表，这些值对应于群集上的卷名。

### <a name="example"></a>示例

若要删除名为“Volume1”的镜像卷，请在 PowerShell 中运行以下命令：

```PowerShell
Remove-VirtualDisk -FriendlyName "Volume1"
```

系统会要求你确认是否要执行该操作并清除该卷包含的所有数据。 选择“Y”或“N”。

   > [!WARNING]
   > 此操作不可恢复。 此示例会永久删除 VirtualDisk 卷对象。

## <a name="next-steps"></a>后续步骤

如需其他重要的存储管理任务的分步说明，另请参阅：

- [规划卷](../concepts/plan-volumes.md)
- [创建卷](create-volumes.md)
- [扩展卷](extend-volumes.md)