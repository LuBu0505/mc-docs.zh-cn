---
title: 备份和还原已加密的 Azure VM
description: 介绍如何使用 Azure 备份服务备份和还原已加密的 Azure VM。
ms.topic: conceptual
author: Johnnytechn
origin.date: 04/03/2019
ms.date: 01/07/2021
ms.author: v-johya
ms.openlocfilehash: aae01cb416b65bed6f0a69145477d0180aff1bff
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023185"
---
# <a name="back-up-and-restore-encrypted-azure-virtual-machines"></a>备份和还原加密的 Azure 虚拟机

本文介绍如何使用 [Azure 备份](backup-overview.md)服务来备份和还原包含已加密磁盘的 Windows 或 Linux Azure 虚拟机 (VM)。 有关详细信息，请参阅 [Azure VM 备份的加密](backup-azure-vms-introduction.md#encryption-of-azure-vm-backups)。

## <a name="encryption-using-platform-managed-keys"></a>使用平台管理的密钥进行加密

默认情况下，VM 中的所有磁盘都会通过使用[存储服务加密](../storage/common/storage-service-encryption.md)的平台管理的密钥 (PMK) 自动进行静态加密。 你可以使用 Azure 备份来备份这些 VM，而无需采取任何支持在你方加密所需的特定措施。 有关使用平台管理的密钥进行加密的详细信息，请[参阅本文](../virtual-machines/disk-encryption.md#platform-managed-keys)。

![加密的磁盘](./media/backup-encryption/encrypted-disks.png)

<!--Not available in MC: ## Encryption using customer-managed keys-->
## <a name="encryption-support-using-ade"></a>使用 ADE 的加密支持

Azure 备份支持备份已使用 Azure 磁盘加密 (ADE) 功能加密了其 OS/数据磁盘的 Azure VM。 ADE 使用 BitLocker 加密 Windows VM，使用 dm-crypt 功能加密 Linux VM。 ADE 与 Azure Key Vault 集成，可以管理磁盘加密密钥和机密。 使用 Key Vault Key 加密密钥 (KEk) 可以额外增加一个安全层，这样可以在将加密机密写入 Key Vault 之前对其进行加密。

Azure 备份可以在使用或者不使用 Azure AD 应用的情况下，通过 ADE 备份和还原 Azure VM。下表提供了相关摘要内容。

**VM 磁盘类型** | **ADE (BEK/dm-crypt)** | **ADE 和 KEK**
--- | --- | ---
**非托管** | 是 | 是
**托管**  | 是 | 是

- 详细了解 [ADE](../security/fundamentals/azure-disk-encryption-vms-vmss.md) 和 [Key Vault](../key-vault/general/overview.md)。
- 有关 Azure VM 磁盘加密，请参阅[常见问题解答](../security/fundamentals/azure-disk-encryption-vms-vmss.md)。

### <a name="limitations"></a>限制

- 可以备份和还原同一订阅与区域中的已加密 VM。
- Azure 备份支持使用独立密钥加密的 VM。 目前不支持属于用于加密 VM 的证书的任何密钥。
- 可以备份和还原恢复服务备份保管库所在的同一订阅与区域中的已加密 VM。
- 无法在文件/文件夹级别恢复已加密的 VM。 需要恢复整个 VM 才能还原文件和文件夹。
- 还原 VM 时，无法对已加密的 VM 的使用[替换现有 VM](backup-azure-arm-restore-vms.md#restore-options) 选项。 只有未加密的托管磁盘才支持此选项。

## <a name="before-you-start"></a>开始之前

开始之前，请执行以下操作：

1. 确保你有一个或多个启用了 ADE 的 [Windows](../virtual-machines/linux/disk-encryption-overview.md) 或 [Linux](../virtual-machines/linux/disk-encryption-overview.md) VM。
2. 查看 Azure VM 备份的[支持矩阵](backup-support-matrix-iaas.md)
3. [创建](backup-create-rs-vault.md)一个恢复服务备份保管库（如果没有）。
4. 如果为已启用备份的 VM 启用加密，则只需为备份服务提供 Key Vault 访问权限，这样，备份就可以继续进行，而不会发生中断。 [详细了解](#provide-permissions)如何分配这些权限。

此外，在某些情况下，还需要完成几项操作：

- 在 VM 上安装 VM 代理：Azure 备份通过为在计算机上运行的 Azure VM 代理安装一个扩展来备份 Azure VM。 如果 VM 是根据 Azure 市场映像创建的，则代理将安装并运行。 如果创建自定义 VM，或迁移本地计算机，则可能需要[手动安装代理](backup-azure-arm-vms-prepare.md#install-the-vm-agent)。

## <a name="configure-a-backup-policy"></a>配置备份策略

1. 如果尚未创建恢复服务备份保管库，请遵照[这些说明](backup-create-rs-vault.md)操作。
1. 在门户中打开保管库，在“概述”部分选择“+备份” 。

    ![备份窗格](./media/backup-azure-vms-encryption/select-backup.png)

1. 在“备份目标” > “工作负荷在哪里运行?”中，选择“Azure”。  
1. 在“要备份哪些内容?”中，选择“虚拟机” 。 然后选择“备份”。

      ![方案窗格](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

1. 在“备份策略” > “选择备份策略”中，选择要与保管库关联的策略。  然后选择“确定”。 
    - 备份策略指定备份创建时间以及这些备份的存储时长。
    - 默认策略的详细信息会在下拉菜单下列出。

    ![选择备份策略](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

1. 如果不想要使用默认策略，请选择“新建”，然后[创建自定义策略](backup-azure-arm-vms-prepare.md#create-a-custom-policy)。

1. 在“虚拟机”下，选择“添加”。

    ![添加虚拟机](./media/backup-azure-vms-encryption/add-virtual-machines.png)

1. 选择要使用所选策略备份的已加密 VM，然后选择“确定”。

      ![选择加密型 VM](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

1. 如果使用的是 Azure Key Vault，则保管库页上会显示一条消息，指出 Azure 备份需要对 Key Vault 中的密钥和机密拥有只读访问权限。

    - 如果收到此消息，不需要执行任何操作。

        ![访问正常](./media/backup-azure-vms-encryption/access-ok.png)

    - 如果收到此消息，需要按[以下过程](#provide-permissions)中所述设置权限。

        ![访问警告](./media/backup-azure-vms-encryption/access-warning.png)

1. 选择“启用备份”以在保管库中部署该备份策略，并为选定的 VM 启用备份。

## <a name="trigger-a-backup-job"></a>触发备份作业

初始备份将根据计划运行，但可以按如下所示立即运行：

1. 在保管库菜单中，选择“备份项”。
2. 在“备份项”中，选择“Azure 虚拟机” 。
3. 在“备份项”列表中，选择省略号 (...)。
4. 选择“立即备份”。
5. 在“立即备份”中，使用日历控件选择恢复点的最后保留日期。 然后选择“确定”。 
6. 监视门户通知。 可以在保管库仪表板 >“备份作业” > “进行中”监视作业进度。  创建初始备份可能需要一些时间，具体取决于 VM 的大小。

## <a name="provide-permissions"></a>提供权限

Azure 备份需要拥有只读访问权限才能备份密钥和机密以及关联的 VM。

- Key Vault 与 Azure 订阅的 Azure AD 租户相关联。 如果你是 **成员用户**，则 Azure 备份需要有权访问 Key Vault，但不需要你执行进一步的操作。
- 如果你是 **来宾用户**，则必须为 Azure 备份提供 Key Vault 访问权限。

设置权限：

1. 在 Azure 门户中，选择“所有服务”并搜索 **Key Vault**。
1. 选择与要备份的已加密 VM 相关联的 Key Vault。
1. 选择“访问策略” > “添加访问策略” 。

    ![添加访问策略](./media/backup-azure-vms-encryption/add-access-policy.png)

1. 在“添加访问策略” > “从模板配置(可选)”中，选择“Azure 备份”。  
    - “密钥权限”和“机密权限”中已预先填充所需的权限。 
    - 如果 VM 是 **仅使用 BEK** 加密的，请删除“密钥权限”对应的选择内容，因为只需要机密的权限。

    ![Azure 备份选择](./media/backup-azure-vms-encryption/select-backup-template.png)

1. 选择 **添加** 。 “备份管理服务”随即会添加到“访问策略”中。 

    ![访问策略](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. 选择“保存”以为 Azure 备份提供权限。

## <a name="restore-an-encrypted-vm"></a>还原已加密的 VM

仅可通过还原 VM 磁盘来还原已加密的 VM，如下所述。 “替换现有项”和“还原 VM”不受支持。 。

按如下所述还原已加密的 VM：

1. [还原 VM 磁盘](backup-azure-arm-restore-vms.md#restore-disks)。
2. 通过执行以下操作之一来重新创建虚拟机实例：
    1. 使用执行还原操作期间生成的模板来自定义 VM 设置，并触发 VM 部署。 [了解详细信息](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm)。
    2. 使用 PowerShell 从已还原的磁盘创建新的 VM。 [了解详细信息](backup-azure-vms-automation.md#create-a-vm-from-restored-disks)。
3. 对于 Linux VM，请重新安装 ADE 扩展，以便打开并装载数据磁盘。

## <a name="next-steps"></a>后续步骤

如果遇到任何问题，请查看以下文章：

- 备份和还原已加密的 Azure VM 时出现的[常见错误](backup-azure-vms-troubleshoot.md)。
- [Azure VM 代理/备份扩展](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)问题。

