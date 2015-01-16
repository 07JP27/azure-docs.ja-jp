﻿<properties urlDisplayName="How to Make a Phone Call from Twilio in Java" pageTitle="Twilio を使用して通話する方法 (Java) - Azure" metaKeywords="Azure Twilio 通話, Twilio 通話 Web サイト, Azure Twilio Java" description="Azure 上の Java アプリケーションで Twilio を使用して Web ページから通話する方法について説明します。" metaCanonical="" services="" documentationCenter="Java" title="How to Make a Phone Call Using Twilio in a Java Application on Azure" authors="MicrosoftHelp@twilio.com; robmcm" videoId="" scriptId="" solutions="" manager="twilio" editor="mollybos" />

<tags ms.service="multiple" ms.workload="na" ms.tgt_pltfrm="na" ms.devlang="Java" ms.topic="article" ms.date="01/01/1900" ms.author="MicrosoftHelp@twilio.com; robmcm" />

# Azure 上の Java アプリケーションで Twilio を使用して通話する方法 

次の例では、Azure でホストされる Web ページから Twilio を使用して通話する方法を示しています。次のスクリーンショットに示すように、作成されたアプリケーションは通話に関する値の入力をユーザーに求めます。

![Azure Call Form Using Twilio and Java][twilio_java]

このトピックでコードを使用するためには次の操作を行う必要があります。

