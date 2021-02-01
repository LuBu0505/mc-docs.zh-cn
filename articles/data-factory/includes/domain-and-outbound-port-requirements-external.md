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
ms.openlocfilehash: 0b8690c634986584dd0903765652660bb9717846
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059663"
---
| 域名                  | 出站端口 | 说明                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.core.chinacloudapi.cn`          | 443            | 使用分阶段复制功能时，由自承载集成运行时用来连接到 Azure 存储帐户。 |
| `*.database.chinacloudapi.cn`      | 1433           | 仅当从或向 Azure SQL 数据库或 Azure Synapse Analytics 复制时才是必需的，否则为可选。 在不打开端口 1433 的情况下，使用暂存复制功能将数据复制到 SQL 数据库或 Azure Synapse Analytics。 |
