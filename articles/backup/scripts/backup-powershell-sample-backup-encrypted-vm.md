---
title: PowerShell 脚本示例 - 备份 Azure VM
description: 本文介绍如何使用 Azure PowerShell 脚本示例备份 Azure 虚拟机。
ms.topic: sample
ms.author: v-johya
ms.date: 01/21/2021
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 0f14615701be94d3bc06cdf49897690731d8f2b1
ms.sourcegitcommit: 102a21dc30622e4827cc005bdf71ade772c1b8de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98751251"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>使用 PowerShell 备份已加密 Azure 虚拟机

此脚本为已加密 Azure 虚拟机创建包含异地冗余存储 (GRS) 的恢复服务保管库。 默认保护策略已应用到此保管库。 此策略为虚拟机生成每日备份，并将每个备份保留 365 天。 该脚本还将触发虚拟机的初始恢复点，并将该恢复点保留 30 天。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
# Edit these global variables with your unique Recovery Services Vault name, resource group name and location
$rsVaultName = "myRsVault"
$rgName = "myResourceGroup"
$location = "China North"

# Register the Recovery Services provider and create a resource group
Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
New-AzResourceGroup -Location $location -Name $rgName

# Create a Recovery Services Vault and set its storage redundancy type
New-AzRecoveryServicesVault `
    -Name $rsVaultName `
    -ResourceGroupName $rgName `
    -Location $location 
$vault1 = Get-AzRecoveryServicesVault â€“Name $rsVaultName
Set-AzRecoveryServicesProperties ` 
    -Vault $vault1 `
    -BackupStorageRedundancy GeoRedundant
    
# Set Recovery Services Vault context and create protection policy
Get-AzRecoveryServicesVault -Name $rsVaultName | Set-AzRecoveryServicesVaultContext 
$schPol = Get-AzRecoveryServicesSchedulePolicyObject -WorkloadType "AzureVM"
$retPol = Get-AzRecoveryServicesRetentionPolicyObject -WorkloadType "AzureVM"
New-AzRecoveryServicesProtectionPolicy `
    -Name "NewPolicy" `
    -WorkloadType "AzureVM" ` 
    -RetentionPolicy $retPol `
    -SchedulePolicy $schPol
    
# Provide permissions to Azure Backup to access key vault and enable backup on the VM
Set-AzKeyVaultAccessPolicy `
    -VaultName "KeyVaultName" `
    -ResourceGroupName "KyeVault-RGName" ` 
    -PermissionsToKeys backup,get,list `
    -PermissionsToSecrets backup,get,list ` 
    -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesProtectionPolicy -Name "NewPolicy" `
Enable-AzRecoveryServicesProtection `
    -Policy $pol `
    -Name "myVM" `
    -ResourceGroupName "VM-RGName" 
    
# Modify protection policy
$retPol = Get-AzRecoveryServicesRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzRecoveryServicesProtectionPolicy -Name "NewPolicy"
Set-AzRecoveryServicesProtectionPolicy `
    -Policy $pol `
    -RetentionPolicy $RetPol
    
# Trigger a backup and monitor backup job
$namedContainer = Get-AzRecoveryServicesContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "myVM"
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
$job = Backup-AzRecoveryServicesBackupItem -Item $item
$joblist = Get-AzRecoveryServicesJob â€“Status "InProgress"
Wait-AzRecoveryServicesJob `
        -Job $joblist[0] `
        -Timeout 43200
```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) | 创建用于存储备份的恢复服务保管库。 |
| [Set-AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) | 设置恢复服务保管库的备份存储属性。 |
| [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| 在恢复服务保管库中使用计划策略和保留策略创建保护策略。 |
| [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | 设置对 Key Vault 的权限来向服务主体授予对加密密钥的访问权限。 |
| [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) | 使用指定的备份保护策略为某一项启用备份。 |
| [Set-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| 修改现有备份保护策略。 |
| [Backup-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) | 为未绑定到备份计划的受保护 Azure 备份项启动备份。 |
| [Wait-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) | 等待 Azure 备份作业完成。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)。

