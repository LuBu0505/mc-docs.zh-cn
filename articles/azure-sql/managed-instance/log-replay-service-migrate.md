---
title: 使用日志重播服务将数据库迁移到 SQL 托管实例
description: 了解如何使用日志重播服务将数据库从 SQL Server 迁移到 SQL 托管实例
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: WenJason
ms.author: v-jay
ms.reviewer: sstein
origin.date: 02/17/2021
ms.date: 02/22/2021
ms.openlocfilehash: ae367f58c379f3af09d2218433fc5e5dd9a6518c
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751849"
---
# <a name="migrate-databases-from-sql-server-to-sql-managed-instance-using-log-replay-service"></a>使用日志重播服务将数据库从 SQL Server 迁移到 SQL 托管实例
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

本文介绍如何使用日志重播服务 (LRS) 手动配置从 SQL Server 2008-2019 到 SQL 托管实例的数据库迁移。 这是一项在“无恢复”模式下为基于 SQL Server 日志传送技术的托管实例启用的云服务。 在不能使用数据迁移服务 (DM) 的情况下、在需要更多控制的情况下，或者在几乎不能容忍停机的情况下，应使用 LRS。

## <a name="when-to-use-log-replay-service"></a>何时使用日志重播服务

在 [Azure DMS](/dms/tutorial-sql-server-to-managed-instance) 无法用于迁移的情况下，可以通过 PowerShell、CLI cmdlet 或 API 直接使用 LRS 云服务，以便手动生成数据库迁移并将其安排到 SQL 托管实例。 

在下面的某些情况下，你可能需要考虑使用 LRS 云服务：
- 你的数据库迁移项目需要更多控制
- 在进行迁移直接转换时偶尔允许停机
- 无法在你的环境中安装 DMS 可执行文件
- DMS 可执行文件没有对数据库备份的文件访问权限
- 对主机 OS 的访问权限不可用，或没有管理员特权

> [!NOTE]
> 我们建议的将数据库从 SQL Server 迁移到 SQL 托管实例的自动方式是使用 Azure DMS。 此服务将在后端使用相同的 LRS 云服务，在“无恢复”模式下进行日志传送。 当 Azure DMS 不完全支持你的方案时，你应考虑以手动方式使用 LRS 来安排迁移。

## <a name="how-does-it-work"></a>工作原理

生成一个使用 LRS 将数据库迁移到云的自定义解决方案时，需要执行图中显示的和下表中概述的几个业务流程步骤。

迁移需要在 SQL Server 上进行完整的数据库备份，并将备份文件复制到 Azure Blob 存储。 LRS 用于将备份文件从 Azure Blob 存储还原到 SQL 托管实例。 Azure Blob 存储用作 SQL Server 与 SQL 托管实例之间的中间存储。

LRS 会监视 Azure Blob 存储中是否存在任何新的差异备份或日志备份（在还原完整备份后添加的），并会自动还原已添加的任何新文件。 可以使用此服务监视在 SQL 托管实例上还原的备份文件的进度，并可根据需要中止该过程。 在迁移过程中还原的数据库将处于还原模式，在完成此过程之前无法用于读取或写入。

可以在自动完成模式或连续模式下启动 LRS。 在自动完成模式下启动时，如果已还原指定的最后一个备份文件，则迁移会自动完成。 在连续模式下启动时，该服务会连续还原已添加的任何新备份文件，但迁移只会在进行手动直接转换时完成。 最终的直接转换步骤会使数据库可以在 SQL 托管实例上用于读取和写入。 

  ![针对 SQL 托管实例说明的日志重播服务业务流程步骤](./media/log-replay-service-migrate/log-replay-service-conceptual.png)

