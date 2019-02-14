---
title: クイック スタート:画像から顔の表情を認識する - Emotion API (Java)
description: Emotion API with Java for Android の使用をすぐに開始するために役立つ情報とコード サンプルを提供します。
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9755507aa858289daff7f297c4013b27f8258b61
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233525"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>クイック スタート:画像から顔の表情を認識するアプリを作成する。

> [!IMPORTANT]
> Emotion API は、2019 年 2 月 15 日に非推奨となる予定です。 現在は、[Face API](https://docs.microsoft.com/azure/cognitive-services/face/) の一部として感情認識機能が一般提供されています。 

この記事では、Emotion API Android クライアント ライブラリの[感情認識メソッド](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)の使用をすばやく開始するために役立つ情報とコード サンプルを提供します。 サンプルでは、Java を使用して、人物の表している感情を認識する方法を示します。

## <a name="prerequisites"></a>前提条件
* Emotion API Java for Android SDK を[ここから](https://github.com/Microsoft/Cognitive-emotion-android)入手します。
* 無料のサブスクリプション キーを[ここから](https://azure.microsoft.com/try/cognitive-services/)入手します。

## <a name="recognize-emotions-java-for-android-example-request"></a>Recognize Emotions Java for Android サンプルの要求

```java
// // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;

public class Main
{
    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
            //   URL below with "westcentralus".
            URIBuilder uriBuilder = new URIBuilder("https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize");

            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers. Replace the example key below with your valid subscription key.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

            // Request body. Replace the example URL below with the URL of the image you want to analyze.
            StringEntity reqEntity = new StringEntity("{ \"url\": \"http://example.com/images/test.jpg\" }");
            request.setEntity(reqEntity);

            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                System.out.println(EntityUtils.toString(entity));
            }
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Recognize Emotions サンプルの応答
呼び出しが成功すると、顔エントリとそれらに関連付けられた感情スコアの配列が、顔の四角形サイズの降順で返されます。 空の応答は、顔が検出されなかったことを示します。 感情エントリには次のフィールドが含まれます。
* faceRectangle - 画像内の顔の四角形の位置。
* scores - 画像内のそれぞれの顔の Emotion スコア。

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
