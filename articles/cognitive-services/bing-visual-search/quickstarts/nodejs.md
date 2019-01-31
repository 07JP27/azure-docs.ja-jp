---
title: クイック スタート:Bing Visual Search REST API と Node.js を使用して画像に関する分析情報を取得する
titleSuffix: Azure Cognitive Services
description: Bing Visual Search API に画像をアップロードし、画像に関する分析情報を取得する方法について説明します。
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 23c3e167f2fd1544a42c98946f4ae95d3451dafe
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180621"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>クイック スタート:Bing Visual Search REST API と Node.js を使用して画像に関する分析情報を取得する

このクイック スタートを使用すると、Bing Visual Search API への最初の呼び出しを行い、検索結果を表示することができます。 このシンプルな JavaScript アプリケーションは、API に画像をアップロードし、それについて返された情報を表示するというものです。 このアプリケーションは JavaScript で記述されていますが、API はほとんどのプログラミング言語と互換性のある RESTful Web サービスです。

ローカルの画像をアップロードする際には、フォーム データに Content-Disposition ヘッダーが含まれている必要があります。 その `name` パラメーターには "image" を設定する必要があり、`filename` パラメーターには任意の文字列を設定できます。 フォームの内容は、イメージのバイナリです。 アップロードできるイメージの最大サイズは、1 MB です。

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>前提条件

* [Node.js](https://nodejs.org/en/download/)
* JavaScript 用の要求モジュール
    * このモジュールは、`npm install request` を使用してインストールできます
* form-data モジュール
    * このモジュールは、`npm install form-data` を使用してインストールできます


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="initialize-the-application"></a>アプリケーションを初期化する

1. 好みの IDE またはエディターで新しい JavaScript ファイルを作成し、次の要件を設定します。

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API エンドポイント、サブスクリプション キー、および画像へのパスを格納する変数を作成します。

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. API からの応答を出力するための、`requestCallback()` という関数を作成します。

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>検索要求を構築して送信する

1. `FormData()` を使用して新しいフォーム データを作成し、`fs.createReadStream()` を使用して、そのフォーム データに画像のパスを追加します。
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. 要求ライブラリを使用して画像をアップロードし、`requestCallback()` を呼び出して応答を出力します。 要求ヘッダーには必ずサブスクリプション キーを追加してください。 

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Custom Search Web アプリの作成](../tutorial-bing-visual-search-single-page-app.md)
