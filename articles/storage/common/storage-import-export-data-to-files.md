---
title: 使用 Azure 导入/导出将数据传输到 Azure 文件 | Microsoft Docs
description: 了解如何在 Azure 门户中创建导入作业，以便将数据传输到 Azure 文件。
author: WenJason
services: storage
ms.service: storage
ms.topic: how-to
origin.date: 10/29/2020
ms.date: 11/30/2020
ms.author: v-jay
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6784f79cfd95d7e71f7e25eac4e1abc2867731f7
ms.sourcegitcommit: a978c5f2c6b53494d67e7c3c5a44b2aa648219a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629070"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>使用 Azure 导入/导出服务将数据导入到 Azure 文件

本文提供了有关如何使用 Azure 导入/导出服务安全地将大量数据导入到 Azure 文件的分步说明。 若要导入数据，此服务要求你将包含数据的受支持磁盘驱动器寄送到某个 Azure 数据中心。

导入/导出服务仅支持将 Azure 文件导入到 Azure 存储。 不支持导出 Azure 文件。

## <a name="prerequisites"></a>先决条件

在创建导入作业来将数据传输到 Azure 文件之前，请仔细查看并完成以下先决条件列表。 必须具备以下条件：

- 拥有可以用于导入/导出服务的活动 Azure 订阅。
- 拥有至少一个 Azure 存储帐户。 请参阅[导入/导出服务支持的存储帐户和存储类型](storage-import-export-requirements.md)的列表。 有关创建新存储帐户的信息，请参阅[如何创建存储帐户](storage-account-create.md)。
- 拥有足够数量的[受支持类型](storage-import-export-requirements.md#supported-disks)的磁盘。
- 拥有运行[受支持 OS 版本](storage-import-export-requirements.md#supported-operating-systems)的 Windows 系统。
- 在 Windows 系统上[下载 WAImportExport 版本 2](https://aka.ms/waiev2)。 解压缩到默认文件夹 `waimportexport`。 例如，`C:\WaImportExport`。


## <a name="step-1-prepare-the-drives"></a>步骤 1：准备驱动器

此步骤生成一个日志文件。 日志文件存储着驱动器序列号、加密密钥和存储帐户详细信息等基本信息。

请执行以下步骤来准备驱动器。

1. 通过 SATA 连接器将磁盘驱动器连接到 Windows 系统。
2. 在每个驱动器上创建一个 NTFS 卷。 为卷分配驱动器号。 不要使用装入点。
3. 修改工具所在的根文件夹中的 *dataset.csv* 文件。 根据是要导入文件还是文件夹还是同时导入两者，在 *dataset.csv* 文件中添加类似于以下示例的条目。

   - **导入文件**：在以下示例中，要复制的数据位于 F: 驱动器中。 文件 *MyFile1.txt* 将被复制到根目录 *MyAzureFileshare1* 中。 如果 *MyAzureFileshare1* 不存在，则会在 Azure 存储帐户中创建该目录。 文件夹结构保持不变。

       ```
           BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None

       ```
   - **导入文件夹**：*MyFolder2* 下的所有文件和文件夹将以递归方式复制到该文件共享。 文件夹结构保持不变。

       ```
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None

       ```
     可以在同一文件中创建与导入的文件夹或文件对应的多个条目。

       ```
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None

       ```
     详细了解如何[准备数据集 CSV 文件](https://docs.microsoft.com/previous-versions/azure/storage/common/storage-import-export-tool-preparing-hard-drives-import)。


4. 修改工具所在的根文件夹中的 *driveset.csv* 文件。 在 *driveset.csv* 文件中添加类似于以下示例的条目。 此驱动器集文件包含磁盘列表和对应的驱动器号，因此，工具可以正确地选取要准备的磁盘列表。

    此示例假定将附加两个磁盘并创建基本 NTFS 卷 G:\ 和 H:\。 H:\ 未加密，而 G: 已加密。 该工具仅会对承载着 H:\（不会对承载着 G:\)）的磁盘进行格式化和加密。

   - **对于未加密的磁盘**：请指定 *Encrypt* 以在磁盘上启用 BitLocker 加密。

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       H,Format,SilentMode,Encrypt,
       ```

   - **对于已加密的磁盘**：请指定 *AlreadyEncrypted* 并提供 BitLocker 密钥。

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
       ```

     可以在同一文件中创建与多个驱动器对应的多个条目。 详细了解如何[准备驱动器集 CSV 文件](https://docs.microsoft.com/previous-versions/azure/storage/common/storage-import-export-tool-preparing-hard-drives-import)。

5. 使用 `PrepImport` 选项将数据复制到磁盘驱动器并做好准备。 为了使第一个复制会话通过新的复制会话复制目录和/或文件，请运行以下命令：

    ```cmd
    .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>]/DataSet:<dataset.csv>
    ```

   下面显示了一个导入示例。

    ```cmd
    .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
    ```

6. 每次运行该命令行时，都会使用通过 `/j:` 参数提供名称创建一个日志文件。 你准备的每个驱动器都有一个在创建导入作业时必须上传的日志文件。 不会处理没有日志文件的驱动器。

    > [!IMPORTANT]
    > - 完成磁盘准备工作后，请不要改磁盘驱动器上的数据，也不要修改日志文件。

有关更多示例，请转到[日志文件示例](#samples-for-journal-files)。

## <a name="step-2-create-an-import-job"></a>步骤 2：创建导入作业

### <a name="portal"></a>[Portal](#tab/azure-portal)

在 Azure 门户中执行以下步骤来创建导入作业。
1. 登录到 https://portal.azure.cn/ 。
2. 转到“所有服务”>“存储”>“导入/导出作业”。

    ![转到“导入/导出”](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. 单击“创建导入/导出作业”。

    ![单击导入/导出作业](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. 在“基本信息”中：

    - 选择“导入到 Azure”。
    - 输入导入作业的描述性名称。 在作业进行过程中以及作业完成后，可以使用此名称来跟踪作业。
        -  此名称只能包含小写字母、数字、连字符和下划线。
        -  此名称必须以字母开头，并且不得包含空格。
    - 选择一个订阅。
    - 选一个择资源组。

        ![创建导入作业 - 步骤 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. 在“作业详细信息”中：

    - 上传你在前面的[步骤 1：准备驱动器](#step-1-prepare-the-drives)中创建的日志文件。
    - 选择要将数据导入到的存储帐户。
    - 放置位置会根据选定存储帐户所属的区域自动进行填充。

       ![创建导入作业 - 步骤 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. 在“回寄信息”中：

    - 从下拉列表中选择承运商。
    - 输入你已在该承运商那里创建的有效承运商帐户编号。 当导入作业完成后，我们使用此帐户寄回驱动器。 
    - 提供完整、有效的联系人姓名、电话号码、电子邮件地址、街道地址、城市、邮政编码、省/自治区/直辖市和国家/地区。

        > [!TIP]
        > 请提供组电子邮件，而非为单个用户指定电子邮件地址。 这可确保即使管理员离开也会收到通知。

       ![创建导入作业 - 步骤 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)


5. 在“摘要”中：

    - 提供用来将磁盘寄回 Azure 的 Azure 数据中心送货地址。 请确保寄送标签上标明了作业名称和完整地址。
    - 单击“确定”以完成导入作业创建。

        ![创建导入作业 - 步骤 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

使用以下步骤在 Azure CLI 中创建导入作业。

[!INCLUDE [azure-cli-prepare-your-environment-h3.md](../../../includes/azure-cli-prepare-your-environment-h3.md)]

### <a name="create-a-job"></a>创建作业

1. 使用 [az extension add](/cli/extension#az_extension_add) 命令添加 [az import-export](/cli/ext/import-export/import-export) 扩展：

    ```azurecli
    az extension add --name import-export
    ```

1. 可以使用现有资源组，也可以创建新组。 若要创建资源组，请运行 [az group create](/cli/group#az_group_create) 命令：

    ```azurecli
    az group create --name myierg --location "China East"
    ```

1. 可以使用现有存储帐户，也可以创建一个存储帐户。 若要创建存储帐户，请运行 [az storage account create](/cli/storage/account#az_storage_account_create) 命令：

    ```azurecli
    az storage account create -resource-group myierg -name myssdocsstorage --https-only
    ```

1. 若要获取可将磁盘寄送到的位置的列表，请使用 [az import-export location list](/cli/ext/import-export/import-export/location#ext_import_export_az_import_export_location_list) 命令：

    ```azurecli
    az import-export location list
    ```

1. 使用 [az import-export location show](/cli/ext/import-export/import-export/location#ext_import_export_az_import_export_location_show) 命令获取你所在区域的位置：

    ```azurecli
    az import-export location show --location "China East"
    ```

1. 运行以下 [az import-export create](/cli/ext/import-export/import-export#ext_import_export_az_import_export_create) 命令来创建导入作业：

    ```azurecli
    az import-export create \
        --resource-group myierg \
        --name MyIEjob1 \
        --location "China East" \
        --backup-drive-manifest true \
        --diagnostics-path waimportexport \
        --drive-list bit-locker-key=439675-460165-128202-905124-487224-524332-851649-442187 \
            drive-header-hash= drive-id=AZ31BGB1 manifest-file=\\DriveManifest.xml \
            manifest-hash=69512026C1E8D4401816A2E5B8D7420D \
        --type Import \
        --log-level Verbose \
        --shipping-information recipient-name="Microsoft Azure Import/Export Service" \
            street-address1="XXXX XXXX" city="XXXX XXXX" state-or-province=CA postal-code=XXXXXX \
            country-or-region=China phone=XXXXXXXX \
        --return-address recipient-name="XXXX XXXX" street-address1="XXXX XXXX XXXX" \
            city=XXXX country-or-region=China state-or-province=CA postal-code=XXXXXX \
            email=gus@contoso.com phone=XXXXXXXX" \
        --return-shipping carrier-name=EMS carrier-account-number=XXXXXXXX \
        --storage-account myssdocsstorage
    ```

   > [!TIP]
   > 请提供组电子邮件，而非为单个用户指定电子邮件地址。 这可确保即使管理员离开也会收到通知。


1. 使用 [az import-export list](/cli/ext/import-export/import-export#ext_import_export_az_import_export_list) 命令查看 myierg 资源组的所有作业：

    ```azurecli
    az import-export list --resource-group myierg
    ```

1. 若要更新作业或取消作业，请运行 [az import-export update](/cli/ext/import-export/import-export#ext_import_export_az_import_export_update) 命令：

    ```azurecli
    az import-export update --resource-group myierg --name MyIEjob1 --cancel-requested true
    ```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

使用以下步骤在 Azure PowerShell 中创建导入作业。

[!INCLUDE [azure-powershell-requirements-h3.md](../../../includes/azure-powershell-requirements-h3.md)]

> [!IMPORTANT]
> 尽管 Az.ImportExport PowerShell 模块为预览版，但你需要使用 `Install-Module` cmdlet 单独安装它。 

```azurepowershell
Install-Module -Name Az.ImportExport
```

### <a name="create-a-job"></a>创建作业

1. 可以使用现有资源组，也可以创建新组。 若要创建资源组，请运行 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) cmdlet：

   ```azurepowershell
   New-AzResourceGroup -Name myierg -Location chinaeast
   ```

1. 可以使用现有存储帐户，也可以创建一个存储帐户。 若要创建存储帐户，请运行 [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet：

   ```azurepowershell
   New-AzStorageAccount -ResourceGroupName myierg -AccountName myssdocsstorage -SkuName Standard_RAGRS -Location chinaeast -EnableHttpsTrafficOnly $true
   ```

1. 若要获取可将磁盘寄送到的位置的列表，请使用 [Get-AzImportExportLocation](https://docs.microsoft.com/powershell/module/az.importexport/get-azimportexportlocation) cmdlet：

   ```azurepowershell
   Get-AzImportExportLocation
   ```

1. 使用带有 `Name` 参数的 `Get-AzImportExportLocation` cmdlet 获取区域的位置：

   ```azurepowershell
   Get-AzImportExportLocation -Name chinaeast
   ```

1. 运行以下 [New-AzImportExport](https://docs.microsoft.com/powershell/module/az.importexport/new-azimportexport) 示例来创建导入作业：

   ```azurepowershell
   $driveList = @(@{
     DriveId = '9CA995BA'
     BitLockerKey = '439675-460165-128202-905124-487224-524332-851649-442187'
     ManifestFile = '\\DriveManifest.xml'
     ManifestHash = '69512026C1E8D4401816A2E5B8D7420D'
     DriveHeaderHash = 'AZ31BGB1'
   })

   $Params = @{
      ResourceGroupName = 'myierg'
      Name = 'MyIEjob1'
      Location = 'chinaeast'
      BackupDriveManifest = $true
      DiagnosticsPath = 'waimportexport'
      DriveList = $driveList
      JobType = 'Import'
      LogLevel = 'Verbose'
      ShippingInformationRecipientName = 'Microsoft Azure Import/Export Service'
      ShippingInformationStreetAddress1 = 'XXXX XXXX'
      ShippingInformationCity = 'XXXX XXXX'
      ShippingInformationStateOrProvince = 'CA'
      ShippingInformationPostalCode = 'XXXXXX'
      ShippingInformationCountryOrRegion = 'China'
      ShippingInformationPhone = '1234567890'
      ReturnAddressRecipientName = 'XXXX XXXX'
      ReturnAddressStreetAddress1 = 'XXXX XXXX'
      ReturnAddressCity = 'XXXX'
      ReturnAddressStateOrProvince = 'CA'
      ReturnAddressPostalCode = 'XXXXXX'
      ReturnAddressCountryOrRegion = 'China'
      ReturnAddressPhone = '1234567890'
      ReturnAddressEmail = 'gus@contoso.com'
      ReturnShippingCarrierName = 'EMS'
      ReturnShippingCarrierAccountNumber = '123456789'
      StorageAccountId = '/subscriptions/<SubscriptionId>/resourceGroups/myierg/providers/Microsoft.Storage/storageAccounts/myssdocsstorage'
   }
   New-AzImportExport @Params
   ```

   > [!TIP]
   > 请提供组电子邮件，而非为单个用户指定电子邮件地址。 这可确保即使管理员离开也会收到通知。

1. 使用 [Get-AzImportExport](https://docs.microsoft.com/powershell/module/az.importexport/get-azimportexport) cmdlet 查看 myierg 资源组的所有作业：

   ```azurepowershell
   Get-AzImportExport -ResourceGroupName myierg
   ```

1. 若要更新作业或取消作业，请运行 [Update-AzImportExport](https://docs.microsoft.com/powershell/module/az.importexport/update-azimportexport) cmdlet：

   ```azurepowershell
   Update-AzImportExport -Name MyIEjob1 -ResourceGroupName myierg -CancelRequested
   ```

---

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>步骤 3：将驱动器寄送到 Azure 数据中心

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>步骤 4：使用跟踪信息更新作业

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>步骤 5：验证数据上传到 Azure

跟踪作业直至完成。 作业完成后，验证数据已上传到 Azure。 仅在已确认上传成功后才删除本地数据。

## <a name="samples-for-journal-files"></a>日志文件示例

若要 **添加更多驱动器**，请创建一个新的驱动器集文件并运行以下命令。

如果后续复制会话中的磁盘驱动器与 *InitialDriveset .csv* 文件中指定的不同，请指定一个新的驱动器集 *.csv* 文件并将其提供为参数 `AdditionalDriveSet` 的值。 使用 **同一日记文件** 的名称并提供 **新的会话 ID**。 AdditionalDriveset CSV 文件的格式与 InitialDriveSet 的格式相同。

```cmd
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

下面显示了一个导入示例。

```cmd
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```


若要向同一驱动器集添加更多数据，请为后续复制会话使用 PrepImport 命令来复制更多文件/目录。

在后续复制会话中将数据复制到 *InitialDriveset.csv* 文件中指定的同一组硬盘驱动器时，请指定 **同一日志文件** 名称并提供 **新的会话 ID**；不需要提供存储帐户密钥。

```cmd
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

下面显示了一个导入示例。

```cmd
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

## <a name="next-steps"></a>后续步骤

* [查看作业和驱动器状态](storage-import-export-view-drive-status.md)
* [查看导入/导出要求](storage-import-export-requirements.md)
