---
title: 训练和部署自定义语音识别模型 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何训练和部署自定义语音识别模型。 训练语音转文本模型可以提高 Microsoft 基线模型或某个自定义模型的识别准确度。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 11/11/2020
ms.date: 03/08/2021
ms.author: v-johya
ms.openlocfilehash: 156bbcdad5bba377025b8a63c64447bcfa2daddb
ms.sourcegitcommit: ec127596b5c56f8ba4d452c39a7b44510b140ed4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103212738"
---
# <a name="train-and-deploy-a-custom-speech-model"></a>训练和部署自定义语音识别模型

本文介绍如何训练和部署自定义语音识别模型。 训练语音转文本模型可以提高 Microsoft 基线模型的识别准确度。 使用人为标记的听录内容和相关文本训练模型。 这些数据集以及以前上传的音频数据用于优化和训练语音转文本模型。

## <a name="use-training-to-resolve-accuracy-problems"></a>通过训练解决准确度问题

如果遇到基本模型识别问题，则可以使用人为标记的脚本和相关数据来训练自定义模型和帮助提高准确度。 使用此表可确定应使用哪个数据集来解决问题：

| 使用案例 | 数据类型 |
| -------- | --------- |
| 提高特定于行业的词汇和语法（例如医疗术语或 IT 行话）的识别准确度 | 相关的文本（句子/言语） |
| 定义发音不标准的字词或术语（例如产品名或首字母缩写）的语音和显示形式 | 相关的文本（发音） |
| 提高说话风格、口音或特定背景杂音的识别准确度 | 音频和人为标记的听录内容 |

## <a name="train-and-evaluate-a-model"></a>训练和评估模型

训练模型的第一步是上传训练数据。 请参阅[准备和测试数据](./how-to-custom-speech-test-and-train.md)以获取分步说明，了解如何准备人为标记的听录内容和相关文本（言语和发音）。 上传训练数据以后，请按以下说明开始训练模型：

<!---Not available in MC: dedicated hardware->
1. Sign in to the [Custom Speech portal](https://speech.azure.cn/customspeech).
2. Go to **Speech-to-text** > **Custom Speech** > **[name of project]** > **Training**.
3. Select **Train model**.
4. Give your training a **Name** and **Description**.
5. In the **Scenario and Baseline model** list, select the scenario that best fits your domain. If you're not sure which scenario to choose, select **General**. The baseline model is the starting point for training. The latest model is usually the best choice.
6. On the **Select training data** page, choose one or more related text datasets or audio + human-labeled transcription datasets that you want to use for training.

> [!NOTE]
> When you train a new model, start with related text; training with audio + human-labeled transcription might take much longer **(up to [several days](how-to-custom-speech-evaluate-data.md#add-audio-with-human-labeled-transcripts)**).

> [!NOTE]
> Not all base models support training with audio. If a base model does not support it, the Speech service will only use the text from the transcripts and ignore the audio. See [Language support](language-support.md#speech-to-text) for a list of base models that support training with audio data.

7. After training is complete, you can do accuracy testing on the newly trained model. This step is optional.
8. Select **Create** to build your custom model.

The **Training** table displays a new entry that corresponds to the new model. The table also displays the status: **Processing**, **Succeeded**, or **Failed**.

See the [how-to](how-to-custom-speech-evaluate-data.md) on evaluating and improving Custom Speech model accuracy. If you choose to test accuracy, it's important to select an acoustic dataset that's different from the one you used with your model to get a realistic sense of the model's performance.

> [!NOTE]
> Both base models and custom  models can be used only up to a certain date (see [Model lifecycle](custom-speech-overview.md#model-lifecycle)). Speech Studio shows this date in the **Expiration** column for each model and endpoint. After that date request to an endpoint or to batch transcription  might fail or fall back to base model.
>
> Retrain your model using the then most recent base model to benefit from accuracy improvements and to avoid that your model expires.

## Deploy a custom model

After you upload and inspect data, evaluate accuracy, and train a custom model, you can deploy a custom endpoint to use with your apps, tools, and products. 

To create a custom endpoint, sign in to the [Custom Speech portal](https://speech.azure.cn/customspeech). Select **Deployment** in the **Custom Speech** menu at the top of the page. If this is your first run, you'll notice that there are no endpoints listed in the table. After you create an endpoint, you use this page to track each deployed endpoint.

Next, select **Add endpoint** and enter a **Name** and **Description** for your custom endpoint. Then select the custom model that you want to associate with the endpoint.  You can also enable logging from this page. Logging allows you to monitor endpoint traffic. If logging is disabled, traffic won't be stored.

![Screenshot that shows the New endpoint page.](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Don't forget to accept the terms of use and pricing details.

Next, select **Create**. This action returns you to the **Deployment** page. The table now includes an entry that corresponds to your custom endpoint. The endpoint’s status shows its current state. It can take up to 30 minutes to instantiate a new endpoint using your custom models. When the status of the deployment changes to **Complete**, the endpoint is ready to use.

After your endpoint is deployed, the endpoint name appears as a link. Select the link to see information specific to your endpoint, like the endpoint key, endpoint URL, and sample code. Take a note of the expiration date and update the endpoint's model before that date to ensure uninterrupted service.

## View logging data

Logging data is available for export if you go to the endpoint's page under **Deployments**.
> [!NOTE]
>Logging data is available for 30 days on Microsoft-owned storage. It will be removed afterwards. If a customer-owned storage account is linked to the Cognitive Services subscription, the logging data won't be automatically deleted.

## Next steps

* [Learn how to use your custom model](how-to-specify-source-language.md)

## Additional resources

- [Prepare and test your data](./how-to-custom-speech-test-and-train.md)
- [Inspect your data](how-to-custom-speech-inspect-data.md)
- [Evaluate your data](how-to-custom-speech-evaluate-data.md)

