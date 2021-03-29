---
title: 包含文件
description: 包含文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 02/03/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 3a9117870ff497bac37f530ecee4a9796c65ac3e
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "104766143"
---
## <a name="best-practices"></a>最佳做法

本部分提供了有关以递归方式设置 ACL 的一些最佳做法指南。 

#### <a name="handling-runtime-errors"></a>处理运行时错误

发生运行时错误可能有许多原因（例如：中断或客户端连接问题）。 如果遇到运行时错误，请重启递归 ACL 过程。 可以将 ACL 重新应用于项，而不会造成负面影响。 

#### <a name="handling-permission-errors-403"></a>处理权限错误 (403)

如果在运行递归 ACL 过程时遇到访问控制异常，则表明 AD [安全主体](/role-based-access-control/overview#security-principal)可能没有足够的权限将 ACL 应用于目录层次结构中的一个或多个子项。 发生权限错误时，此过程会停止，系统会提供一个继续标记。 请修复权限问题，然后使用继续标记来处理剩余的数据集。 已成功处理的目录和文件不需要再次处理。 你还可以选择重启递归 ACL 过程。 可以将 ACL 重新应用于项，而不会造成负面影响。 

#### <a name="credentials"></a>凭据 

建议你预配一个在目标存储帐户或容器范围中分配有[存储 Blob 数据所有者](/role-based-access-control/built-in-roles#storage-blob-data-owner)角色的 Azure AD 安全主体。 

#### <a name="performance"></a>性能 

为了减少延迟，建议你在与存储帐户位于同一区域中的 Azure 虚拟机 (VM) 中运行递归 ACL 过程。 

#### <a name="acl-limits"></a>ACL 限制

可应用于目录或文件的 ACL 的最大数目为 32 个访问 ACL 和 32 个默认 ACL。 有关详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](/storage/blobs/data-lake-storage-access-control)。