| Operation | 详细信息 |
| :----------------------------- | :------------------------- |
| 1\.将数据库备份从 SQL Server 复制到 Azure Blob 存储。 | - 使用 [Azcopy](/storage/common/storage-use-azcopy-v10) 或 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)将完整备份、差异备份和日志备份从 SQL Server 复制到 Azure Blob 存储。 <br />- 在迁移多个数据库时，每个数据库都需要一个单独的文件夹。 |
| 2\.在云中启动 LRS 服务。 | - 可通过选择 cmdlet 来启动服务： <br /> PowerShell [start-azsqlinstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/start-azsqlinstancedatabaselogreplay) <br /> CLI [az_sql_midb_log_replay_start cmdlet](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_start)。 <br /><br />- 启动后，该服务会从 Azure Blob 存储中获取备份，并开始在 SQL 托管实例上还原它们。 <br /> - 还原一开始上传的所有备份后，该服务会监视是否有新文件上传到文件夹，并且会持续应用日志（基于 LSN 链），直到该服务停止。 |
| 2\.1.监视操作进度。 | - 可以选择 cmdlet 来监视还原操作的进度： <br /> PowerShell [get-azsqlinstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabaselogreplay) <br /> CLI [az_sql_midb_log_replay_show cmdlet](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_show)。 |
| 2\.2.根据需要停止\中止操作。 | - 如果迁移过程需要中止，则可以选择使用 cmdlet 来停止操作： <br /> PowerShell [stop-azsqlinstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqlinstancedatabaselogreplay) <br /> CLI [az_sql_midb_log_replay_stop](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_stop) cmdlet。 <br /><br />- 这会导致删除 SQL 托管实例上正在还原的数据库。 <br />- LRS 在停止后不能继续用于数据库。 迁移过程需要再次从头开始。 |
| 3\.准备就绪时直接转换到云。 | - 所有备份都已还原到 SQL 托管实例后，通过选择 API 调用或 cmdlet 来启动 LRS 完成操作，以便完成直接转换： <br />PowerShell [complete-azsqlinstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/complete-azsqlinstancedatabaselogreplay) <br /> CLI [az_sql_midb_log_replay_complete](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_complete) cmdlet。 <br /><br />- 这会导致停止 LRS 服务并恢复托管实例上的数据库。 <br />- 将应用程序连接字符串从 SQL Server 重新指向 SQL 托管实例。 <br />- 操作完成后，数据库即可用于云中的读/写操作。 |

## <a name="requirements-for-getting-started"></a>入门要求

### <a name="sql-server-side"></a>SQL Server 端
- SQL Server 2008-2019
- 数据库的完整备份（一个或多个文件）
- 差异备份（一个或多个文件）
- 日志备份（不为事务日志文件进行拆分）
- **必须启用校验和**（强制规定）

