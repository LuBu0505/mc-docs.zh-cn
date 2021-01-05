---
title: 验证内置防火墙中的端口安全性
description: 了解如何验证 Azure SQL 托管实例中的内置防火墙保护。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, carlrab
origin.date: 12/04/2018
ms.date: 01/04/2021
ms.openlocfilehash: 9927d05d6960db6f6bd1d23e903b1b823aa0e816
ms.sourcegitcommit: cf3d8d87096ae96388fe273551216b1cb7bf92c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/31/2020
ms.locfileid: "97830305"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>验证 Azure SQL 托管实例内置防火墙
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL 托管实例[强制入站安全规则](connectivity-architecture-overview.md#mandatory-inbound-security-rules-with-service-aided-subnet-configuration)要求管理端口 9000、9003、1438、1440 和 1452 从保护 SQL 托管实例的网络安全组 (NSG) 上的任何源中开放。 虽然这些端口在 NSG 级别处于开放状态，但它们由内置防火墙在网络级别进行保护。

## <a name="verify-firewall"></a>验证防火墙

若要验证这些端口，请使用任何安全扫描程序工具来测试这些端口。 以下屏幕截图显示了如何使用这些工具之一。

![验证内置防火墙](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>后续步骤

有关 SQL 托管实例和连接的详细信息，请参阅 [Azure SQL 托管实例连接体系结构](connectivity-architecture-overview.md)。
