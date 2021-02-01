---
title: Azure Key Vault 可用性和冗余 - Azure Key Vault | Microsoft Docs
description: 了解 Azure Key Vault 可用性和冗余。
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
origin.date: 08/28/2020
ms.date: 01/14/2021
ms.author: v-tawe
ms.openlocfilehash: 95acb70aa4575ae55dd78596242c405339350050
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059935"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure 密钥保管库可用性和冗余

Azure 密钥保管库具有多层冗余功能，确保密钥和机密持续可供应用程序使用，即使服务的单个组件发生故障也是如此。

<!--
> [!NOTE]
> This guide applies to vaults. Managed HSM pools use a different high availability and disaster recovery model. See [Managed HSM Disaster Recovery Guide](../managed-hsm/disaster-recovery-guide.md) for more information.
-->
会在区域中复制密钥保管库的内容，并复制到至少 150 英里以外的次要区域，但位于同一个地理位置，以保持密钥和机密的持久性。 有关特定区域对的详细信息，请参阅 [Azure 配对区域](../../best-practices-availability-paired-regions.md)。  

如果密钥保管库服务中的单独组件发生故障，则区域内的替代组件将继续处理请求，确保不会导致功能损失。 无需执行任何操作即可开始此过程，它会自动发生，且相关信息是透明的。

在整个 Azure 区域不可用的情况下（这很少见），对该区域中的 Azure 密钥保管库发出的请求会自动路由（“故障转移”  ）到次要区域。 当主要区域再次可用时，请求将路由回（“故障回复”  ）到主要区域。 同样，不需要采取任何措施，因为这会自动发生。

通过这种高可用性设计，Azure 密钥保管库不需要停机进行维护活动。

应注意以下几个事项：

* 发生区域故障转移时，可能需要等待几分钟让服务故障转移。 在故障转移之前的这段时间内发出的请求可能会失败。
* 在故障转移期间，密钥保管库处于只读模式。 在此模式下支持的请求包括：
  * 列出证书
  * 获取证书
  * 列出机密
  * 获取机密
  * 列出密钥
  * 获取密钥（属性）
  * 加密
  * 解密
  * 包装
  * 解包
  * Verify
  * 签名
  * 备份

* 在故障转移期间，无法更改密钥保管库属性。 不能更改访问策略或防火墙配置和设置。

* 故障回复之后，所有请求类型（包括读取 *和* 写入请求）都将可用。