### <a name="azure-side"></a>Azure 端
- PowerShell Az.SQL 模块 2.16.0 或更高版本（[安装](https://www.powershellgallery.com/packages/Az.Sql/)）
- CLI 2.19.0 或更高版本（[安装](/cli/install-azure-cli)）
- 已预配 Azure Blob 存储
- 为 Blob 存储生成的只有读取和列出权限的 SAS 安全令牌 

## <a name="best-practices"></a>最佳实践

强烈建议执行以下操作，这是最佳做法：
- 运行[数据迁移助手](https://docs.microsoft.com/sql/dma/dma-overview)来验证数据库是否会正常迁移到 SQL 托管实例。 
- 将完整备份和差异备份拆分为多个文件，而不是一个文件。
- 启用备份压缩。
- 计划在启动 LRS 服务后的 47 小时内完成迁移。

> [!IMPORTANT]
> - 在迁移过程完成之前，不能使用通过 LRS 还原的数据库。 这是因为基础技术在“无恢复”模式下是日志传送。
> - LRS 不支持日志传送的备用模式，因为 SQL 托管实例和最新面市的 SQL Server 版本之间存在版本差异。

## <a name="steps-to-execute"></a>要执行的步骤

## <a name="copy-backups-from-sql-server-to-azure-blob-storage"></a>将备份从 SQL Server 复制到 Azure Blob 存储

通过 LRS 将数据库迁移到托管实例时，可以使用以下两种方法将备份复制到 blob 存储：
- 使用 SQL Server 的原生[备份到 URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url) 功能。
- 使用 [Azcopy](/storage/common/storage-use-azcopy-v10) 或 [Azure 存储资源管理器](https://azure.microsoft.com/en-us/features/storage-explorer)将备份复制到 Blob 容器。 

## <a name="create-azure-blob-and-sas-authentication-token"></a>创建 Azure Blob 和 SAS 身份验证令牌

Azure Blob 存储用作 SQL Server 与 SQL 托管实例之间的备份文件中间存储。 按照以下步骤创建 Azure Blob 存储容器：

1. [创建存储帐户](/storage/common/storage-account-create?tabs=azure-portal)
2. 在存储帐户中[创建 Blob 容器](/storage/blobs/storage-quickstart-blobs-portal)

创建 Blob 容器后，请按照以下步骤生成只具有读取和列出权限的 SAS 身份验证令牌：

1. 使用 Azure 门户访问存储帐户
2. 导航到存储资源管理器
3. 展开“Blob 容器”
4. 右键单击 Blob 容器
5. 选择“获取共享访问签名”
6. 选择令牌到期时间范围。 确保令牌在迁移期间有效。
7. 确保只选中“读取”和“列出”权限
8. 单击“创建”
9. 在 URI 中复制以“sv=”开头的令牌，以便在代码中使用

> [!IMPORTANT]
> Azure Blob 存储的 SAS 令牌的权限只能是“读取”和“列出”。 如果为 SAS 身份验证令牌授予了任何其他权限，则无法启动 LRS 服务。 这些安全要求是设计使然。

## <a name="log-in-to-azure-and-select-subscription"></a>登录到 Azure 并选择订阅

通过以下 PowerShell cmdlet 登录到 Azure：

```powershell
Login-AzAccount -Environment AzureChinaCloud
```

使用以下 PowerShell cmdlet 选择 SQL 托管实例所在的相应订阅：

```powershell
Select-AzSubscription -SubscriptionId <subscription ID>
```

## <a name="start-the-migration"></a>开始迁移

通过启动 LRS 服务来启动迁移。 可以在自动完成模式或连续模式下启动此服务。 在自动完成模式下启动时，如果已还原指定的最后一个备份文件，则迁移会自动完成。 此选项要求 start 命令指定最后一个备份文件的文件名。 在连续模式下启动 LRS 时，该服务会连续还原已添加的任何新备份文件，但迁移只会在进行手动直接转换时完成。 

### <a name="start-lrs-in-autocomplete-mode"></a>在自动完成模式下启动 LRS

若要在自动完成模式下启动 LRS 服务，请使用以下 PowerShell 或 CLI 命令。 指定最后一个备份文件的名称（包含 -LastBackupName 参数）。 还原指定的最后一个备份文件的名称后，服务会自动启动直接转换。

在自动完成模式下启动 LRS - PowerShell 示例：

```powershell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" `
    -StorageContainerUri "https://test.blob.core.chinacloudapi.cn/testing" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D" `
    -AutoComplete -LastBackupName "last_backup.bak"
```

在自动完成模式下启动 LRS - CLI 示例：

```cli
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb -a --last-bn "backup.bak"
    --storage-uri "https://test.blob.core.chinacloudapi.cn/testing"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="start-lrs-in-continuous-mode"></a>在连续模式下启动 LRS

在连续模式下启动 LRS - PowerShell 示例：

```powershell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" -StorageContainerUri "https://test.blob.core.chinacloudapi.cn/testing" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

在连续模式下启动 LRS - CLI 示例：

```cli
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb
    --storage-uri "https://test.blob.core.chinacloudapi.cn/testing"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

> [!IMPORTANT]
> 启动 LRS 后，系统管理的任何软件补丁都会在接下来的 47 小时内停止。 该时段过后，下一个自动化软件补丁会自动停止正在进行的 LRS。 这种情况下无法继续迁移，需要从头重新开始。 

## <a name="monitor-the-migration-progress"></a>监视迁移进度

若要监视迁移操作进度，请使用以下 PowerShell 命令：

```powershell
Get-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

若要监视迁移操作进度，请使用以下 CLI 命令：

```cli
az sql midb log-replay show -g mygroup --mi myinstance -n mymanageddb
```

## <a name="stop-the-migration"></a>停止迁移

如果需要停止迁移，请使用以下 cmdlet。 停止迁移会删除 SQL 托管实例上正在还原的数据库，因此无法继续迁移。

若要停止\中止迁移过程，请使用以下 PowerShell 命令：

```powershell
Stop-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

若要停止\中止迁移过程，请使用以下 CLI 命令：

```cli
az sql midb log-replay stop -g mygroup --mi myinstance -n mymanageddb
```

## <a name="complete-the-migration-continuous-mode"></a>完成迁移（连续模式）

如果在连续模式下启动 LRS，则在确保所有备份都已还原后，启动直接转换就可以完成迁移。 直接转换完成后，就可以迁移数据库并准备进行读写访问。

若要在 LRS 连续模式下完成迁移过程，请使用以下 PowerShell 命令：

```powershell
Complete-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
-InstanceName "ManagedInstance01" `
-Name "ManagedDatabaseName" -LastBackupName "last_backup.bak"
```

若要在 LRS 连续模式下完成迁移过程，请使用以下 CLI 命令：

```cli
az sql midb log-replay complete -g mygroup --mi myinstance -n mymanageddb --last-backup-name "backup.bak"
```

## <a name="next-steps"></a>后续步骤
- 详细了解如何[将 SQL Server 迁移到 SQL 托管实例](../migration-guides/managed-instance/sql-server-to-managed-instance-guide.md)。
- 详细了解 [SQL Server 与 Azure SQL 托管实例之间的差异](transact-sql-tsql-differences-sql-server.md)。