1. Twilio アカウント認証トークンを取得します。Twilio を利用し始める前に、[http://www.twilio.com/pricing][twilio_pricing] で価格を検討します。[https://www.twilio.com/try-twilio][try_twilio] でサインアップできます。Twilio から提供される API については、[http://www.twilio.com/api][twilio_api] を参照してください。
2. 電話番号を発信元 ID として Twilio で確認します。電話番号を確認する方法については、[https://www.twilio.com/user/account/phone-numbers/verified#][verify_phone] を参照してください。既存の番号を使用する代わりに、Twilio 電話番号を購入することができます。
この例では、有効な電話番号を callform.jsp の **From** の値として使用します (後で説明)。
3. Twilio JAR を入手します。[https://github.com/twilio/twilio-java][twilio_java_github] で、GitHub のソースをダウンロードして独自の JAR を作成するか、ビルド済み JAR (依存関係あり/なし) をダウンロードすることができます。
このトピックでのコードは、ビルド済み TwilioJava-3.3.8-with-dependencies JAR を使用して記述されています。
4. JAR を Java ビルド パスに追加します。
5. この Java アプリケーションの作成に Eclipse を使用している場合は、Eclipse の展開アセンブリ機能を使用して、アプリケーション展開ファイル (WAR) に Twilio JAR をインクルードできます。この Java アプリケーションの作成に Eclipse を使用していない場合、Twilio JAR が Java アプリケーションと同じ Azure ロールにインクルードされており、アプリケーションのクラス パスに追加されていることを確認してください。
6. cacerts キーストアに Equifax Secure Certificate Authority 証明書と MD5 フィンガープリント 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 が格納されていることを確認します (シリアル番号は 35:DE:F4:CF、SHA1 フィンガープリントは D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A)。これは、[https://api.twilio.com][twilio_api_service] サービスの証明機関 (CA) 証明書であり、Twilio API の使用時に呼び出されます。この CA 証明書を JDK の cacert ストアに追加する方法については、「[証明書を Java CA 証明書ストアに追加する方法][add_ca_cert]」を参照してください。

さらに、[Hello World アプリケーションを Azure Plugin for Eclipse with Java (Microsoft Open Technologies 提供) で作成する方法に関する情報][azure_java_eclipse_hello_world]に精通すること、または、Eclipse を使用していない場合は、Azure 上の Java アプリケーションをホストする別の手法に精通することを強くお勧めします。

## 通話用の Web フォームの作成

次のコードは、通話するためのユーザー データを取得する Web フォームの作成方法を示しています。この例では、**TwilioCloud** という名前の新しい動的 Web プロジェクトを作成し、**callform.jsp** を JSP ファイルとして追加しました。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## 通話用のコードの作成
次のコードは、callform.jsp によって表示されるフォームへの入力が完了すると呼び出され、通話メッセージを作成して通話を生成します。この例では、**makecall.jsp** という名前の JSP ファイルを **TwilioCloud** プロジェクトに追加しました (次のコードは、**accountSID** および **authToken** に割り当てられたプレースホルダー値の代わりに Twilio アカウントと認証トークンを使用します)。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

通話を行うだけでなく、makecall.jsp は Twilio エンドポイント、API バージョン、通話状態を表示します。たとえば、次のスクリーンショットのようになります。

![Azure Call Response Using Twilio and Java][twilio_java_response]

## アプリケーションの実行
次に示しているのは、アプリケーションを実行する大まかな手順です。詳細な手順については、[Microsoft Azure Plugin for Eclipse with Java (Microsoft Open Technologies 提供) を使用した Hello World アプリケーションの作成に関するページ][azure_java_eclipse_hello_world]で確認できます。

1. TwilioCloud WAR を Azure の **approot** フォルダーにエクスポートします。 
2. TwilioCloud WAR を解凍するように **startup.cmd** を変更します。
3. アプリケーションをコンピューティング エミュレーター用にコンパイルします。
4. コンピューティング エミュレーターでデプロイを開始します。
5. ブラウザーを開き、**http://localhost:8080/TwilioCloud/callform.jsp** を実行します。
6. フォームで値を入力し、**[Make this call]** クリックして、makecall.jsp で結果を確認します。

Azure にデプロイする準備ができたら、クラウドへのデプロイ用に再コンパイルし、Azure にデプロイして、ブラウザーで http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp を実行します (*your_hosted_name* は実際の値に置き換えてください)。

## 次のステップ
このコードは、Azure 上の Java で Twilio を使用した基本機能を示すために用意しました。運用環境で Azure に展開する前に、エラー処理やその他の機能をさらに追加することができます。次に例を示します。

*  Web フォームを使用する代わりに、Azure ストレージ BLOB または SQL データベースを使用して、電話番号と通話テキストを保存できます。Java で Azure ストレージ BLOB を使用する方法の詳細については、[Java から BLOB ストレージ サービスを使用する方法に関するページ][howto_blob_storage_java]を参照してください。Java での SQL データベースの使用については、[Java での SQL データベースの使用に関するページ][howto_sql_azure_java]を参照してください。
* **RoleEnvironment.getConfigurationSettings** を使用して、Twilio のアカウント ID と認証トークンを makecall.jsp 内にハードコーディングするのではなく、デプロイの構成設定から取得するようにします。**RoleEnvironment** クラスの詳細については、[JSP での Azure サービス ランタイム ライブラリの使用に関するページ][azure_runtime_jsp]および Azure サービス ランタイム パッケージのドキュメント ([http://dl.windowsazure.com/javadoc][azure_javadoc]) を参照してください。
*  makecall.jsp コードで、Twilio から提供される URL ([http://twimlets.com/message][twimlet_message_url]) を **Url** 変数に設定します。この URL で、通話の次の動作を Twilio に指示する TwiML (Twilio マークアップ言語) 応答が返されるようにします。たとえば、返される TwiML 応答に **&lt;Say&gt;** 動詞を含めて、通話受信者に対してテキストが読み上げられるようにできます。Twilio から提供される URL を使用する代わりに、独自のサービスを作成して Twilio の要求への応答を返すことができます。詳細については、「[Java で音声および SMS 機能に Twilio を使用する方法][howto_twilio_voice_sms_java]」を参照してください。TwiML の詳細については、[http://www.twilio.com/docs/api/twiml][twiml] で確認できます。**&lt;Say&gt;** を始めとする Twilio の動詞の詳細については、[http://www.twilio.com/docs/api/twiml/say][twilio_say] で確認できます。
* また、[https://www.twilio.com/docs/security][twilio_docs_security] の Twilio に関するセキュリティ ガイドラインを参照してください。

Twilio の詳細については、[https://www.twilio.com/docs][twilio_docs] を参照してください。

## 関連項目
* [Java で音声および SMS 機能に Twilio を使用する方法][howto_twilio_voice_sms_java]
* [証明書を Java CA 証明書ストアに追加する方法][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: ../java-add-certificate-ca-store
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/ja-jp/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: ../partner-twilio-java-how-to-use-voice-sms
[howto_blob_storage_java]: http://www.windowsazure.com/ja-jp/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/ja-jp/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/ja-jp/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
