---
title: 动态字典 - 翻译器
titleSuffix: Azure Cognitive Services
description: 本文介绍如何使用 Azure 认知服务翻译器的动态字典功能。
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 05/26/2020
ms.date: 03/08/2021
ms.author: v-johya
ms.openlocfilehash: bafb172f26ad87c478b3e8f408e7214d6a771ce7
ms.sourcegitcommit: ec127596b5c56f8ba4d452c39a7b44510b140ed4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103212578"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>如何使用动态字典

若已知道要应用于某个单词或短语的翻译，可以在请求中将其作为标记提供。 动态字典仅适用于复合名词，例如专有名称和产品名称。

**语法：**

<mstrans:dictionary translation="translation of phrase">phrase</mstrans:dictionary>

**要求：**

* `From` 和 `To` 语言必须包含英语和另一种受支持的语言。 
* 你必须在 API 翻译请求中包含 `From` 参数，而不是使用自动检测功能。 

**示例：en-de：**

源输入：`The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

目标输出：`Das Wort "wordomatic" ist ein Wörterbucheintrag.`

无论使用还是不使用 HTML 模式，此功能都以相同的方式工作。

请谨慎使用此功能。