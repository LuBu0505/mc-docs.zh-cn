---
title: include 文件
description: include 文件
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: include
origin.date: 10/09/2019
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: 933f5fa75f534fc8377cf5ebcb7036165b440425
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059662"
---
| 域名                                          | 出站端口 | 说明                |
| ----------------------------------------------------- | -------------- | ---------------------------|
| 中国：`*.servicebus.chinacloudapi.cn`   | 443            | 自承载集成运行时需要用它来进行交互式创作。 |
| 中国：`{datafactory}.{region}.datafactory.azure.cn` | 443            | 自承载集成运行时连接到数据工厂服务时需要此端口。 <br>对于云中新建的数据工厂，请在自承载集成运行时密钥中查找 FQDN，其格式为 {datafactory}.{region}.datafactory.azure.cn。 对于旧数据工厂，如果在自承载集成密钥中找不到 FQDN，请改用“*.frontend.datamovement.azure.cn”。 |
| `download.microsoft.com`    | 443            | 自承载集成运行时下载更新时需要此端口。 如果已禁用自动更新，则可以跳过对此域的配置。 |
| 密钥保管库 URL | 443           | 如果将凭据存储在 Key Vault 中，则 Azure Key Vault 需要此端口。 |
