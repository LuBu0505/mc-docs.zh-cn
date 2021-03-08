---
title: 语言支持 - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音服务支持多种语言，可用于语音到文本和文本到语音转换，以及语音翻译。 本文提供了按服务功能列出的语言支持的完整列表。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 01/07/2021
ms.date: 02/19/2021
ms.author: v-johya
ms.custom: references_regions
ms.openlocfilehash: 1703ba5f237a52664e153bd220ccdf55f3a0e0fe
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697546"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>语音服务的语言和语音支持

语言支持因语音服务功能而异。 下表汇总了对[语音转文本](#speech-to-text)、[文本转语音](#text-to-speech)和[语音翻译](#speech-translation)服务产品的语言支持。

## <a name="speech-to-text"></a>语音转文本

Microsoft 语音 SDK 和 REST API 都支持以下语言（区域设置）。 

为了提高准确性，已为一部分语言提供了自定义功能，你可通过上传音频和人工标记的脚本或相关文本（语句）进行自定义。 支持使用“音频 + 人为标记的脚本”自定义声学模型，仅限于下面列出的特定基础模型。 其他基础模型和语言将只使用脚本的文本来训练自定义模型，就像“相关文本: 句子”一样。 若要了解有关自定义的详细信息，请参阅[自定义语音识别入门](./custom-speech-overview.md)。

<!--
To get the AM and ML bits:
https://chinaeast2.dev.cognitive.azure.cn/docs/services/speech-to-text-api-v3-0/operations/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.azure.cn -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| 语言                 | 区域设置 (BCP-47) | 自定义  | [语言检测](how-to-automatic-language-detection.md) |
|------------------------------------|--------|---------------------------------------------------|-------------------------------|
| 阿拉伯语(巴林)，现代标准  | `ar-BH` | 文本                                   | 是                           | 
| 阿拉伯语（埃及）                     | `ar-EG` | 文本                                   | 是                          |
| 阿拉伯语（伊拉克）                      | `ar-IQ` | 文本                                   |                           |
| 阿拉伯语（以色列）                    | `ar-IL` | 文本                                   |                           |
| 阿拉伯语（约旦）                    | `ar-JO` | 文本                                   |                           |
| 阿拉伯语（科威特）                    | `ar-KW` | 文本                                   |                           |
| 阿拉伯语（黎巴嫩）                   | `ar-LB` | 文本                                   |                           |
| 阿拉伯语（阿曼）                      | `ar-OM` | 文本                                   |                           |
| 阿拉伯语（卡塔尔）                     | `ar-QA` | 文本                                   |                           |
| 阿拉伯语(沙特阿拉伯)              | `ar-SA` | 文本                                   | 是                          |
| 阿拉伯语（巴勒斯坦）        | `ar-PS` | 文本                                   |                           |
| 阿拉伯语（叙利亚）                     | `ar-SY` | 文本                                   | 是                          |
| 阿拉伯语（阿拉伯联合酋长国）      | `ar-AE` | 文本                                   |                           |
| 保加利亚语(保加利亚)               | `bg-BG` | 文本                                   |                           |
| 加泰罗尼亚语(西班牙)                    | `ca-ES` | 文本                                   | 是                          |
| 中文（粤语，繁体）   | `zh-HK` | 音频 (20201015)<br>文本                 |        是                   |
| 中文（普通话，简体）     | `zh-cn` | 音频 (20200910)<br>文本                 |     是                      |
| 中文(台湾普通话)       | `zh-TW` | 音频 (20190701, 20201015)<br>文本                 |           是                |
| 克罗地亚语（克罗地亚）                 | `hr-HR` | 文本                                   |                           |
| 捷克语（捷克共和国）             | `cs-CZ` | 文本                                   |                           |
| 丹麦语（丹麦）                   | `da-DK` | 文本                                   | 是                          |
| 荷兰语（荷兰）                | `nl-NL` | 音频 (20201015)<br>文本                                   |    是                       |
| 英语（澳大利亚）                | `en-AU` | 音频 (20201019)<br>文本                 | 是                          |
| 英语（加拿大）                   | `en-CA` | 音频 (20201019)<br>文本                 | 是                          |
| 英语（香港）                | `en-HK` | 文本                                   |                           |
| 英语（印度）                    | `en-IN` | 音频 (20200923)<br>文本                 | 是                          |
| 英语（爱尔兰）                  | `en-IE` | 文本                                   |                           |
| 英语（新西兰）              | `en-NZ` | 音频 (20201019)<br>文本                 |  是                         |
| 英语（尼日利亚）                  | `en-NG` | 文本                                   |                           |
| 英语（菲律宾）              | `en-PH` | 文本                                   |                           |
| 英语（新加坡）                | `en-SG` | 文本                                   |                           |
| 英语（南非）             | `en-ZA` | 文本                                   |                           |
| 英语（英国）           | `en-GB` | 音频 (20201019)<br>文本<br>发音| 是                          |
| 英语（美国）            | `en-US` | 音频 (20201019)<br>文本<br>发音| 是                          |
| 爱沙尼亚语（爱沙尼亚）                  | `et-EE` | 文本                                   |                           |
| 芬兰语（芬兰）                  | `fi-FI` | 文本                                   |     是                      |
| 法语（加拿大）                    | `fr-CA` | 音频 (20201015)<br>文本                 |     是                      |
| 法语（法国）                    | `fr-FR` | 音频 (20201015)<br>文本<br>发音|      是                     |
| 德语（德国）                   | `de-DE` | 音频 (20190701, 20200619, 20201127)<br>文本<br>发音|  是                         |
| 希腊语(希腊)                     | `el-GR` | 文本                                   |                           |
| 古吉拉特语(印度)                  | `gu-IN` | 文本                                   |                           |
| 印地语（印度）                      | `hi-IN` | 音频 (20200701)<br>文本                 |     是                      |
| 匈牙利语(匈牙利)                | `hu-HU` | 文本                                   |                           |
| 爱尔兰语（爱尔兰）                     | `ga-IE` | 文本                                   |                           |
| 意大利语（意大利）                    | `it-IT` | 音频 (20201016)<br>文本<br>发音|      是                     |
| 日语（日本）                   | `ja-JP` | 文本                                   |      是                     |
| 韩语(韩国)                     | `ko-KR` | 音频 (20201015)<br>文本                 |      是                     |
| 拉脱维亚语(拉脱维亚)                   | `lv-LV` | 文本                                   |                           |
| 立陶宛语(立陶宛)             | `lt-LT` | 文本                                   |                           |
| 马耳他语（马耳他）                     | `mt-MT` | 文本                                   |                           |
| 马拉地语(印度)                    | `mr-IN` | 文本                                   |                           |
| 挪威 (Bokm�l, Norway)         | `nb-NO` | 文本                                   |     是                      |
| 波兰语（波兰）                    | `pl-PL` | 文本                                   |       是                    |
| 葡萄牙语(巴西)                | `pt-BR` | 音频 (20190620, 20201015)<br>文本<br>发音|          是                 |
| 葡萄牙语(葡萄牙)              | `pt-PT` | 文本                                   |             是              |
| 罗马尼亚语(罗马尼亚)                 | `ro-RO` | 文本                                   |                           |
| 俄语（俄罗斯）                   | `ru-RU` | 音频 (20200907)<br>文本                 |                是           |
| 斯洛伐克语(斯洛伐克)                  | `sk-SK` | 文本                                   |                           |
| 斯洛文尼亚语(斯洛文尼亚)               | `sl-SI` | 文本                                   |                           |
| 西班牙语（阿根廷）                | `es-AR` | 文本                                   |                           |
| 西班牙语（玻利维亚）                  | `es-BO` | 文本                                   |                           |
| 西班牙语（智利）                    | `es-CL` | 文本                                   |                           |
| 西班牙语（哥伦比亚）                 | `es-CO` | 文本                                   |                           |
| 西班牙语（哥斯达黎加）               | `es-CR` | 文本                                   |                           |
| 西班牙语（古巴）                     | `es-CU` | 文本                                   |                           |
| 西班牙语（多米尼加共和国）       | `es-DO` | 文本                                   |                           |
| 西班牙语（厄瓜多尔）                  | `es-EC` | 文本                                   |                           |
| 西班牙语（萨尔瓦多）              | `es-SV` | 文本                                   |                           |
| 西班牙语（赤道几内亚）        | `es-GQ` | 文本                                   |                           |
| 西班牙语（危地马拉）                | `es-GT` | 文本                                   |                           |
| 西班牙语（洪都拉斯）                 | `es-HN` | 文本                                   |                           |
| 西班牙语(墨西哥)                   | `es-MX` | 音频 (20200907)<br>文本                 |    是                       |
| 西班牙（尼加拉瓜）                | `es-NI` | 文本                                   |                           |
| 西班牙语（巴拿马）                   | `es-PA` | 文本                                   |                           |
| 西班牙语（巴拉圭）                 | `es-PY` | 文本                                   |                           |
| 西班牙语（秘鲁）                     | `es-PE` | 文本                                   |                           |
| 西班牙语（波多黎各）              | `es-PR` | 文本                                   |                           |
| 西班牙语(西班牙)                    | `es-ES` | 音频 (20201015)<br>文本                 |  是                         |
| 西班牙语（乌拉圭）                  | `es-UY` | 文本                                   |                           |
| 西班牙语（美国）                      | `es-US` | 文本                                   |                           |
| 西班牙语（委内瑞拉）                | `es-VE` | 文本                                   |                           |
| 瑞典语（瑞典）                   | `sv-SE` | 文本                                   |   是                        |
| 泰米尔语（印度）                      | `ta-IN` | 文本                                   |                           |
| 泰卢固语（印度）                     | `te-IN` | 文本                                   |                           |
| 泰语（泰国）                    | `th-TH` | 文本                                   |      是                     |
| 土耳其语（土耳其）                   | `tr-TR` | 文本                                   |                           |

## <a name="text-to-speech"></a>文本转语音

Microsoft 语音 SDK 和 REST API 支持以下语音，其中的每种语音都支持特定语言和方言（按区域设置标识）。 还可以通过[语音/列表 API](rest-text-to-speech.md#get-a-list-of-voices) 获取每个特定区域/终结点支持的语言和语音的完整列表。 

> [!IMPORTANT]
> 标准语音、自定义语音和神经语音的定价各不相同。 有关其他信息，请访问[定价](https://www.azure.cn/pricing/details/cognitive-services/)页。

### <a name="neural-voices"></a>神经语音

神经文本到语音转换是由深度神经网络提供支持的新型语音合成。 使用神经语音时，几乎无法将合成的语音与人类录音区分开来。

使用神经语音可使得与聊天机器人和语音助手的交互更加自然且富有吸引力、将数字文本（如电子书）转换为有声读物以及增强车载导航系统。 随着类人的自然韵律和字词的清晰发音，用户在与 AI 系统交互时，神经语音显著减轻了听力疲劳。

> [!NOTE]
> 神经语音由使用 24 khz 采样率的样本创建。
> 合成时，所有声音都可以使用其他采样率进行上采样或下采样。


| 语言 | Locale | 性别 | 语音名称 | 风格支持 |
|---|---|---|---|---|
| 阿拉伯语（埃及） | `ar-EG` | Female | `ar-EG-SalmaNeural` | 常规 |
| 阿拉伯语（埃及） | `ar-EG` | 男 | `ar-EG-ShakirNeural` <sup>新建</sup> | 常规 |
| 阿拉伯语（沙特阿拉伯） | `ar-SA` | 女 | `ar-SA-ZariyahNeural` | 常规 |
| 阿拉伯语（沙特阿拉伯） | `ar-SA` | 男 | `ar-SA-HamedNeural` <sup>新建</sup> | 常规 |
| 保加利亚语(保加利亚) | `bg-BG` | 女 | `bg-BG-KalinaNeural` | 常规 |
| 保加利亚语(保加利亚) | `bg-BG` | 男 | `bg-BG-BorislavNeural` <sup>新建</sup> | 常规 |
| 加泰罗尼亚语(西班牙) | `ca-ES` | 女 | `ca-ES-AlbaNeural` | 常规 |
| 加泰罗尼亚语(西班牙) | `ca-ES` | Female | `ca-ES-JoanaNeural` <sup>新建</sup> | 常规 |
| 加泰罗尼亚语(西班牙) | `ca-ES` | 男 | `ca-ES-EnricNeural` <sup>新建</sup> | 常规 |
| 中文（粤语，繁体） | `zh-HK` | Female | `zh-HK-HiuGaaiNeural` | 常规 |
| 中文（粤语，繁体） | `zh-HK` | Female | `zh-HK-HiuMaanNeural` <sup>新建</sup> | 常规 |
| 中文(粤语，繁体) | `zh-HK` | 男 | `zh-HK-WanLungNeural` <sup>新建</sup> | 常规 |
| 中文（普通话，简体） | `zh-cn` | 女 | `zh-cn-XiaoxiaoNeural` | 常规，[使用 SSML](speech-synthesis-markup.md#adjust-speaking-styles) 提供多种语音风格  |
| 中文（普通话，简体） | `zh-cn` | 女 | `zh-cn-XiaoyouNeural` | 儿童语音，针对讲故事进行了优化 |
| 中文（普通话，简体） | `zh-cn` | 男 | `zh-cn-YunyangNeural` | 针对新闻阅读进行了优化，<br /> [使用 SSML](speech-synthesis-markup.md#adjust-speaking-styles) 提供多种语音风格 |
| 中文（普通话，简体） | `zh-cn` | 男 | `zh-cn-YunyeNeural` | 针对讲故事进行了优化  |
| 中文(台湾普通话) | `zh-TW` | Female | `zh-TW-HsiaoChenNeural` <sup>新建</sup> | 常规 |
| 中文(台湾普通话) | `zh-TW` | Female | `zh-TW-HsiaoYuNeural` | 常规 |
| 中文(台湾普通话) | `zh-TW` | 男 | `zh-TW-YunJheNeural` <sup>新建</sup> | 常规 |
| 克罗地亚语（克罗地亚） | `hr-HR` | 女 | `hr-HR-GabrijelaNeural` | 常规 |
| 克罗地亚语（克罗地亚） | `hr-HR` | 男 | `hr-HR-SreckoNeural` <sup>新建</sup> | 常规 |
| 捷克语（捷克） | `cs-CZ` | 女 | `cs-CZ-VlastaNeural` | 常规 |
| 捷克语（捷克） | `cs-CZ` | 男 | `cs-CZ-AntoninNeural` <sup>新建</sup> | 常规 |
| 丹麦语（丹麦） | `da-DK` | 女 | `da-DK-ChristelNeural` | 常规 |
| 丹麦语（丹麦） | `da-DK` | 男 | `da-DK-JeppeNeural` <sup>新建</sup> | 常规 |
| 荷兰语（荷兰） | `nl-NL` | 女 | `nl-NL-ColetteNeural` | 常规 |
| 荷兰语（荷兰） | `nl-NL` | Female | `nl-NL-FennaNeural` <sup>新建</sup> | 常规 |
| 荷兰语（荷兰） | `nl-NL` | 男 | `nl-NL-MaartenNeural` <sup>新建</sup> | 常规 |
| 英语（澳大利亚） | `en-AU` | Female | `en-AU-NatashaNeural` | 常规 |
| 英语（澳大利亚） | `en-AU` | 男 | `en-AU-WilliamNeural` | 常规 |
| 英语（加拿大） | `en-CA` | Female | `en-CA-ClaraNeural` | 常规 |
| 英语（加拿大） | `en-CA` | 男 | `en-CA-LiamNeural` <sup>新建</sup> | 常规 |
| 英语（印度） | `en-IN` | Female | `en-IN-NeerjaNeural` | 常规 |
| 英语（印度） | `en-IN` | 男 | `en-IN-PrabhatNeural` <sup>新建</sup> | 常规 |
| 英语（爱尔兰） | `en-IE` | 女 | `en-IE-EmilyNeural` | 常规 |
| 英语（爱尔兰） | `en-IE` | 男 | `en-IE-ConnorNeural` <sup>新建</sup> | 常规 |
| 英语（英国） | `en-GB` | 女 | `en-GB-LibbyNeural` | 常规 |
| 英语（英国） | `en-GB` | 女 | `en-GB-MiaNeural` | 常规 |
| 英语（英国） | `en-GB` | 男 | `en-GB-RyanNeural` | 常规 |
| 英语（美国） | `en-US` | 女 | `en-US-AriaNeural` | 常规，[使用 SSML](speech-synthesis-markup.md#adjust-speaking-styles) 提供多种语音风格  |
| 英语（美国） | `en-US` | 女 | `en-US-JennyNeural` | 常规 |
| 英语（美国） | `en-US` | 男 | `en-US-GuyNeural` | 常规 |
| 芬兰语（芬兰） | `fi-FI` | 女 | `fi-FI-NooraNeural` | 常规 |
| 芬兰语（芬兰） | `fi-FI` | Female | `fi-FI-SelmaNeural` <sup>新建</sup> | 常规 |
| 芬兰语（芬兰） | `fi-FI` | 男 | `fi-FI-HarriNeural` <sup>新建</sup> | 常规 |
| 法语（加拿大） | `fr-CA` | Female | `fr-CA-SylvieNeural` | 常规 |
| 法语（加拿大） | `fr-CA` | 男 | `fr-CA-JeanNeural` | 常规 |
| 法语（法国） | `fr-FR` | Female | `fr-FR-DeniseNeural` | 常规 |
| 法语（法国） | `fr-FR` | 男 | `fr-FR-HenriNeural` | 常规 |
| 法语（瑞士） | `fr-CH` | 女 | `fr-CH-ArianeNeural` | 常规 |
| 法语（瑞士） | `fr-CH` | 男 | `fr-CH-FabriceNeural` <sup>新建</sup> | 常规 |
| 德语（奥地利） | `de-AT` | 女 | `de-AT-IngridNeural` | 常规 |
| 德语（奥地利） | `de-AT` | 男 | `de-AT-JonasNeural` <sup>新建</sup> | 常规 |
| 德语（德国） | `de-DE` | Female | `de-DE-KatjaNeural` | 常规 |
| 德语（德国） | `de-DE` | 男 | `de-DE-ConradNeural` | 常规 |
| 德语（瑞士） | `de-CH` | 女 | `de-CH-LeniNeural` | 常规 |
| 德语（瑞士） | `de-CH` | 男 | `de-CH-JanNeural` <sup>新建</sup> | 常规 |
| 希腊语(希腊) | `el-GR` | 女 | `el-GR-AthinaNeural` | 常规 |
| 希腊语(希腊) | `el-GR` | 男 | `el-GR-NestorasNeural` <sup>新建</sup> | 常规 |
| 希伯来语（以色列） | `he-IL` | 女 | `he-IL-HilaNeural` | 常规 |
| 希伯来语（以色列） | `he-IL` | 男 | `he-IL-AvriNeural` <sup>新建</sup> | 常规 |
| 印地语（印度） | `hi-IN` | Female | `hi-IN-SwaraNeural` | 常规 |
| 印地语（印度） | `hi-IN` | 男 | `hi-IN-MadhurNeural` <sup>新建</sup> | 常规 |
| 匈牙利语(匈牙利) | `hu-HU` | 女 | `hu-HU-NoemiNeural` | 常规 |
| 匈牙利语(匈牙利) | `hu-HU` | 男 | `hu-HU-TamasNeural` <sup>新建</sup> | 常规 |
| 印度尼西亚语(印度尼西亚) | `id-ID` | Female | `id-ID-GadisNeural` <sup>新建</sup> | 常规 |
| 印度尼西亚语(印度尼西亚) | `id-ID` | 男 | `id-ID-ArdiNeural` | 常规 |
| 意大利语（意大利） | `it-IT` | Female | `it-IT-ElsaNeural` | 常规 |
| 意大利语（意大利） | `it-IT` | Female | `it-IT-IsabellaNeural` | 常规 |
| 意大利语（意大利） | `it-IT` | 男 | `it-IT-DiegoNeural` | 常规 |
| 日语（日本） | `ja-JP` | 女 | `ja-JP-NanamiNeural` | 常规 |
| 日语（日本） | `ja-JP` | 男 | `ja-JP-KeitaNeural` | 常规 |
| 韩语(韩国) | `ko-KR` | Female | `ko-KR-SunHiNeural` | 常规 |
| 韩语(韩国) | `ko-KR` | 男 | `ko-KR-InJoonNeural` | 常规 |
| 马来语（马来西亚） | `ms-MY` | 女 | `ms-MY-YasminNeural` | 常规 |
| 马来语（马来西亚） | `ms-MY` | 男 | `ms-MY-OsmanNeural` <sup>新建</sup> | 常规 |
| 挪威 (Bokm�l, Norway) | `nb-NO` | 女 | `nb-NO-IselinNeural` | 常规 |
| 挪威 (Bokm�l, Norway) | `nb-NO` | Female | `nb-NO-PernilleNeural` <sup>新建</sup> | 常规 |
| 挪威 (Bokm�l, Norway) | `nb-NO` | 男 | `nb-NO-FinnNeural` <sup>新建</sup> | 常规 |
| 波兰语（波兰） | `pl-PL` | Female | `pl-PL-AgnieszkaNeural` <sup>新建</sup> | 常规 |
| 波兰语（波兰） | `pl-PL` | 女 | `pl-PL-ZofiaNeural` | 常规 |
| 波兰语（波兰） | `pl-PL` | 男 | `pl-PL-MarekNeural` <sup>新建</sup> | 常规 |
| 葡萄牙语（巴西） | `pt-BR` | Female | `pt-BR-FranciscaNeural` | 常规，[使用 SSML](speech-synthesis-markup.md#adjust-speaking-styles) 提供多种语音风格  |
| 葡萄牙语(巴西) | `pt-BR` | 男 | `pt-BR-AntonioNeural` | 常规 |
| 葡萄牙语(葡萄牙) | `pt-PT` | Female | `pt-PT-FernandaNeural` | 常规 |
| 葡萄牙语(葡萄牙) | `pt-PT` | Female | `pt-PT-RaquelNeural` <sup>新建</sup> | 常规 |
| 葡萄牙语(葡萄牙) | `pt-PT` | 男 | `pt-PT-DuarteNeural` <sup>新建</sup> | 常规 |
| 罗马尼亚语(罗马尼亚) | `ro-RO` | 女 | `ro-RO-AlinaNeural` | 常规 |
| 罗马尼亚语(罗马尼亚) | `ro-RO` | 男 | `ro-RO-EmilNeural` <sup>新建</sup> | 常规 |
| 俄语（俄罗斯） | `ru-RU` | Female | `ru-RU-DariyaNeural` | 常规 |
| 俄语（俄罗斯） | `ru-RU` | Female | `ru-RU-SvetlanaNeural` <sup>新建</sup> | 常规 |
| 俄语（俄罗斯） | `ru-RU` | 男 | `ru-RU-DmitryNeural` <sup>新建</sup> | 常规 |
| 斯洛伐克语(斯洛伐克) | `sk-SK` | 女 | `sk-SK-ViktoriaNeural` | 常规 |
| 斯洛伐克语(斯洛伐克) | `sk-SK` | 男 | `sk-SK-LukasNeural` <sup>新建</sup> | 常规 |
| 斯洛文尼亚语(斯洛文尼亚) | `sl-SI` | 女 | `sl-SI-PetraNeural` | 常规 |
| 斯洛文尼亚语(斯洛文尼亚) | `sl-SI` | 男 | `sl-SI-RokNeural` <sup>新建</sup> | 常规 |
| 西班牙语（墨西哥） | `es-MX` | Female | `es-MX-DaliaNeural` | 常规 |
| 西班牙语（墨西哥） | `es-MX` | 男 | `es-MX-JorgeNeural` | 常规 |
| 西班牙语(西班牙) | `es-ES` | 女 | `es-ES-ElviraNeural` | 常规 |
| 西班牙语(西班牙) | `es-ES` | 男 | `es-ES-AlvaroNeural` | 常规 |
| 瑞典语（瑞典） | `sv-SE` | 女 | `sv-SE-HilleviNeural` | 常规 |
| 瑞典语（瑞典） | `sv-SE` | Female | `sv-SE-SofieNeural` <sup>新建</sup> | 常规 |
| 瑞典语（瑞典） | `sv-SE` | 男 | `sv-SE-MattiasNeural` <sup>新建</sup> | 常规 |
| 泰米尔语（印度） | `ta-IN` | 女 | `ta-IN-PallaviNeural` | 常规 |
| 泰米尔语（印度） | `ta-IN` | 男 | `ta-IN-ValluvarNeural` <sup>新建</sup> | 常规 |
| 泰卢固语（印度） | `te-IN` | Female | `te-IN-ShrutiNeural` | 常规 |
| 泰卢固语（印度） | `te-IN` | 男 | `te-IN-MohanNeural` <sup>新建</sup> | 常规 |
| 泰语（泰国） | `th-TH` | 女 | `th-TH-AcharaNeural` | 常规 |
| 泰语（泰国） | `th-TH` | 女 | `th-TH-PremwadeeNeural` | 常规 |
| 泰语（泰国） | `th-TH` | 男 | `th-TH-NiwatNeural` <sup>新建</sup> | 常规 |
| 土耳其语（土耳其） | `tr-TR` | Female | `tr-TR-EmelNeural` | 常规 |
| 土耳其语（土耳其） | `tr-TR` | 男 | `tr-TR-AhmetNeural` <sup>新建</sup> | 常规 |
| 越南语(越南) | `vi-VN` | 女 | `vi-VN-HoaiMyNeural` | 常规 |
| 越南语(越南) | `vi-VN` | 男 | `vi-VN-NamMinhNeural` <sup>新建</sup> | 常规 |

<!-- #### Neural voices in preview -->

<!--Customized in MC-->
### <a name="standard-voices"></a>标准语音

40 多种标准语音在 10 多种语言和区域设置中提供，允许你将文本转换为合成语音。 有关区域可用性的详细信息，请参阅[区域](regions.md#standard-and-neural-voices)。

> [!NOTE]
> 除了两个例外，标准语音从使用 16 khz 采样率的样本创建。
> en-US-AriaRUS 和 en-US-GuyRUS 语音也是从使用 24 khz 采样率的样本创建的。
> 合成时，所有声音都可以使用其他采样率进行上采样或下采样。

| 语言 | 区域设置 (BCP-47) | 性别 | 语音名称 |
|--|--|--|--|
| 阿拉伯语（阿拉伯） | `ar-EG` | Female | `ar-EG-Hoda`|
| 阿拉伯语（沙特阿拉伯） | `ar-SA` | 男 | `ar-SA-Naayf`|
| 保加利亚语(保加利亚) | `bg-BG` | 男 | `bg-BG-Ivan`|
| 中文(粤语，繁体) | `zh-HK` | 男 | `zh-HK-Danny`|
| 中文（粤语，繁体） | `zh-HK` | Female | `zh-HK-TracyRUS`|
| 中文（普通话，简体） | `zh-cn` | 女 | `zh-cn-HuihuiRUS`|
| 中文（普通话，简体） | `zh-cn` | 男 | `zh-cn-Kangkang`|
| 中文（普通话，简体） | `zh-cn` | 女 | `zh-cn-Yaoyao`|
| 中文(台湾普通话) |  `zh-TW` | Female | `zh-TW-HanHanRUS`|
| 中文(台湾普通话) |  `zh-TW` | Female | `zh-TW-Yating`|
| 中文(台湾普通话) |  `zh-TW` | 男 | `zh-TW-Zhiwei`|
| 英语（澳大利亚） | `en-AU` | Female | `en-AU-Catherine`|
| 英语（澳大利亚） | `en-AU` | Female | `en-AU-HayleyRUS`|
| 英语（加拿大） | `en-CA` | Female | `en-CA-HeatherRUS`|
| 英语（加拿大） | `en-CA` | Female | `en-CA-Linda`|
| 英语（印度） | `en-IN` | Female | `en-IN-Heera`|
| 英语（印度） | `en-IN` | Female | `en-IN-PriyaRUS`|
| 英语（印度） | `en-IN` | 男 | `en-IN-Ravi`|
| 英语（爱尔兰） | `en-IE` | 男 | `en-IE-Sean`|
| 英语（英国） | `en-GB` | 男 | `en-GB-George`|
| 英语（英国） | `en-GB` | Female | `en-GB-HazelRUS`|
| 英语（英国） | `en-GB` | Female | `en-GB-Susan`|
| 英语（美国） | `en-US` | 男 | `en-US-BenjaminRUS`|
| 英语（美国） | `en-US` | 男 | `en-US-GuyRUS`|
| 英语（美国） | `en-US` | Female | `en-US-AriaRUS`|
| 英语（美国） | `en-US` | 女 | `en-US-ZiraRUS`|
| 法语（加拿大） | `fr-CA` | Female | `fr-CA-Caroline`|
| 法语（加拿大） | `fr-CA` | Female | `fr-CA-HarmonieRUS`|
| 法语（法国） | `fr-FR` | Female | `fr-FR-HortenseRUS`|
| 法语（法国） | `fr-FR` | Female | `fr-FR-Julie`|
| 法语（法国） | `fr-FR` | 男 | `fr-FR-Paul`|
| 法语（瑞士） | `fr-CH` | 男 | `fr-CH-Guillaume`|
| 德语（奥地利） | `de-AT` | 男 | `de-AT-Michael`|
| 德语（德国） | `de-DE` | Female | `de-DE-HeddaRUS`|
| 德语（德国） | `de-DE` | 男 | `de-DE-Stefan`|
| 德语（瑞士） | `de-CH` | 男 | `de-CH-Karsten`|
| 印地语（印度） | `hi-IN` | 男 | `hi-IN-Hemant`|
| 印地语（印度） | `hi-IN` | Female | `hi-IN-Kalpana`|
| 韩语(韩国) | `ko-KR` | Female | `ko-KR-HeamiRUS`|
| 俄语（俄罗斯） | `ru-RU` | Female | `ru-RU-EkaterinaRUS`|
| 俄语（俄罗斯） | `ru-RU` | Female | `ru-RU-Irina`|
| 俄语（俄罗斯） | `ru-RU` | 男 | `ru-RU-Pavel`|
| 西班牙语（墨西哥） | `es-MX` | Female | `es-MX-HildaRUS`|
| 西班牙语（墨西哥） | `es-MX` | 男 | `es-MX-Raul`|
| 西班牙语(西班牙) | `es-ES` | 女 | `es-ES-HelenaRUS`|
| 西班牙语(西班牙) | `es-ES` | 女 | `es-ES-Laura`|
| 西班牙语(西班牙) | `es-ES` | 男 | `es-ES-Pablo`|

> [!IMPORTANT]
> `en-US-Jessa` 语音已更改为 `en-US-Aria`。 如果以前使用了“Jessa”，请转换为“Aria”。

> [!TIP]
> 可以继续在语音合成请求中使用完整的服务名称映射，如“Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)”。
<!--Customized in MC-->

### <a name="customization"></a>自定义

自定义语音在标准层和神经网络层中可用。 这两层支持的语言不同。 

| 语言 | Locale | Standard | 神经 |
|--|--|--|--|
| 中文（普通话，简体） | `zh-cn` | 是 | 是 |
| 中英双语 | `zh-cn` 双语 | 是 | 是 |
| 英语（澳大利亚） | `en-AU` | 否 | 是 |
| 英语（印度） | `en-IN` | 是 | 是 |
| 英语（英国） | `en-GB` | 是 | 是 |
| 英语（美国） | `en-US` | 是 | 是 |
| 法语（加拿大） | `fr-CA` | 否 | 是 |
| 法语（法国） | `fr-FR` | 是 | 是 |
| 德语（德国） | `de-DE` | 是 | 是 |
| 意大利语（意大利） | `it-IT` | 是 | 是 |
| 日语（日本） | `ja-JP` | 否 | 是 |
| 韩语(韩国) | `ko-KR` | 否 | 是 |
| 葡萄牙语（巴西） | `pt-BR` | 是 | 是 |
| 西班牙语（墨西哥） | `es-MX` | 是 | 是 |
| 西班牙语(西班牙) | `es-ES` | 否 | 是 |

选择与训练自定义语音模型所需的训练数据相匹配的正确区域设置。 例如，如果你的录音数据是以带英国口音的英语说出的，请选择 `en-GB`。

> [!NOTE]
> 除了中英双语模型之外，我们在自定义语音中不支持其他双语模型训练。 如果要训练一种也可以说英语的中文语音，请选择“中英双语”。

<!--Customized in MC-->
## <a name="speech-translation"></a>语音翻译

**语音翻译** API 支持使用不同的语言进行语音转语音和语音转文本的翻译。 源语言必须始终来自“语音转文本”语言表。 可用的目标语言取决于翻译目标是语音还是文本。 可以将传入的语音翻译成 [60 种以上的语言](https://www.microsoft.com/translator/business/languages/)。 这些语言的子集可用于[语音合成](language-support.md#text-languages)。

### <a name="text-languages"></a>文本语言

| 文本语言       | 语言代码 |
| :------------------ | :-----------: |
| 阿拉伯语              |     `ar`      |
| 简体中文  |   `zh-Hans`   |
| 中文(繁体) |   `zh-Hant`   |
| 英语             |     `en`      |
| 法语              |     `fr`      |
| 德语              |     `de`      |
| Hindi               |     `hi`      |
| 朝鲜语              |     `ko`      |
| 俄语             |     `ru`      |
| 西班牙语             |     `es`      |

<!-- ## Speaker Recognition -->

<!--Customized in MC-->
## <a name="more-languages"></a>更多语言

> [!NOTE]
> 如果前面的列表中不支持你指定的语言，请参考[主权云支持的区域设置](sovereign-clouds.md)和[联系支持](https://www.azure.cn/support/contact/)，可以根据要求部署该语言。
> 有关更多语言支持，请参阅[此处](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support)的支持的语言列表
## <a name="next-steps"></a>后续步骤

* [创建一个 Azure 试用帐户](https://www.azure.cn/home/features/cognitive-services/)
* [了解如何在 C# 中识别语音](./get-started-speech-to-text.md?pivots=programming-language-chsarp)

