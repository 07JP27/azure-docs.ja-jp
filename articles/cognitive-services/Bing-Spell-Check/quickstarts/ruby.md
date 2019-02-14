---
title: クイック スタート:Bing Spell Check API (Ruby)
titlesuffix: Azure Cognitive Services
description: Bing Spell Check API をすぐに使い始めるのに役立つ情報とコード サンプルを提供します。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: d3c273c7e5774e4f6d5b6984b77ff76bfb041343
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873174"
---
# <a name="quickstart-for-bing-spell-check-api-with-ruby"></a>Bing Spell Check API のクイック スタート (Ruby) 

この記事では、Ruby で [Bing Spell Check API](https://azure.microsoft.com/services/cognitive-services/spell-check/)  を使用する方法について説明します。 Spell Check API は、認識できない単語のリストを置換候補と共に返します。 一般に、この API にテキストを送信し、テキスト内の単語を置換候補に置き換えるか、その判断をユーザー自身が行えるように置換候補をアプリケーションのユーザーに表示することになるでしょう。 この記事では、"Hollo, wrld!" というテキストを含んだ要求を送信する方法について説明します。 置換候補は "Hello" と "world" です。

## <a name="prerequisites"></a>前提条件

このコードを実行するには、[Ruby 2.4](https://www.ruby-lang.org/en/downloads/) 以降が必要です。

[Cognitive Services API アカウント](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)と **Bing Spell Check API v7** を取得している必要があります。 このクイック スタートには[無料試用版](https://azure.microsoft.com/try/cognitive-services/#lang)で十分です。 無料試用版を起動するとき、アクセス キーを入力する必要があります。または、Azure ダッシュボードの有料サブスクリプション キーを使用できます。 「[Cognitive Services の価格 - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)」もご覧ください。

## <a name="get-spell-check-results"></a>スペル チェックの結果を取得する

1. お気に入りの IDE で新しい Ruby プロジェクトを作成します。
2. 下記のコードを追加します。
3. `key` の値を、お使いのサブスクリプションで有効なアクセス キーに置き換えます。
4. プログラムを実行します。

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = 'https://api.cognitive.microsoft.com'
path = '/bing/v7.0/spellcheck?'
params = 'mkt=en-us&mode=proof'

uri = URI(uri + path + params)
uri.query = URI.encode_www_form({
    # Request parameters
    'text' => 'Hollo, wrld!'
})

# NOTE: Replace this example key with a valid subscription key.
key = 'ENTER KEY HERE'

# The headers in the following example 
# are optional but should be considered as required:
#
# X-MSEdge-ClientIP: 999.999.999.999  
# X-Search-Location: lat: +90.0000000000000;long: 00.0000000000000;re:100.000000000000
# X-MSEdge-ClientID: <Client ID from Previous Response Goes Here>
#
# See below for more information.

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/x-www-form-urlencoded"

request['Ocp-Apim-Subscription-Key'] = key

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

result = JSON.pretty_generate(JSON.parse(response.body))
puts result
```

**応答**

成功した応答は、次の例に示すように JSON で返されます。 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Bing Spell Check のチュートリアル](../tutorials/spellcheck.md)

## <a name="see-also"></a>関連項目

- [Bing Spell Check の概要](../proof-text.md)
- [Bing Spell Check API v7 リファレンス](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
