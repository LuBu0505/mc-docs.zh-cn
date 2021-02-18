---
title: 排查 Azure 中共享映像的问题
description: 了解如何排查共享映像库的问题。
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: troubleshooting
ms.workload: infrastructure
origin.date: 10/27/2020
author: rockboyfor
ms.date: 01/04/2021
ms.testscope: no
ms.testdate: 07/06/2020
ms.author: v-yeche
ms.reviewer: cynthn
ms.openlocfilehash: 0ded2968266f059c392e818a7211207b54498be1
ms.sourcegitcommit: b4fd26098461cb779b973c7592f951aad77351f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2021
ms.locfileid: "97857140"
---
<!--Verified successfully-->
<!--Content from released includes file-->
# <a name="troubleshoot-shared-image-galleries-in-azure"></a>排查 Azure 中共享映像库的问题

如果在对共享映像库、映像定义和映像版本执行任何操作时遇到问题，请在调试模式下再次运行失败的命令。 在 Azure CLI 中，请通过传递 `--debug` 开关激活调试模式；在 PowerShell 中，请通过传递 `-Debug` 开关激活调试模式。 找到错误以后，请按本文的说明来排查错误。

## <a name="creating-or-modifying-a-gallery"></a>创建或修改库 ##

库名称无效。允许的字符为英文字母数字字符（带下划线），句点必须位于中间，总共最多 80 个字符。所有其他的特殊字符（包括短划线）都不允许使用。  
**原因：** 库名不符合命名要求。  
**解决方法**：选择满足以下条件的名称： 
- 存在 80 个字符的限制
- 只包含英文字母、数字、下划线和句点
- 以英文字母或数字开头和结尾

*根据验证规则，实体名称 "galleryName" 无效: ^[^\_\W][\w-.\_]{0,79}(?<![-.])$。*  
**原因：** 库名不符合命名要求。  
**解决方法**：选择满足以下条件的库名： 
- 存在 80 个字符的限制
- 只包含英文字母、数字、下划线和句点
- 以英文字母或数字开头和结尾

*提供的资源名称 <galleryName\> 的以下尾随字符无效: <character\>。名称不能以以下字符结尾: <character\>*  
**原因：** 库名以句点或下划线结尾。  
**解决方法**：选择满足以下条件的库名： 
- 存在 80 个字符的限制
- 只包含英文字母、数字、下划线和句点
- 以英文字母或数字开头和结尾

