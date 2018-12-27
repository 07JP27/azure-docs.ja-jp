---
title: 'クイック スタート: 視覚検索クエリを作成する (Java) - Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Bing Visual Search API にイメージをアップロードし、そのイメージに関する分析情報を取得する方法。
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: c1b63b12a48f5ccfb1a396ffa9282249b03893fe
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445176"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-java"></a>クイック スタート: Java での最初の Bing Visual Search クエリ

Bing Visual Search API は、提供された画像に関する情報を返します。 イメージを提供するには、イメージの URL または分析情報トークンを使用するか、イメージをアップロードします。 これらのオプションの詳細については、「[Bing Visual Search API とは](../overview.md)」を参照してください。 この記事では、イメージのアップロードについて説明します。 よく知られているランドマークの写真を撮影し、それに関する情報を取得するモバイル シナリオで、イメージのアップロードは便利であると考えられます。 たとえば、分析情報にはランドマークに関する雑学が含まれることがあります。 

ローカル イメージをアップロードする場合は、次に示すフォーム データを POST の本文に含める必要があります。 フォーム データには Content-Disposition ヘッダーが含まれる必要があります。 その `name` パラメーターには "image" を設定する必要があり、`filename` パラメーターには任意の文字列を設定できます。 フォームの内容は、イメージのバイナリです。 アップロードできるイメージの最大サイズは、1 MB です。 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

この記事では、Bing Visual Search API 要求を送信し、JSON 検索結果を表示するシンプルなコンソール アプリケーションを紹介します。 このアプリケーションは Java で記述されていますが、API は、HTTP 要求の発行と JSON の解析が可能な任意のプログラミング言語と互換性がある RESTful Web サービスです。 


## <a name="prerequisites"></a>前提条件
このクイック スタートでは、「[Cognitive Services の価格 - Bing Search API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/)」に記載されている S9 価格帯のサブスクリプションを開始する必要があります。 

Azure portal でサブスクリプションを開始するには、次の手順に従います。
1. Azure portal の一番上の "`Search resources, services, and docs`" と表示されているテキスト ボックスに「BingSearchV7」と入力します。  
2. ドロップダウン リストの [Marketplace] から `Bing Search v7` を選択します。
3. 新しいリソースの名前を [`Name`] に入力します。
4. `Pay-As-You-Go` サブスクリプションを選択します。
5. `S9` 価格レベルを選択します。
6. [`Enable`] をクリックしてサブスクリプションを開始します。

このコードをコンパイルして実行するには、[JDK 7 または 8](https://aka.ms/azure-jdks) が必要です。 好みの Java IDE がある場合はそれを使用してもかまいませんが、テキスト エディターで十分です。

## <a name="running-the-application"></a>アプリケーションの実行

Java で MultipartEntityBuilder を使用してイメージをアップロードする方法を次に示します。

このアプリケーションを実行するには、次の手順に従います。

1. [gson ライブラリ](https://github.com/google/gson)をダウンロードまたはインストールします。 また、Maven 経由で入手することもできます。
2. 普段使用している IDE またはエディターで新しい Java プロジェクトを作成します。
3. `VisualSearch.java` という名前のファイルで提供されているコードを追加します。
4. `subscriptionKey` 値を、サブスクリプション キーに置き換えます。
4. `imagePath` 値を、アップロードするイメージのパスに置き換えます。
5. プログラムを実行します。


```java
package uploadimage;

import java.util.*;
import java.io.*;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class UploadImage2 {

    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere";
    static String imagePath = "<pathtoyourimagetouploadgoeshere>";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addBinaryBody("image", new File(imagePath))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    
}
```

## <a name="next-steps"></a>次の手順

[分析情報トークンを使用してイメージに関する分析情報を取得する](../use-insights-token.md)  
[Bing Visual Search 画像アップロードのチュートリアル](../tutorial-visual-search-image-upload.md)
[Bing Visual Search シングルページ アプリのチュートリアル](../tutorial-bing-visual-search-single-page-app.md)  
[Bing Visual Search の概要](../overview.md)  
[試してみる](https://aka.ms/bingvisualsearchtryforfree)  
[無料試用版のアクセス キーを入手する](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API のリファレンス](https://aka.ms/bingvisualsearchreferencedoc)

