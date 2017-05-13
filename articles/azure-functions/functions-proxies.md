---
title: "Azure Functions でのプロキシの操作 | Microsoft Docs"
description: "Azure Functions Proxies の使用方法の概要"
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 6a0a81a011d9a1cc1a8a81ba8ef92d90308c534d
ms.contentlocale: ja-jp
ms.lasthandoff: 05/03/2017


---
# <a name="working-with-azure-functions-proxies-preview"></a>Azure Functions Proxies (プレビュー) の操作

> [!Note] 
> Azure Functions Proxies は、現在、プレビューの段階です。 プレビューの間は無料ですが、プロキシの実行には標準の Functions の料金が適用されます。 詳細については、[Azure Functions の価格](https://azure.microsoft.com/pricing/details/functions/)に関するページを参照してください。

この記事では、Azure Functions Proxies を構成および操作する方法について説明します。 この機能を使用すると、他のリソースによって実装される、関数アプリのエンドポイントを指定することができます。 これらのプロキシを使用して、大きな API を複数の関数アプリに分割できます (マイクロサービス アーキテクチャでのように)。その場合でも、クライアントには単一の API サーフェスとして表示されます。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <a name="enable"></a>Azure Functions Proxies の有効化

既定では、プロキシは有効になっていません。 機能が無効になっていてもプロキシを作成することはできますが、実行されません。 以下の手順は、プロキシを有効にする方法を示しています。

1. [Azure Portal] を開き、関数アプリに移動します。
2. **[Function App の設定]** を選択します。
3. **[Enable Azure Functions Proxies (preview) (Azure Functions Proxies (プレビュー) を有効にする)]** をオンにします。

新機能が使用可能になったときに、ここに戻って、プロキシ ランタイムを更新することもできます。


## <a name="create"></a>プロキシの作成

このセクションでは、Functions ポータルでプロキシを作成する方法を示します。

1. [Azure Portal] を開き、関数アプリに移動します。
2. 左側のナビゲーションで、**[New proxy (新しいプロキシ)]** を選択します。
3. プロキシの名前を指定します。
4. **ルート テンプレート**と **HTTP メソッド**を指定して、この関数アプリで公開されるエンドポイントを構成します。 これらのパラメーターは、[HTTP トリガー]の規則に従って動作します。
5. **バックエンド URL** を他のエンドポイントに設定します。 これは、別の関数アプリ内の関数にすることも、他の任意の API にすることもできます。 静的な値である必要はありません。[アプリケーション設定]や[元のクライアント要求のパラメーター]を参照することもできます。
6. [作成] をクリックします。

これで、プロキシが関数アプリの新しいエンドポイントになりました。 クライアントの観点からは、Azure Functions の HttpTrigger と同じです。 プロキシの URL をコピーし、任意の HTTP クライアントでテストすることで、新しいプロキシを試してみることができます。








## <a name="modify-requests-responses"></a>要求や応答に変更を加える

バックエンドに対する要求やバックエンドからの応答には、Azure Functions プロキシを使って変更を加えることができます。 この変換時には、「[変数の使用]」で定義している変数を使うことができます。

### <a name="modify-backend-request"></a>バックエンドへの要求に変更を加える

既定では、バックエンドへの要求は、元の要求のコピーとして初期化されます。 バックエンドの URL を設定することに加え、HTTP メソッドやヘッダー、クエリ文字列のパラメーターに変更を加えることもできます。 変更後の値から、[アプリケーション設定]や[元のクライアント要求のパラメーター]を参照することが可能です。

現時点では、ポータルからバックエンドへの要求に変更を加えることはできません。 この機能を proxies.json から利用する方法については、「[requestOverrides オブジェクトの定義]」をご覧ください。

### <a name="modify-response"></a>応答に変更を加える

既定では、クライアントへの応答は、バックエンドからの応答のコピーとして初期化されます。 応答の状態コード、理由の文字列、ヘッダー、本文には、変更を加えることができます。 変更後の値から、[アプリケーション設定]や[元のクライアント要求のパラメーター]、[バックエンドからの応答のパラメーター]を参照することが可能です。

現時点では、ポータルから応答に変更を加えることはできません。 この機能を proxies.json から利用する方法については、「[responseOverrides オブジェクトの定義]」をご覧ください。






## <a name="using-variables"></a>変数の使用

プロキシの構成は必ずしも静的である必要はありません。 条件に応じて元の要求やバックエンドからの応答、アプリケーション設定から得られる変数を使うことができます。

### <a name="request-parameters"></a>要求のパラメーターを参照する

要求のパラメーターは、バックエンド URL プロパティの入力として、または要求や応答に加える変更の一部として使うことができます。 パラメーターには、ベース プロキシの構成に指定されているルート テンプレートに由来するものものあれば、受信要求のプロパティに由来するものもあります。

#### <a name="route-template-parameters"></a>ルート テンプレートのパラメーター
ルート テンプレートに使われているパラメーターは、名前とそれを囲む中かっこ ({}) で参照することができます。

たとえば、プロキシに `/pets/{petId}` のようなルート テンプレートがある場合、バックエンド URL には `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}` のようにして、`{petId}` の値を含めることができます。 ルート テンプレートが `/api/{*restOfPath}` のようにワイルドカードで終了している場合、値 `{restOfPath}` は受信要求の残りのパス セグメントの文字列表現になります。

#### <a name="additonal-request-parameters"></a>要求のパラメーター (その他)
構成の値には、ルート テンプレートのパラメーターに加え、次の値を使うことができます。

* **{request.method}**: 元の要求に使われていた HTTP メソッド。
* **{request.headers.&lt;HeaderName&gt;}**: 元の要求から読み取り可能なヘッダー。 "&lt;HeaderName&gt;" は、読み取るヘッダーの名前に置き換えます。 該当するヘッダーが要求に含まれていない場合、この値は空の文字列になります。
* **{request.querystring.&lt;ParameterName&gt;}**: 元の要求から読み取り可能なクエリ文字列パラメーター。 "&lt;ParameterName&gt;" は、読み取るパラメーターの名前に置き換えます。 該当するパラメーターが要求に含まれていない場合、この値は空の文字列になります。

### <a name="response-parameters"></a>バックエンドからの応答のパラメーターを参照する

応答のパラメーターは、クライアントへの応答に加える変更の一部として使うことができます。 構成の値には、次の値を使うことができます。

* **{backend.response.statusCode}**: バックエンドからの応答で返される HTTP 状態コード。
* **{backend.response.statusReason}**: バックエンドからの応答で返される HTTP 理由文字列。
* **{backend.response.headers.&lt;HeaderName&gt;}**: バックエンドの応答から読み取り可能なヘッダー。 "&lt;HeaderName&gt;" は、読み取るヘッダーの名前に置き換えます。 該当するヘッダーが要求に含まれていない場合、この値は空の文字列になります。

### <a name="use-appsettings"></a>アプリケーション設定を参照する

[Function App に対して定義されているアプリケーション設定](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop)を参照することもできます。その場合は、設定名をパーセント記号 "%" で囲みます。

たとえば、`https://%ORDER_PROCESSING_HOST%/api/orders` のバックエンド URL で、"%ORDER_PROCESSING_HOST%" は ORDER_PROCESSING_HOST 設定の値に置き換えられます。

> [!TIP] 
> 複数のデプロイまたはテスト環境がある場合は、バックエンド ホストのアプリケーション設定を使用してください。 そうすることで、常にその環境の適切なバックエンドと通信することができます。





## <a name="advanced-configuration"></a>詳細な構成

構成するプロキシは、関数アプリ ディレクトリのルートにある proxies.json ファイルに格納されます。 このファイルを手動で編集し、Functions でサポートされているいずれかの[デプロイ方法](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment)を使用するときにアプリの一部としてデプロイできます。 ファイルが処理されるようにするには、この機能を[有効](#enable)にしておく必要があります。 

> [!TIP] 
> いずれのデプロイ方法もまだ設定していない場合は、ポータルから proxies.json ファイルを編集することもできます。 目的の Function App に移動して [プラットフォーム機能] を選択し、[App Service エディター] を選択します。 Function App のファイル構造全体が表示され、変更を加えることができます。

Proxies.json は、名前付きプロキシとその定義から成るプロキシ オブジェクトによって定義されます。 エディターがサポートしていれば、必要に応じて [JSON スキーマ](http://json.schemastore.org/proxies)を参照してコード補完を行うことができます。 サンプル ファイルは、次のようになります。

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

各プロキシには、上の例の "proxy1" のようなフレンドリ名があります。 対応するプロキシ定義オブジェクトは、以下のプロパティによって定義されます。

* **matchCondition**: 必須 - このプロキシの実行をトリガーする要求を定義するオブジェクト。 [HTTP トリガー]と共有される、次の 2 つのプロパティが含まれています。
    * _methods_: プロキシが応答する HTTP メソッドの配列です。 指定しない場合、プロキシはルートのすべての HTTP メソッドに応答します。
    * _route_: 必須 - プロキシがどの要求 URL に応答するかを制御するルート テンプレートを定義します。 HTTP トリガーとは異なり、既定値はありません。
* **backendUri**: 要求のプロキシ先となる、バックエンド リソースの URL。 この値から、アプリケーション設定や元のクライアント要求のパラメーターを参照することが可能です。 このプロパティが含まれていない場合、Azure Functions は HTTP 200 OK で応答します。
* **requestOverrides**: バックエンドへの要求に対する変換を定義するオブジェクト。 「[requestOverrides オブジェクトの定義]」をご覧ください。
* **responseOverrides**: クライアントへの応答に対する変換を定義するオブジェクト。 「[responseOverrides オブジェクトの定義]」をご覧ください。

> [!Note] 
> ルート プロパティ Azure Functions Proxies では、Functions ホスト構成の routePrefix プロパティは考慮されません。 /api のようなプレフィックスを含めたい場合は、ルート プロパティに含める必要があります。

### <a name="requestOverrides"></a>requestOverrides オブジェクトの定義

バックエンド リソースが呼び出されたときに要求に対して行う変更は、requestOverrides オブジェクトで定義します。 このオブジェクトは、次のプロパティによって定義されます。

* **backend.request.method**: バックエンドを呼び出す際に使用される HTTP メソッドです。
* **backend.request.querystring.&lt;ParameterName&gt;**: バックエンドの呼び出し時に設定できるクエリ文字列パラメーター。 "&lt;ParameterName&gt;" は、設定するパラメーターの名前に置き換えます。 空の文字列を指定した場合、このパラメーターはバックエンドへの要求に反映されません。
* **backend.request.headers.&lt;HeaderName&gt;**: バックエンドの呼び出し時に設定可能なヘッダー。 "&lt;HeaderName&gt;" は、設定するヘッダーの名前に置き換えます。 空の文字列を指定した場合、このヘッダーはバックエンドへの要求に反映されません。

アプリケーション設定や元のクライアント要求のパラメーターを値から参照することができます。

実際の構成の例を次に示します。

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept" : "application/xml",
                "backend.request.headers.x-functions-key" : "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>responseOverrides オブジェクトの定義

クライアントに返される応答に対して行う変更は、requestOverrides オブジェクトで定義します。 このオブジェクトは、次のプロパティによって定義されます。

* **response.statusCode**: クライアントに返される HTTP 状態コード。
* **response.statusReason**: クライアントに返される HTTP 理由文字列。
* **response.body**: クライアントに返される本体の文字列表記。
* **response.headers.&lt;HeaderName&gt;**: クライアントへの応答時に設定可能なヘッダー。 "&lt;HeaderName&gt;" は、設定するヘッダーの名前に置き換えます。 空の文字列を指定した場合、このヘッダーは応答に反映されません。

アプリケーション設定や元のクライアント要求のパラメーター、バックエンドからの応答のパラメーターを値から参照することができます。

実際の構成の例を次に示します。

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type" : "text/plain"
            }
        }
    }
}
```
> [!Note] 
> この例では本文を直接設定しているため、`backendUri` プロパティは必要ありません。 このように Azure Functions プロキシを使って API のモッキングを行うことができます。

[Azure Portal]: https://portal.azure.com
[HTTP トリガー]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modifying the backend request]: #modify-backend-request
[Modifying the response]: #modify-response
[requestOverrides オブジェクトの定義]: #requestOverrides
[responseOverrides オブジェクトの定義]: #responseOverrides
[アプリケーション設定]: #use-appsettings
[変数の使用]: #using-variables
[元のクライアント要求のパラメーター]: #request-parameters
[バックエンドからの応答のパラメーター]: #response-parameters
