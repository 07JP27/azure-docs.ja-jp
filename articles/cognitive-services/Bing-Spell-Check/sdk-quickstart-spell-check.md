---
title: クイック スタート:Bing Spell Check SDK、C#
titlesuffix: Azure Cognitive Services
description: Spell Check SDK コンソール アプリケーションのセットアップ
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 01/30/2018
ms.author: v-gedod
ms.openlocfilehash: 9e7b2bfd25279420272132ef54e4c970333e49c0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882048"
---
# <a name="quickstart-bing-spell-check-sdk-with-c"></a>クイック スタート:Bing Spell Check SDK (C#)

Bing Spell Check SDK には、スペル チェックに関する REST API の機能が含まれています。

## <a name="application-dependencies"></a>アプリケーションの依存関係
**[検索]** で [Cognitive Services のアクセス キー](https://azure.microsoft.com/try/cognitive-services/)を取得します。  「[Cognitive Services の価格 - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)」もご覧ください。

Bing Spell Check SDK を使用してコンソール アプリケーションを設定するには、Visual Studio のソリューション エクスプローラーで `Manage NuGet Packages` オプションを参照します。 `Microsoft.Azure.CognitiveServices.Language.SpellCheck` パッケージを追加します。

[SpellCheck SDK パッケージ](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.SpellCheck/1.2.0)をインストールすると、次の項目を含む依存関係もインストールされます。

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="spell-check-client"></a>スペル チェック クライアント

`SpellCheckClient` クライアントのインスタンスを作成するには、次のディレクティブを追加します。

```cs
using Microsoft.Azure.CognitiveServices.Language.SpellCheck;
```

次に、クライアントをインスタンス化します。

```cs
var client = new SpellCheckClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```

このクライアントを使用してテキストのスペル チェックを実行します。 `acceptLanguage` パラメーターは省略可能です。

```cs
var result = client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof", acceptLanguage: "en-US", market: "en-US").Result;
Console.WriteLine("Correction for Query# \"bill gatas\"");
```

結果を解析します。

```cs
// SpellCheck Results
if (result?.Body.FlaggedTokens?.Count > 0)
{
    // find the first spellcheck result
    var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();

    if (firstspellCheckResult != null)
    {
        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);

        var suggestions = firstspellCheckResult.Suggestions;
        if (suggestions?.Count > 0)
        {
            var firstSuggestion = suggestions.FirstOrDefault();
            Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
            Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
        }
        }
        else
        {
            Console.WriteLine("Couldn't get any Spell check results!");
        }
    }
    else
    {
        Console.WriteLine("Didn't see any SpellCheck results..");
    }
```

## <a name="complete-console-application"></a>コンソール アプリケーションの全体像

次のコンソール アプリケーションは、上記のコードを実行します。

```cs
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Language.SpellCheck;

namespace SpellCheckSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new SpellCheckClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));

            try
            {
                var result = client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof", acceptLanguage: "en-US", market:"en-US").Result;
                Console.WriteLine("Correction for Query# \"bill gatas\"");

                // SpellCheck Results
                if (result?.Body.FlaggedTokens?.Count > 0)
                {
                    // find the first spellcheck result
                    var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();

                    if (firstspellCheckResult != null)
                    {
                        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
                        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
                        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
                        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);

                        var suggestions = firstspellCheckResult.Suggestions;
                        if (suggestions?.Count > 0)
                        {
                            var firstSuggestion = suggestions.FirstOrDefault();
                            Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
                            Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
                        }
                    }
                    else
                    {
                        Console.WriteLine("Couldn't get any Spell check results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any SpellCheck results..");
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            SpellCheckError("YOUR-ACCESS-KEY");

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

        // This will trigger an error response from the API.
        public static void SpellCheckError(string subscriptionKey)
        {
            var client = new SpellCheckClient(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                var result = client.SpellCheckerAsync(text: "", mode: "proof").Result;
            }
            catch (Exception ex)
            {
                Console.WriteLine("Correction for Query# \"empty text field\"");

                if (ex.GetBaseException().GetType() == typeof(Exception) )
                {
                    Console.WriteLine("Encountered exception. " + ex.Message);
                }
            }
        }
    }
}

```

## <a name="next-steps"></a>次の手順

[Cognitive Services の .NET 向け SDK のサンプル](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