*提供的位置 <region\> 不适用于资源类型 "Microsoft.Compute/galleries"。资源类型的可用区域列表为 �*  
**原因：** 为库指定的区域不正确，或者该区域需要一个访问请求。  
**解决方法**：检查区域名称拼写是否正确。 可以运行此命令来查看你有权访问哪些区域。 如果区域不在列表中，请提交[访问请求](https://docs.microsoft.com/troubleshoot/azure/general/region-access-request-process)。

删除嵌套资源前无法删除资源。  
**原因：** 你已尝试删除至少包含一个现有映像定义的库。 库必须为空后才能删除。  
**解决方法**：删除库中的所有映像定义，然后删除库。 如果映像定义包含映像版本，则必须先删除映像版本，然后才能删除映像定义。

资源 <galleryName\> 已存在于资源组 <resourceGroup\> 中的位置 <region\_1\>。不能在位置 <region\_2\> 创建同名的资源。请选择新的资源名称。  
**原因：** 资源组中已存在同名的现有库，而你已尝试在另一区域中创建另一个同名的库。  
**解决方法**：使用其他库或资源组。

## <a name="creating-or-modifying-image-definitions"></a>创建或修改映像定义 ##

不允许更改属性 "galleryImage.properties.<property\>"。  
**原因：** 你已尝试更改 OS 类型、OS 状态、Hyper-V 代系、产品/服务、发布者或 SKU。 系统不允许更改这些属性中的任何一个。  
**解决方法**：改为创建新的映像定义。

资源 <galleryName/imageDefinitionName\> 已存在于资源组 <resourceGroup\> 中的位置 <region\_1\>。不能在位置 <region\_2\> 创建同名的资源。请选择新的资源名称。  
**原因：** 同一库和资源组中已存在同名的现有映像定义。 你已尝试在同一库中创建另一个同名但区域不同的映像定义。  
**解决方法**：对映像定义使用其他名称，或将映像定义放在其他库或资源组中。

提供的资源名称 <galleryName\>/<imageDefinitionName\> 的以下尾随字符无效: <character\>。名称不能以下列字符结尾: <character\>  
**原因：** <imageDefinitionName\> 名称以句点或下划线结尾。  
**解决方法**：选择满足以下条件的映像定义名称： 
- 存在 80 个字符的限制
- 只包含英文字母、数字、下划线、连字符和句点
- 以英文字母或数字开头和结尾。

根据验证规则，实体名称 <imageDefinitionName\> 无效: ^[^\_\\W][\\w-.\_]{0,79}(?<![-.])$"  
**原因：** <imageDefinitionName\> 名称以句点或下划线结尾。  
**解决方法**：选择满足以下条件的映像定义名称： 
- 存在 80 个字符的限制
- 只包含英文字母、数字、下划线、连字符和句点
- 以英文字母或数字开头和结尾

资产名称 galleryImage.properties.identifier.<property\> 无效。该名称不能为空。允许的字符为大写或小写字母、数字、连字符 (-)、句点 (.)、下划线 (\_)。名称不允许以句点 (.) 结尾。名称长度不能超过 <number\> 个字符。  
**原因：** 发布者、产品/服务或 SKU 值不符合命名要求。  
**解决方法**：选择满足以下条件的值： 
- 发布者名称的限制为 128 个字符，产品/服务和 SKU 名称的限制为 64 个字符
- 只包含英文字母、数字、连字符、下划线和句点
- 不以句点结尾

*无法对嵌套资源执行请求的操作。找不到父资源 <galleryName\>。*  
**原因：** 当前订阅和资源组中没有名为 <galleryName\> 的库。  
**解决方法**：检查库、订阅和资源组的名称是否正确。 否则，请创建名为 <galleryName\> 的新库。

*提供的位置 <region\> 不适用于资源类型 "Microsoft.Compute/galleries"。资源类型的可用区域列表为 �*  
**原因：** <region\> 名称不正确，或者该名称需要一个访问请求。  
**解决方法**：检查区域名称拼写是否正确。 可以运行此命令来查看你有权访问哪些区域。 如果区域不在列表中，请提交[访问请求](https://docs.microsoft.com/troubleshoot/azure/general/region-access-request-process)。

*无法将值 <value\> 序列化为类型 "iso-8601"，ISO8601Error:缺少 ISO 8601 时间指示符 'T'。无法分析日期/时间字符串 <value\>*  
**原因：** 未将已提供给属性的值作为日期正确设置其格式。  
**解决方法**：以 yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz 或 ISO 8601 格式提供日期。

*无法将字符串转换为 DateTimeOffset: <value\>。路径为 "properties.<property\>"*  
**原因：** 未将已提供给属性的值作为日期正确设置其格式。  
**解决方法**：以 yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz 或 ISO 8601 格式提供日期。
EndOfLifeDate 必须设置为未来日期。  
**原因：** 未将生命周期结束日期属性作为今天的日期之后的某个日期正确设置其格式。  
**解决方法**：以 yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz 或 ISO 8601 格式提供日期。

*argument --<property\>: int 值 <value\> 无效*  
原因：<property\> 仅接受整数值，<value\> 不是整数。  
**解决方法**：选择整数值。

*<property\> 的最小值不得大于 <property\> 的最大值。*  
**原因：** 为 <property\> 提供的最小值大于为 <property\> 提供的最大值。  
**解决方法**：更改值，使最小值小于或等于最大值。

*由(发布者:<Publisher\>、产品/服务:<Offer\>、sku:<SKU\>)标识的库映像 <imageDefinitionName\> 已存在。请选择其他发布者、产品/服务、sku 组合。*  
**原因：** 你已尝试使用相同的发布者、产品/服务和 SKU 三元组作为同一库中的现有映像定义来创建新的映像定义。  
**解决方法**：在库中，所有映像定义必须有一个由发布者、产品/服务和 SKU 组成的独一无二的组合。 请选择一个独一无二的组合，或选择一个新的库，然后重新创建映像定义。

删除嵌套资源前无法删除资源。  
**原因：** 你已尝试删除包含映像版本的映像定义。 映像定义必须为空后才能删除。  
**解决方法**：删除映像定义中的所有映像版本，然后删除映像定义。

*无法绑定参数 <property\>。无法将值 <value\> 转换为类型 <propertyType\>。无法将标识符名称 <value\> 与有效的枚举器名称匹配。请指定以下枚举器名称之一，然后重试: <choice1\>, <choice2\>, �*  
**原因：** 属性有受限的一组可能值，且 <value\> 不是其中之一。  
**解决方法**：选择可能的 <choice\> 值之一。

*无法绑定参数 <property\>。无法将值 <value\> 转换为类型 &quot;System.DateTime&quot;*  
**原因：** 未将已提供给属性的值作为日期正确设置其格式。  
**解决方法**：以 yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz 或 ISO 8601 格式提供日期。

*无法绑定参数 <property\>。无法将值 <value\> 转换为类型 &quot;System.Int32&quot;*  
原因：<property\> 仅接受整数值，<value\> 不是整数。  
**解决方法**：选择整数值。

此区域不支持 ZRS 存储帐户类型。  
**原因：** 你已在尚不支持标准区域冗余存储 (ZRS) 的区域中选择了 ZRS。  
**解决方法**：将存储帐户类型更改为 Premium\_LRS 或 Standard\_LRS。 查看文档，了解已启用 ZRS 预览版的最新[区域列表](../storage/common/storage-redundancy.md#zone-redundant-storage)。

## <a name="creating-or-updating-image-versions"></a>创建或更新映像版本 ##

提供的位置 <region\> 不适用于资源类型 "Microsoft.Compute/galleries"。资源类型的可用区域列表为 �  
**原因：** <region\> 名称不正确，或者该名称需要一个访问请求。  
**解决方法**：检查区域名称拼写是否正确。 可以运行此命令来查看你有权访问哪些区域。 如果区域不在列表中，请提交[访问请求](https://docs.microsoft.com/troubleshoot/azure/general/region-access-request-process)。

*无法对嵌套资源执行请求的操作。找不到父资源 <galleryName/imageDefinitionName\>。*  
**原因：** 当前订阅和资源组中没有名为 <galleryName/imageDefinitionName\> 的库。  
**解决方法**：检查库、订阅和资源组的名称是否正确。 否则，请在指定资源组中创建名为 < galleryName\> 的新库和/或名为 <imageDefinitionName\> 的映像定义。

*无法绑定参数 <property\>。无法将值 <value\> 转换为类型 &quot;System.DateTime&quot;*  
**原因：** 未将已提供给属性的值作为日期正确设置其格式。  
**解决方法**：以 yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz 或 ISO 8601 有效格式提供日期。

*无法绑定参数 <property\>。无法将值 <value\> 转换为类型 &quot;System.Int32&quot;*  
原因：<property\> 仅接受整数值，<value\> 不是整数。  
**解决方法**：选择整数值。

库映像版本发布配置文件区域 <publishingRegions\> 必须包含映像版本 <sourceRegion\> 的位置  
**原因：** 源映像的位置 (<sourceRegion\>) 必须包括在 <publishingRegions\> 列表中。  
**解决方法**：在 <publishingRegions\> 列表中包括 <sourceRegion\>。

参数 <property\> 的值 <value\> 超出范围。该值必须介于 <minValue\> 到 <maxValue\>（两者均含）之间。  
原因：<value\> 超出了 <property\> 的可能值的范围。  
**解决方法**：选择一个在 <minValue\> 到 <maxValue\>（两者均含）范围内的值。

找不到源 <resourceID\>。请检查源是否存在，以及是否与要创建的库映像版本位于同一区域。  
**原因：** <resourceID\> 中没有源，或者 <resourceID\> 中的源与要创建的库映像不在同一区域中。  
**解决方法**：检查 <resourceID\> 值是否正确，以及库映像版本的源区域是否与 <resourceID\> 值的区域相同。

不允许更改属性 "galleryImageVersion.properties.storageProfile.<diskImage\>.source.id"。  
**原因：** 库映像版本的源 ID 在创建之后无法更改。  
**解决方法**：确保此源 ID 与现有源 ID 相同，更改映像版本的版本号，或删除当前映像版本，然后重试。

在输入数据磁盘中检测到重复的 lun 编号。每个数据磁盘的 lun 编号必须独一无二。  
**原因：** 使用磁盘和/或磁盘快照的列表创建映像版本时，两个或更多个磁盘或磁盘快照具有相同的 LUN。  
**解决方法**：删除或更改任何重复的 LUN。

在输入磁盘中发现重复的源 ID。每个磁盘的源 ID 应该独一无二。  
**原因：** 使用磁盘和/或磁盘快照的列表创建映像版本时，两个或更多个磁盘或磁盘快照具有相同的资源 ID。  
**解决方法**：删除或更改任何重复的磁盘源 ID。

路径 "properties.storageProfile.<diskImages\>.source.id" 中的属性 ID <resourceID\> 无效。需要以 "/subscriptions/{subscriptionId}" 或 "/providers/{resourceProviderNamespace}/" 开头的完全限定的资源 ID。  
**原因：** <ResourceID\> 值的格式不正确。  
**解决方法**：检查资源 ID 是否正确。

源 ID <resourceID\> 必须是托管映像、虚拟机或其他库映像版本  
**原因：** <ResourceID\> 值的格式不正确。  
**解决方法**：如果使用 VM、托管映像或库映像版本作为源映像，请检查 VM、托管映像或库映像版本的资源 ID 是否正确。

源 ID <resourceID\> 必须是托管磁盘或快照。  
**原因：** <ResourceID\> 值的格式不正确。  
**解决方法**：如果使用磁盘和/或磁盘快照作为映像版本的源，请检查磁盘和/或磁盘快照的资源 ID 是否正确。

无法从 <resourceID\> 创建库映像版本，因为父库映像中的 OS 状态(<OsState\_1\>)不是 <OsState\_2\>。  
**原因：** 操作系统状态（“通用”或“专用”）与映像定义中指定的操作系统状态不匹配。  
**解决方法**：根据操作系统状态为 <OsState\_1\> 的 VM 选择源，或根据 <OsState\_2\> 为 VM 创建新的映像定义。

ID 为 "<resourceID\>" 的资源的虚拟机监控程序代系 ['<V#\_1\>'] 不同于父库映像虚拟机监控程序代系 ['<V#\_2\>']  
**原因：** 映像版本的虚拟机监控程序代系与映像定义中指定的虚拟机监控程序代系不匹配。 映像定义操作系统是 <V#\_1\>，映像版本操作系统是 <V#\_2\>。  
**解决方法**：选择与映像定义具有相同虚拟机监控程序代系的源，或者创建/选择与映像版本具有相同虚拟机监控程序代系的新映像定义。

ID 为 "<resourceID\>" 的资源的 OS 类型 ['<OsType\_1\>'] 不同于父库映像 OS 类型 ['<OsType \_2\>']  
**原因：** 映像版本的虚拟机监控程序代系与映像定义中指定的虚拟机监控程序代系不匹配。 映像定义操作系统是 <OsType\_1\>，映像版本操作系统是 <OsType\_2\>。  
**解决方法**：选择与映像定义具有相同操作系统 (Linux/Windows) 的源，或者创建/选择与映像版本具有相同操作系统的新映像定义。

源虚拟机 <ResourceID\> 不能包含临时 OS 磁盘。  
**原因：** <ResourceID\> 中的源包含一个临时 OS 磁盘。 共享映像库当前不支持临时 OS 磁盘。  
**解决方法**：根据不使用临时 OS 磁盘的 VM 选择不同的源。

源虚拟机 <resourceID\> 不能包含 UltraSSD 帐户类型中存储的磁盘 ['<diskID\>']。  
**原因：** 磁盘 <diskID\> 是超级 SSD 磁盘。 共享映像库当前不支持超级 SSD 磁盘。  
**解决方法**：使用仅包含高级 SSD、标准 SSD 和/或标准 HDD 托管磁盘的源。

必须从托管磁盘创建源虚拟机 <resourceID\>。  
**原因：** <resourceID\> 中的虚拟机使用非托管磁盘。  
**解决方法**：使用一个其所基于的 VM 仅包含高级 SSD、标准 SSD 和/或标准 HDD 托管磁盘的源。

源 "<resourceID\>" 上的请求数过多。请减少源上的请求数，或等待一段时间后再重试。  
**原因：** 由于请求过多，此映像版本的源当前正受阻。  
**解决方法**：尝试稍后创建此映像版本。

磁盘加密集 "<diskEncryptionSetID\>" 必须位于库资源所在的订阅 "<subscriptionID\>" 中。  
**原因：** 磁盘加密集只能在创建时所在的订阅和区域中使用。  
**解决方法**：在映像版本所在的订阅和区域中创建或使用加密集。

加密的源 "<resourceID\>" 所在的订阅 ID 不同于当前库映像版本订阅 "<subscriptionID\_1\>"。请使用未加密的源重试，或使用源的订阅 "<subcriptionID\_2\>" 创建库映像版本。  
**原因：** 共享映像库当前不支持从另一源映像创建另一订阅中的映像版本（如果对源映像进行了加密）。  
**解决方法**：使用未加密的源，或在源所在的订阅中创建映像版本。

找不到磁盘加密集 <diskEncryptionSetID\>。  
**原因：** 磁盘加密可能不正确。  
**解决方法**：检查磁盘加密集的资源 ID 是否正确。

映像版本名称无效。映像版本名称应遵循 Major(int).Minor(int).Patch(int)格式，例如:1.0.0、2018.12.1，等等。  
**原因：** 映像版本的有效格式是用句点分隔的三个整数。 映像版本名称不符合此有效格式。  
**解决方法**：使用遵循 Major(int).Minor(int).Patch(int) 格式的映像版本名称。 例如：1.0.0 或 2018.12.1。

参数 galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.dataDiskImages.diskEncryptionSetId 的值无效  
**原因：** 数据磁盘映像上使用的磁盘加密集的资源 ID 使用的格式无效。  
**解决方法**：确保磁盘加密集的资源 ID 遵循 /subscriptions/<subscriptionID\>/resourceGroups/<resourceGroupName\>/providers/Microsoft.Compute/<diskEncryptionSetName\> 格式。

参数 galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.osDiskImage.diskEncryptionSetId 的值无效。  
**原因：** OS 磁盘映像上使用的磁盘加密集的资源 ID 使用的格式无效。  
**解决方法**：确保磁盘加密集的资源 ID 遵循 /subscriptions/<subscriptionID\>/resourceGroups/<resourceGroupName\>/providers/Microsoft.Compute/<diskEncryptionSetName\> 格式。

对于更新库映像版本的请求，无法使用区域 [<region\>] 中的磁盘加密集来指定新的数据磁盘映像加密 lun [<number\>]。若要更新此版本，请删除该新 lun。如果需要更改数据磁盘映像加密设置，则必须使用正确的设置创建新的库映像版本。  
**原因：** 已向现有映像版本的数据磁盘添加加密。 不能向现有映像版本添加加密。  
**解决方法**：创建新的库映像版本或删除已添加的加密设置。

库项目版本源只能在 storageProfile 下直接指定，或者在单独的 OS 或数据磁盘中指定。只能提供刚好一种源类型(用户映像、快照、磁盘、虚拟机)。  
**原因：** 缺少源 ID。  
**解决方法**：确保源的源 ID 存在。

找不到源: <resourceID\>。请确保源存在。  
**原因：** 源的资源 ID 可能不正确。  
**解决方法**：确保源的资源 ID 正确。

目标区域 "<Region\_1\>" 中的磁盘 "galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.osDiskImage.diskEncryptionSetId" 需要磁盘加密集，因为磁盘加密集 "<diskEncryptionSetID\>" 用于区域 "<Region\_2\>" 中的相应磁盘  
**原因：** 加密已用于 <Region\_2\> 中的 OS 磁盘，但未用于 <Region\_1\> 中的 OS 磁盘。  
**解决方法**：如果在 OS 磁盘上使用加密，请在所有区域中使用加密。

目标区域 "<Region\_1\>" 中的磁盘 "LUN <number\>" 需要磁盘加密集，因为磁盘加密集 "<diskEncryptionSetID\>" 用于区域 "<Region\_2\>" 中的相应磁盘  
**原因：** 加密已用于 <Region\_2\> 中 LUN <number\> 处的数据磁盘，但未用于 <Region\_1\> 中的相应数据磁盘。  
**解决方法**：如果在数据磁盘上使用加密，请在所有区域中使用加密。

encryption.dataDiskImages 中指定的 lun [<number\>] 无效。lun 必须是以下值之一: ['0,9']。  
**原因：** 为加密指定的 LUN 与附加到 VM 的磁盘的任何 LUN 都不匹配。  
**解决方法**：将加密中的 LUN 更改为 VM 中存在的数据磁盘的 LUN。

在目标区域 "<region\>" encryption.dataDiskImages 中指定了重复的 lun "<number\>"。  
**原因：** <region\> 中使用的加密设置指定了某个 LUN 至少两次。  
**解决方法**：更改 <region\> 中的 LUN，确保 <region\> 中的所有 LUN 都独一无二。

OSDiskImage 和 DataDiskImage 不能指向同一 blob <sourceID\>  
**原因：** OS 磁盘和至少一个数据磁盘的源不是独一无二的。  
**解决方法**：更改 OS 磁盘和/或数据磁盘的源，以确保 OS 磁盘以及每个数据磁盘都独一无二。

目标发布区域中不允许出现重复区域。  
**原因：** 某个区域在发布区域中多次列出。  
**解决方法**：删除重复区域。

不允许在现有映像中添加新的数据磁盘或更改数据磁盘的 LUN。  
**原因：** 对映像版本的更新调用包含新的数据磁盘，或有某个磁盘的新 LUN。  
**解决方法**：使用现有映像版本的 LUN 和数据磁盘。

磁盘加密集 <diskEncryptionSetID\> 必须位于库资源所在的订阅 <subscriptionID\> 中。  
**原因：** 共享映像库当前不支持使用其他订阅中的磁盘加密集。  
**解决方法**：在同一订阅中创建映像版本和磁盘加密集。

## <a name="creating-or-updating-a-vm-or-scale-sets-from-an-image-version"></a>从映像版本创建或更新 VM 或规模集 ##

客户端有权对范围 <resourceID\> 执行操作 "Microsoft.Compute/galleries/images/versions/read"，但当前租户 <tenantId1\> 无权访问链接的订阅 <subscriptionId2\>。  
**原因：** 虚拟机或规模集是通过另一租户中的 SIG 映像创建的。 你已尝试对虚拟机或规模集进行更改，但无权访问拥有该映像的订阅。  
**解决方法**：与映像版本的订阅的所有者联系，让其授予你对映像版本的读取访问权限。

库映像 <resourceID\> 在 <region\> 区域中不可用。请联系映像所有者以将映像复制到此区域，或更改请求的区域。  
**原因：** 要在某个区域中创建的 VM 不在库映像的已发布区域的列表中。  
**解决方法**：将映像复制到该区域，或在库映像的发布区域的其中一个区域中创建 VM。

不允许使用参数 "osProfile"。  
**原因：** 为从专用映像版本创建的 VM 提供了管理员用户名、密码或 SSH 密钥。  
**解决方法**：如果要从该映像创建 VM，请勿包含管理员用户名、密码或 SSH 密钥。 在其他情况下，可使用通用映像版本，并提供管理员用户名、密码或 SSH 密钥。

必需的参数 "osProfile" 缺失(null)。  
**原因：** VM 是通过通用映像创建的，缺少管理员用户名、密码或 SSH 密钥。 由于通用映像不保留管理员用户名、密码或 SSH 密钥，因此，在创建 VM 或规模集的过程中，必须指定这些字段。  
**解决方法**：指定管理员用户名、密码或 SSH 密钥，或使用专用映像版本。

无法从 <resourceID\> 创建库映像版本，因为父库映像中的 OS 状态(“专用”)不是“通用”。  
**原因：** 映像版本是从通用源创建的，但其父定义是专用的。  
**解决方法**：使用专用源来创建映像版本，或使用通用的父定义。

无法更新虚拟机规模集 <vmssName\>，因为 VM 规模集的当前 OS 状态是“通用”，这不同于已更新的库映像 OS 状态(“专用”)。  
**原因：** 规模集的当前源映像是通用源映像，但你却使用专用源映像对其进行更新。 规模集的当前源映像和新的源映像必须处于同一状态。  
**解决方法**：若要更新规模集，请使用通用映像版本。

共享映像库 <versionId\> 中的磁盘加密设置 <diskEncryptionSetId\> 属于订阅 <subscriptionId1\>，不能与订阅 <subscriptionId2\> 中的资源一起使用  
**原因：** 用于对映像版本进行加密的磁盘加密集驻留在与用于托管映像版本的订阅不同的订阅中。  
**解决方法**：对映像版本和磁盘加密集使用相同的订阅。

创建 VM 或虚拟机规模集需要很长的时间。  
**解决方法**：验证你要尝试从其创建 VM 或虚拟机规模集的映像版本的 OSType 与用于创建映像版本的源的 OSType 是否相同。  

## <a name="creating-a-disk-from-an-image-version"></a>从映像版本创建磁盘 ##

参数 imageReference 的值无效。  
**原因：** 你已尝试进行从 SIG 映像版本到磁盘的导出，但使用了映像中不存在的 LUN 位置。    
**解决方法**：检查映像版本，了解在使用的 LUN 位置。

## <a name="sharing-resources"></a>共享资源

可以通过 [Azure 基于角色的访问控制 (Azure RBAC)](../role-based-access-control/rbac-and-directory-admin-roles.md) 启用对映像库、映像定义和映像版本资源的跨订阅共享。 

## <a name="replication-speed"></a>复制速度

请使用 --expand ReplicationStatus 标志来检查是否已完成到所有指定目标区域的复制。 如果尚未完成，请等待作业完成，最长等待时间为 6 小时。 如果失败，请再次触发用于创建并复制映像版本的命令。 如果要将映像版本复制到许多目标区域，可考虑分阶段进行复制。

## <a name="azure-limits-and-quotas"></a>Azure 限制和配额 

[Azure 限制和配额](../azure-resource-manager/management/azure-subscription-service-limits.md)适用于所有共享映像库、映像定义和映像版本资源。 请确保未超出订阅限制。 

## <a name="next-steps"></a>后续步骤

详细了解[共享映像库](./linux/shared-image-galleries.md)。

<!-- Update_Description: update meta properties, wording update, update link -->