<properties 
	pageTitle="Azure API Management のポリシー" 
	description="API Management のポリシーを作成、編集、構成する方法について説明します。" 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/04/2016" 
	ms.author="sdanie"/>


#Azure API Management のポリシー

Azure API Management のポリシーは、発行者がその構成を通じて API の動作を変更できる、システムの強力な機能の 1 つです。ポリシーは、API の要求または応答に対して順に実行される一連のステートメントのコレクションです。代表的なステートメントとしては、XML 形式から JSON 形式への変換や、(開発者からの呼び出しの回数を制限する) 呼び出しレート制限が挙げられます。他にも多数のポリシーが標準で提供されています。

ポリシー ステートメントとその設定の一覧については、「[Azure API Management ポリシー リファレンス][]」を参照してください。

ポリシーは、API コンシューマーとマネージ API の間に配置されたゲートウェイ内で適用されます。ゲートウェイは、すべての要求を受け取り、通常はそれらの要求をそのまま基底の API に転送します。ただし、ポリシーを使用すると、受信要求と送信応答の両方に変更を適用できます。

ポリシーの式は、ポリシーで特に指定されていない限り、任意の API Management ポリシーで属性値またはテキスト値として使用できます。[制御フロー][] ポリシーや[変数の設定][]ポリシーなど、一部のポリシーはポリシーの式に基づいています。詳細については、「[詳細なポリシー][]」と「[ポリシーの式][]」をご覧ください。

## <a name="scopes"> </a>ポリシーの構成方法
ポリシーは、グローバルに構成することも、[成果物][]、[API][]、または[操作][]をスコープとして構成することもできます。ポリシーを構成するには、パブリッシャー ポータルのポリシー エディターに移動します。

![[ポリシー] メニュー][policies-menu]

ポリシー エディターは、[ポリシー スコープ] \(上部)、ポリシーを編集するための [ポリシー定義] \(左側)、およびステートメントの一覧 \(右側) の 3 つのメイン セクションから構成されます。

![ポリシー エディター][policies-editor]

ポリシーの構成を開始するには、ポリシーを適用するスコープを最初に選択する必要があります。次のスクリーンショットでは、**[スターター]** 成果物が選択されています。ポリシー名の横の四角形は、既にポリシーがこのレベルで適用されていることを示します。

![スコープ][policies-scope]

ポリシーが既に適用されているため、定義ビューに構成が表示されます。

![構成][policies-configure]

ポリシーは、最初は読み取り専用として表示されます。定義を編集するには、**[ポリシーの構成]** アクションをクリックします。

![編集][policies-edit]

ポリシー定義は、一連の受信ステートメントと送信ステートメントが記述された単純な XML ドキュメントです。XML は、定義ウィンドウで直接編集できます。ウィンドウの右側には、ステートメントの一覧が表示されます。上のスクリーンショットの**[呼び出しレート制限]** ステートメントを見るとわかるように、現在のスコープに適用できるステートメントが有効になり、強調表示されます。

有効なステートメントをクリックすると、定義ビュー内のカーソル位置に適切な XML が追加されます。

使用できるポリシー ステートメントと設定の一覧については、「[Azure API Management ポリシー リファレンス][]」を参照してください。

たとえば、着信要求を指定された IP アドレスに制限する新しいステートメントを追加するには、`inbound` XML 要素内にカーソルを置き、**[呼び出し元 IP の制限]** ステートメントをクリックします。

![制限ポリシー][policies-restrict]

これで、ステートメントを構成する方法を示す XML スニペットが `inbound` 要素に追加されます。

	<ip-filter action="allow | forbid">
		<address>address</address>
		<address-range from="address" to="address"/>
	</ip-filter>

受信要求を制限して、IP アドレス 1.2.3.4 からの要求のみを受け付けるには、次のように XML を変更します。

	<ip-filter action="allow">
		<address>1.2.3.4</address>
	</ip-filter>

![保存][policies-save]

ポリシーのステートメントを構成したら、**[保存]** をクリックします。変更は、API Management ゲートウェイに即座に反映されます。

##<a name="sections"> </a>ポリシー構成について

ポリシーは、要求および応答に関して順に実行される一連のステートメントから構成されます。構成は、次のコードからわかるように、`inbound`、`backend`、`outbound`、および `on-error` の各セクションに適切に分けられています。

	<policies>
	  <inbound>
	    <!-- statements to be applied to the request go here -->
	  </inbound>
	  <backend>
	    <!-- statements to be applied before the request is forwarded to 
	         the backend service go here -->
	  </backend>
	  <outbound>
	    <!-- statements to be applied to the response go here -->
	  </outbound>
	  <on-error>
	    <!-- statements to be applied if there is an error condition go here -->
	  </on-error>
	</policies> 

要求の処理中にエラーが発生した場合、`inbound`、`backend`、または `outbound` セクションの残りの手順はスキップされ、実行は `on-error` セクションのステートメントにジャンプします。`on-error` セクションにポリシー ステートメントを配置することで、`context.LastError` プロパティを使用してエラーを確認し、`set-body` ポリシーを使用してエラーの検査とカスタマイズを行い、エラーが発生した場合の動作を構成できます。組み込み手順用と、ポリシー ステートメントの処理中に発生する可能性があるエラー用のエラー コードがあります。詳細については、[API Management のポリシーにおけるエラー処理](https://msdn.microsoft.com/library/azure/mt629506.aspx)に関するページを参照してください。

ポリシーは異なるレベル (グローバル、成果物、API、および操作) で指定できるため、構成では、親ポリシーに対するポリシー定義のステートメントを実行する順序を指定できるようになっています。

ポリシー スコープは、次の順序で評価されます。

1. グローバル スコープ
2. 成果物スコープ
3. API スコープ
4. 操作スコープ

スコープに含まれるステートメントは、`base` 要素が存在する場合は、その配置に従って評価されます。

たとえば、グローバル レベルのポリシーと API 向けに構成されたポリシーがある場合、API が使用されるたびに両方のポリシーが適用されます。API Management では、基本要素を介してポリシー ステートメントの組み合わせの順序を指定できます。

	<policies>
    	<inbound>
        	<cross-domain />
        	<base />
        	<find-and-replace from="xyz" to="abc" />
    	</inbound>
	</policies>

上のポリシー定義の例では、`cross-domain` ステートメントが上位のポリシーよりも前に実行され、その後に `find-and-replace` ポリシーが続いています。

ポリシー ステートメントの中で同じポリシーが 2 回出現した場合は、最近評価されたポリシーが適用されます。これを使用して、上位のスコープに定義されているポリシーを上書きできます。現在のスコープに含まれるポリシーをポリシー エディターに表示するには、**[選択したスコープの有効なポリシーを再計算する]** をクリックします。

グローバル ポリシーには親ポリシーがないため、`<base>` 要素を使用しても効果はありません。

## 次のステップ

ポリシーの式に関する次のビデオをご覧ください。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Azure API Management ポリシー リファレンス]: api-management-policy-reference.md
[成果物]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis
[操作]: api-management-howto-add-operations.md

[詳細なポリシー]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[制御フロー]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[変数の設定]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[ポリシーの式]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png

<!---HONumber=AcomDC_0309_2016-->
