---
title: 人脸 JavaScript 客户端库快速入门
description: 使用适用于 JavaScript 的人脸客户端库来检测人脸、查找相似的人脸（按图像进行人脸搜索）、识别人脸（人脸识别搜索）并迁移人脸数据。
services: cognitive-services
author: v-jaswel
manager: chrhoder
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: include
ms.date: 03/08/2021
ms.author: v-johya
ms.openlocfilehash: df8e064028ca943a20d64e4850a57526b519ead3
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766403"
---
## <a name="quickstart-face-client-library-for-javascript"></a>快速入门：适用于 JavaScript 的人脸客户端库

开始使用适用于 JavaScript 的人脸客户端库进行人脸识别。 请按照以下步骤安装程序包并试用基本任务的示例代码。 通过人脸服务，可以访问用于检测和识别图像中的人脸的高级算法。

使用适用于 JavaScript 的人脸客户端库可以：

* [检测图像中的人脸](#detect-faces-in-an-image)
* [查找相似人脸](#find-similar-faces)
* [创建人员组](#create-a-person-group)
* [识别人脸](#identify-a-face)

[参考文档](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/) | [库源代码](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-face) | [包 (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-face) | [示例](https://docs.microsoft.com/samples/browse/?products=azure&term=face&languages=javascript)

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)
* 最新版本的 [Node.js](https://nodejs.org/en/)
* 拥有 Azure 订阅后，请在 Azure 门户中[创建人脸资源](https://portal.azure.cn/#create/Microsoft.CognitiveServicesFace)，以获取密钥和终结点。 部署后，单击“转到资源”。
    * 需要从创建的资源获取密钥和终结点，以便将应用程序连接到人脸 API。 你稍后会在快速入门中将密钥和终结点粘贴到下方的代码中。
    * 可以使用免费定价层 (`F0`) 试用该服务，然后再升级到付费层进行生产。

## <a name="setting-up"></a>设置

### <a name="create-a-new-nodejs-application"></a>创建新的 Node.js 应用程序

在控制台窗口（例如 cmd、PowerShell 或 Bash）中，为应用创建一个新目录并导航到该目录。 

```console
mkdir myapp && cd myapp
```

运行 `npm init` 命令以使用 `package.json` 文件创建一个 node 应用程序。 

```console
npm init
```

### <a name="install-the-client-library"></a>安装客户端库 

安装 `ms-rest-azure` 和 `azure-cognitiveservices-face` NPM 包:

```console
npm install @azure/cognitiveservices-face @azure/ms-rest-js
```

应用的 `package.json` 文件将使用依赖项进行更新。

创建一个名为 `index.js` 的文件，并导入以下库：

> [!TIP]
> 想要立即查看整个快速入门代码文件？ 可以在 [GitHub]() 上找到它，其中包含此快速入门中的代码示例。

```javascript
const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");
```

为资源的 Azure 终结点和密钥创建变量。 

> [!IMPORTANT]
> 转到 Azure 门户。 如果你在“先决条件”部分创建的人脸资源部署成功，请单击“后续步骤”下的“转到资源”按钮  。 在资源的“密钥和终结点”页的“资源管理”下可以找到密钥和终结点 。 
>
> 完成后，请记住将密钥从代码中删除，并且永远不要公开发布该密钥。 对于生产环境，请考虑使用安全的方法来存储和访问凭据。 有关详细信息，请参阅认知服务[安全性](/cognitive-services/cognitive-services-security)文章。

```javascript
key = "<paste-your-face-key-here>"
endpoint = "<paste-your-face-endpoint-here>"
```

## <a name="object-model"></a>对象模型

以下类和接口将处理人脸 .NET 客户端库的某些主要功能：

|名称|说明|
|---|---|
|[FaceClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/faceclient) | 此类代表使用人脸服务的授权，使用所有人脸功能时都需要用到它。 请使用你的订阅信息实例化此类，然后使用它来生成其他类的实例。 |
|[人脸](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/face)|此类处理可对人脸执行的基本检测和识别任务。 |
|[DetectedFace](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/detectedface)|此类代表已从图像中的单个人脸检测到的所有数据。 可以使用它来检索有关人脸的详细信息。|
|[FaceList](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/facelist)|此类管理云中存储的 **FaceList** 构造，这些构造存储各种不同的人脸。 |
|[PersonGroupPerson](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroupperson)| 此类管理云中存储的 **Person** 构造，这些构造存储属于单个人员的一组人脸。|
|[PersonGroup](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroup)| 此类管理云中存储的 **PersonGroup** 构造，这些构造存储各种不同的 **Person** 对象。 |

## <a name="code-examples"></a>代码示例

以下代码片段演示如何使用适用于 .NET 的人脸客户端库执行以下任务：

* [对客户端进行身份验证](#authenticate-the-client)
* [检测图像中的人脸](#detect-faces-in-an-image)
* [查找相似人脸](#find-similar-faces)
* [创建人员组](#create-a-person-group)
* [识别人脸](#identify-a-face)

> [!TIP]
> 想要立即查看整个快速入门代码文件？ 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/Face/sdk_quickstart.js) 上找到它，其中包含此快速入门中的代码示例。

## <a name="authenticate-the-client"></a>验证客户端

使用终结点和密钥实例化某个客户端。 使用密钥创建 [ApiKeyCredentials](https://docs.microsoft.comhttps://docs.microsoft.com/javascript/api/@azure/ms-rest-js/apikeycredentials) 对象，并使用该对象在终结点中创建 [FaceClient](https://docs.microsoft.comhttps://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/faceclient) 对象 。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

## <a name="declare-global-values-and-helper-function"></a>声明全局值和 helper 函数

你稍后将添加的一些人脸操作需要以下全局值。

该 URL 指向示例图像的文件夹。 UUID 将用作要创建的 PersonGroup 的名称和 ID。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

你将使用以下函数来等待 PersonGroup 的训练完成。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

## <a name="detect-faces-in-an-image"></a>在图像中检测人脸

### <a name="get-detected-face-objects"></a>获取检测到的人脸对象

创建新方法以检测人脸。 `DetectFaceExtract` 方法处理给定 URL 处的三个图像，并在程序内存中创建 [DetectedFace](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/detectedface) 对象的列表。 **[FaceAttributeType](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/faceattributetype)** 值列表指定要提取的特征。 

然后，`DetectFaceExtract` 方法会分析并输出每个检测到的人脸的属性数据。 每个属性必须在原始人脸检测 API 调用中单独指定（在 **[FaceAttributeType](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/faceattributetype)** 列表中）。 下面的代码处理每个属性，但你可能只需要使用一个或一些属性。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

> [!TIP]
> 还可以检测本地图像中的人脸。 请参阅 [Face](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/face) 方法，如 [DetectWithStreamAsync](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/face#detectWithStream_msRest_HttpRequestBody__FaceDetectWithStreamOptionalParams__ServiceCallback_DetectedFace____)。

## <a name="find-similar-faces"></a>查找相似人脸

以下代码采用检测到的单个人脸（源），并搜索其他一组人脸（目标），以找到匹配项（按图像进行人脸搜索）。 找到匹配项后，它会将匹配的人脸的 ID 输出到控制台。

### <a name="detect-faces-for-comparison"></a>检测人脸以进行比较

首先定义另一个人脸检测方法。 需要先检测图像中的人脸，然后才能对其进行比较；此检测方法已针对比较操作进行优化。 它不会提取以上部分所示的详细人脸属性，而是使用另一个识别模型。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

### <a name="find-matches"></a>查找匹配项

以下方法检测一组目标图像和单个源图像中的人脸。 然后，它将比较这些人脸，并查找与源图像类似的所有目标图像。 最后，该方法会将匹配项详细信息输出到控制台。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

## <a name="identify-a-face"></a>识别人脸

[识别](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/face#identify_string____FaceIdentifyOptionalParams__ServiceCallback_IdentifyResult____)操作采用一个人（或多个人）的图像，并在图像中查找每个人脸的标识（人脸识别搜索）。 它将每个检测到的人脸与某个 [PersonGroup](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroup)（面部特征已知的不同 [Person](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/person) 对象的数据库）进行比较。 为了执行“识别”操作，你首先需要创建并训练 [PersonGroup](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroup)。

### <a name="add-faces-to-person-group"></a>将人脸添加到人员组

创建以下函数以将人脸添加到 [PersonGroup](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroup)。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

### <a name="wait-for-training-of-person-group"></a>等待人员组的训练

创建以下 helper 函数来等待人员组完成训练。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

### <a name="create-a-person-group"></a>创建人员组

以下代码：
- 创建 [PersonGroup](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroup)
- 通过调用你以前定义的 `AddFacesToPersonGroup` 将人脸添加到人员组。
- 训练该人员组。
- 识别该人员组中的人脸。

现已准备好在验证、识别或分组操作中使用此 **Person** 组及其关联的 **Person** 对象。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

> [!TIP]
> 你还可以从本地图像创建 PersonGroup。 请参阅 [PersonGroupPerson](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroupperson) 方法，如 [AddFaceFromStream](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/persongroupperson#addFaceFromStream_string__string__msRest_HttpRequestBody__Models_PersonGroupPersonAddFaceFromStreamOptionalParams_)。

## <a name="main"></a>Main

最后，创建并调用 `main` 函数。

```js
'use strict';

const msRest = require("@azure/ms-rest-js");
const Face = require("@azure/cognitiveservices-face");
const uuid = require("uuid/v4");

key = "<paste-your-[product-name]-key-here>"
endpoint = "<paste-your-[product-name]-endpoint-here>"

// <credentials>
const credentials = new msRest.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': key } });
const client = new Face.FaceClient(credentials, endpoint);
// </credentials>

// <globals>
const image_base_url = "https://csdx.blob.core.chinacloudapi.cn/resources/Face/Images/";
const person_group_id = uuid();
// </globals>

// <helpers>
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
// </helpers>

// <detect>
async function DetectFaceExtract() {
    console.log("========DETECT FACES========");
    console.log();

    // Create a list of images
    const image_file_names = [
        "detection1.jpg",    // single female with glasses
        // "detection2.jpg", // (optional: single man)
        // "detection3.jpg", // (optional: single male construction worker)
        // "detection4.jpg", // (optional: 3 people at cafe, 1 is blurred)
        "detection5.jpg",    // family, woman child man
        "detection6.jpg"     // elderly couple, male female
    ];

// NOTE await does not work properly in for, forEach, and while loops. Use Array.map and Promise.all instead.
    await Promise.all (image_file_names.map (async function (image_file_name) {
        let detected_faces = await client.face.detectWithUrl(image_base_url + image_file_name,
            {
                returnFaceAttributes: ["Accessories","Age","Blur","Emotion","Exposure","FacialHair","Gender","Glasses","Hair","HeadPose","Makeup","Noise","Occlusion","Smile"],
                // We specify detection model 1 because we are retrieving attributes.
                detectionModel: "detection_01"
            });
        console.log (detected_faces.length + " face(s) detected from image " + image_file_name + ".");
        console.log("Face attributes for face(s) in " + image_file_name + ":");

// Parse and print all attributes of each detected face.
        detected_faces.forEach (async function (face) {
            // Get the bounding box of the face
            console.log("Bounding box:\n  Left: " + face.faceRectangle.left + "\n  Top: " + face.faceRectangle.top + "\n  Width: " + face.faceRectangle.width + "\n  Height: " + face.faceRectangle.height);

            // Get the accessories of the face
            let accessories = face.faceAttributes.accessories.join();
            if (0 === accessories.length) {
                console.log ("No accessories detected.");
            }
            else {
                console.log ("Accessories: " + accessories);
            }

            // Get face other attributes
            console.log("Age: " + face.faceAttributes.age);
            console.log("Blur: " + face.faceAttributes.blur.blurLevel);

            // Get emotion on the face
            let emotions = "";
            let emotion_threshold = 0.0;
            if (face.faceAttributes.emotion.anger > emotion_threshold) { emotions += "anger, "; }
            if (face.faceAttributes.emotion.contempt > emotion_threshold) { emotions += "contempt, "; }
            if (face.faceAttributes.emotion.disgust > emotion_threshold) { emotions +=  "disgust, "; }
            if (face.faceAttributes.emotion.fear > emotion_threshold) { emotions +=  "fear, "; }
            if (face.faceAttributes.emotion.happiness > emotion_threshold) { emotions +=  "happiness, "; }
            if (face.faceAttributes.emotion.neutral > emotion_threshold) { emotions +=  "neutral, "; }
            if (face.faceAttributes.emotion.sadness > emotion_threshold) { emotions +=  "sadness, "; }
            if (face.faceAttributes.emotion.surprise > emotion_threshold) { emotions +=  "surprise, "; }
            if (emotions.length > 0) {
                console.log ("Emotions: " + emotions.slice (0, -2));
            }
            else {
                console.log ("No emotions detected.");
            }
            
            // Get more face attributes
            console.log("Exposure: " + face.faceAttributes.exposure.exposureLevel);
            if (face.faceAttributes.facialHair.moustache + face.faceAttributes.facialHair.beard + face.faceAttributes.facialHair.sideburns > 0) {
                console.log("FacialHair: Yes");
            }
            else {
                console.log("FacialHair: No");
            }
            console.log("Gender: " + face.faceAttributes.gender);
            console.log("Glasses: " + face.faceAttributes.glasses);

            // Get hair color
            var color = "";
            if (face.faceAttributes.hair.hairColor.length === 0) {
                if (face.faceAttributes.hair.invisible) { color = "Invisible"; } else { color = "Bald"; }
            }
            else {
                color = "Unknown";
                var highest_confidence = 0.0;
                face.faceAttributes.hair.hairColor.forEach (function (hair_color) {
                    if (hair_color.confidence > highest_confidence) {
                        highest_confidence = hair_color.confidence;
                        color = hair_color.color;
                    }
                });
            }
            console.log("Hair: " + color);

            // Get more attributes
            console.log("Head pose:");
            console.log("  Pitch: " + face.faceAttributes.headPose.pitch);
            console.log("  Roll: " + face.faceAttributes.headPose.roll);
            console.log("  Yaw: " + face.faceAttributes.headPose.yaw);
 
            console.log("Makeup: " + ((face.faceAttributes.makeup.eyeMakeup || face.faceAttributes.makeup.lipMakeup) ? "Yes" : "No"));
            console.log("Noise: " + face.faceAttributes.noise.noiseLevel);

            console.log("Occlusion:");
            console.log("  Eye occluded: " + (face.faceAttributes.occlusion.eyeOccluded ? "Yes" : "No"));
            console.log("  Forehead occluded: " + (face.faceAttributes.occlusion.foreheadOccluded ? "Yes" : "No"));
            console.log("  Mouth occluded: " + (face.faceAttributes.occlusion.mouthOccluded ? "Yes" : "No"));

            console.log("Smile: " + face.faceAttributes.smile);
            console.log();
        });
    }));
}
// </detect>

// <recognize>
async function DetectFaceRecognize(url) {
    // Detect faces from image URL. Since only recognizing, use the recognition model 1.
    // We use detection model 2 because we are not retrieving attributes.
    let detected_faces = await client.face.detectWithUrl(url,
        {
            detectionModel: "detection_02",
            recognitionModel: "recognition_03"
        });
    return detected_faces;
}
// </recognize>

// <find_similar>
async function FindSimilar() {
    console.log("========FIND SIMILAR========");
    console.log();

    const source_image_file_name = "findsimilar.jpg";
    const target_image_file_names = [
        "Family1-Dad1.jpg",
        "Family1-Daughter1.jpg",
        "Family1-Mom1.jpg",
        "Family1-Son1.jpg",
        "Family2-Lady1.jpg",
        "Family2-Man1.jpg",
        "Family3-Lady1.jpg",
        "Family3-Man1.jpg"
    ];

    let target_face_ids = (await Promise.all (target_image_file_names.map (async function (target_image_file_name) {
        // Detect faces from target image url.
        var faces = await DetectFaceRecognize(image_base_url + target_image_file_name);
        console.log(faces.length + " face(s) detected from image: " +  target_image_file_name + ".");
        return faces.map (function (face) { return face.faceId });;
    }))).flat();

    // Detect faces from source image url.
    let detected_faces = await DetectFaceRecognize(image_base_url + source_image_file_name);

    // Find a similar face(s) in the list of IDs. Comapring only the first in list for testing purposes.
    let results = await client.face.findSimilar(detected_faces[0].faceId, { faceIds : target_face_ids });
    results.forEach (function (result) {
        console.log("Faces from: " + source_image_file_name + " and ID: " + result.faceId + " are similar with confidence: " + result.confidence + ".");
    });
    console.log();
}
// </find_similar>

// <add_faces>
async function AddFacesToPersonGroup(person_dictionary, person_group_id) {
    console.log ("Adding faces to person group...");
    // The similar faces will be grouped into a single person group person.
    
    await Promise.all (Object.keys(person_dictionary).map (async function (key) {
        const value = person_dictionary[key];

        // Wait briefly so we do not exceed rate limits.
        await sleep (1000);

        let person = await client.personGroupPerson.create(person_group_id, { name : key });
        console.log("Create a person group person: " + key + ".");

        // Add faces to the person group person.
        await Promise.all (value.map (async function (similar_image) {
            console.log("Add face to the person group person: (" + key + ") from image: " + similar_image + ".");
            await client.personGroupPerson.addFaceFromUrl(person_group_id, person.personId, image_base_url + similar_image);
        }));
    }));

    console.log ("Done adding faces to person group.");
}
// </add_faces>

// <wait_for_training>
async function WaitForPersonGroupTraining(person_group_id) {
    // Wait so we do not exceed rate limits.
    console.log ("Waiting 10 seconds...");
    await sleep (10000);
    let result = await client.personGroup.getTrainingStatus(person_group_id);
    console.log("Training status: " + result.status + ".");
    if (result.status !== "succeeded") {
        await WaitForPersonGroupTraining(person_group_id);
    }
}
// </wait_for_training>

/* NOTE This function might not work with the free tier of the Face service
because it might exceed the rate limits. If that happens, try inserting calls
to sleep() between calls to the Face service.
*/
// <identify>
async function IdentifyInPersonGroup() {
    console.log("========IDENTIFY FACES========");
    console.log();

// Create a dictionary for all your images, grouping similar ones under the same key.
    const person_dictionary = {
        "Family1-Dad" : ["Family1-Dad1.jpg", "Family1-Dad2.jpg"],
        "Family1-Mom" : ["Family1-Mom1.jpg", "Family1-Mom2.jpg"],
        "Family1-Son" : ["Family1-Son1.jpg", "Family1-Son2.jpg"],
        "Family1-Daughter" : ["Family1-Daughter1.jpg", "Family1-Daughter2.jpg"],
        "Family2-Lady" : ["Family2-Lady1.jpg", "Family2-Lady2.jpg"],
        "Family2-Man" : ["Family2-Man1.jpg", "Family2-Man2.jpg"]
    };

    // A group photo that includes some of the persons you seek to identify from your dictionary.
    let source_image_file_name = "identification1.jpg";

    // Create a person group. 
    console.log("Creating a person group with ID: " + person_group_id);
    await client.personGroup.create(person_group_id, { name : person_group_id, recognitionModel : "recognition_03" });

    await AddFacesToPersonGroup(person_dictionary, person_group_id);

    // Start to train the person group.
    console.log();
    console.log("Training person group: " + person_group_id + ".");
    await client.personGroup.train(person_group_id);

    await WaitForPersonGroupTraining(person_group_id);
    console.log();

    // Detect faces from source image url.
    let face_ids = (await DetectFaceRecognize(image_base_url + source_image_file_name)).map (face => face.faceId);

// Identify the faces in a person group.
    let results = await client.face.identify(face_ids, { personGroupId : person_group_id});
    await Promise.all (results.map (async function (result) {
        let person = await client.personGroupPerson.get(person_group_id, result.candidates[0].personId);
        console.log("Person: " + person.name + " is identified for face in: " + source_image_file_name + " with ID: " + result.faceId + ". Confidence: " + result.candidates[0].confidence + ".");
    }));
    console.log();
}
// </identify>

// <main>
async function main() {
    await DetectFaceExtract();
    await FindSimilar();
    await IdentifyInPersonGroup();
    console.log ("Done.");
}
main();
// </main>
```

## <a name="run-the-application"></a>运行应用程序

在快速入门文件中使用 `node` 命令运行应用程序。

```console
node index.js
```

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与之相关联的任何其他资源。

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>后续步骤

此快速入门介绍了如何使用适用于 JavaScript 的人脸客户端库来执行基本的人脸识别任务。 接下来，请在参考文档中详细了解该库。

> [!div class="nextstepaction"]
> [人脸 API 参考 (JavaScript)](https://docs.microsoft.comhttps://docs.microsoft.com/javascript/api/@azure/cognitiveservices-face/)

* [什么是人脸服务？](../../overview.md)
* 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/Face/sdk_quickstart.js) 上找到此示例的源代码。

