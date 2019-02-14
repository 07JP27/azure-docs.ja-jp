---
title: クイック スタート:画像から顔の表情を認識する - Emotion API (JavaScript)
titlesuffix: Azure Cognitive Services
description: Emotion API と JavaScript の使用をすぐに開始するために役立つ情報とコード サンプルを提供します。
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 96388bb3cf2b73000fe871bc9e08720038c5c5a3
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/13/2019
ms.locfileid: "56234698"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>クイック スタート:画像から顔の表情を認識するアプリを作成する。

> [!IMPORTANT]
> Emotion API は、2019 年 2 月 15 日に非推奨となる予定です。 現在は、[Face API](https://docs.microsoft.com/azure/cognitive-services/face/) の一部として感情認識機能が一般提供されています。 

この記事では、[Emotion API Recognize メソッド](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) と JavaScript を使って、画像内の 1 人以上の個人が表す感情を認識するための、役立つ情報とコード サンプルを提供します。

## <a name="prerequisite"></a>前提条件
* [ここ](https://azure.microsoft.com/try/cognitive-services/)から無料サブスクリプション キーを取得するか、Azure サブスクリプションがある場合は、Emotion API リソースを作成し、そこから自分のサブスクリプション キーとエンドポイントを取得します。

![Emotion API リソースを作成する](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>感情認識 JavaScript の要求の例

次をコピーし、`test.html` などのファイルに保存します。 要求 `url` をサブスクリプション キーの取得元の場所を使用するように変更し、"Ocp-Apim-Subscription-Key" の値を有効なサブスクリプション キーに置き換えてください。 これらはそれぞれ、Azure portal の Emotion API リソースの概要のセクションとキーのセクションにあります。

![API エンドポイント](../Images/api-url.png)

![API サブスクリプション キー](../Images/keys.png)

要求本文を、使用する画像の場所に変更します。 サンプルを実行するには、お使いのブラウザーにファイルをドラッグ アンド ドロップします。

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<h2>Face Rectangle</h2>
<ul id="faceRectangle">
<!-- Will populate list with response content -->
</ul>

<h2>Emotions</h2>
<ul id="scores">
<!-- Will populate list with response content -->
</ul>

<body>

<script type="text/javascript">
    $(function() {
        // No query string parameters for this API call.
        var params = { };

        $.ajax({
            // NOTE: You must use the same location in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
            //   URL below with "westcentralus".
            url: "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers, also supports "application/octet-stream"
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","<your subscription key>");
            },
            type: "POST",
            // Request body
            data: '{"url": "<your image url>"}',
        }).done(function(data) {
            // Get face rectangle dimensions
            var faceRectangle = data[0].faceRectangle;
            var faceRectangleList = $('#faceRectangle');

            // Append to DOM
            for (var prop in faceRectangle) {
                faceRectangleList.append("<li> " + prop + ": " + faceRectangle[prop] + "</li>");
            }

            // Get emotion confidence scores
            var scores = data[0].scores;
            var scoresList = $('#scores');

            // Append to DOM
            for(var prop in scores) {
                scoresList.append("<li> " + prop + ": " + scores[prop] + "</li>")
            }
        }).fail(function(err) {
            alert("Error: " + JSON.stringify(err));
        });
    });
</script>
</body>
</html>
```

## <a name="recognize-emotions-sample-response"></a>感情認識の応答例
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
