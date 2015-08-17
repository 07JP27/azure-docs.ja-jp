<properties 
	pageTitle="Azure API Management の OAuth 2.0 を使用して開発者アカウントを認証する方法" 
	description="API Management で OAuth 2.0 を使用してユーザーを承認する方法について説明します。" 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/05/2015" 
	ms.author="sdanie"/>

# Azure API Management の OAuth 2.0 を使用して開発者アカウントを認証する方法

多くの API では、[OAuth 2.0](http://oauth.net/2/) がサポートされています。OAuth 2.0 を使用すると、API をセキュリティで保護して、有効なユーザーのみにアクセスが許可されること、および有効なユーザーが許可されたリソースのみにアクセスできることを保証できます。Azure API Management では、対話型の開発者コンソールでそのような API を使用できるようにするために、OAuth 2.0 に対応する API を使用するサービス インスタンスを構成できます。

## <a name="prerequisites"> </a>前提条件

このガイドでは、開発者アカウントに OAuth 2.0 認証を使用するように API Management サービス インスタンスを構成する方法を説明していますが、OAuth 2.0 プロバイダーを構成する方法は説明していません。手順は似ていて、API Management サービス インスタンスでの OAuth 2.0 の構成に使用される情報は同じですが、各 OAuth 2.0 プロバイダーの構成は異なっています。このトピックは、Azure Active Directory を OAuth 2.0 プロバイダーとして使用する例を示しています。

>[AZURE.NOTE]Azure Active Directory を使用して OAuth 2.0 を構成する方法について詳しくは、[WebApp-GraphAPI-DotNet][] のサンプルを参照してください。

## <a name="step1"> </a>API Management で OAuth 2.0 認証サーバーを構成する

最初に、ご利用の API Management サービスの Azure ポータルで **[管理]** をクリックします。API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>[AZURE.NOTE]まだ API Management サービス インスタンスを作成していない場合は、「[Azure API Management の使用][]」チュートリアルの「[API Management インスタンスの作成][]」を参照してください。

左側の **[API Management]** メニューで **[セキュリティ]** をクリックし、**[OAuth 2.0]** をクリックしてから、**[認証サーバーの追加 (Add authorization server)]** をクリックします。

![OAuth 2.0][api-management-oauth2]

**[認証サーバーの追加 (Add authorization server)]** をクリックした後に、新しい認証サーバーのフォームが表示されます。

![新しいサーバー][api-management-oauth2-server-1]

**[名前]** フィールドと **[説明]** フィールドに、名前とオプションの説明を入力します。

>[AZURE.NOTE]これらのフィールドは、OAuth 2.0 認証サーバーを現在の API Management サービス インスタンス内で識別するために使用されるもので、それらの値が OAuth 2.0 サーバーによって自動入力されることはありません。

**[クライアント登録ページ URL (Client registration page URL)]** を入力します。このページでは、ユーザーがアカウントを作成して管理できます。このページは使用される OAuth 2.0 プロバイダーによって異なります。**[クライアント登録ページ URL]** では、ユーザーによるアカウント管理をサポートする OAuth 2.0 プロバイダーについて、ユーザーが自身のアカウントを作成および構成するために使用できるページを指定します。この機能を OAuth 2.0 プロバイダーがサポートしている場合でも、組織によってはこの機能を構成または使用していない場合があります。OAuth 2.0 プロバイダーでユーザーによるアカウント管理が構成されていない場合は、会社の URL、`https://placeholder.contoso.com` のような URL などのプレースホルダー URL を入力してください。

フォームの次のセクションには、**[認証コード付与タイプ (Authorization code grant types)]**、**[認証エンドポイント URL (Authorization endpoint URL)]**、および **[認証要求方式 (Authorization request method)]** 設定が含まれます。

![新しいサーバー][api-management-oauth2-server-2]

必要なタイプにチェックマークを入れて、**[認証コード付与タイプ (Authorization code grant types)]** を指定します。**[認証コード]** が既定で指定されています。

**[認証エンドポイント URL (Authorization endpoint URL)]** を入力します。Azure Active Directory では、この URL は以下の URL のようになります。ここで、`<client_id>` は、使用するアプリケーションを OAuth 2.0 サーバーが識別するためのクライアント ID に置き換えてください。

    https://login.windows.net/<client_id>/oauth2/authorize

**[認証要求方式 (Authorization request method)]** は、認証要求が OAuth 2.0 サーバーに送信される方法を指定します。既定では **[GET]** が選択されています。

次のセクションでは、**[トークン エンドポイント URL (Token endpoint URL)]**、**[クライアント認証方式(Client authentication methods)]**、**[アクセス トークン送信方式 (Access token sending method)]**、および **[既定のスコープ (Default scope)]** を指定します。

![新しいサーバー][api-management-oauth2-server-3]

Azure Active Directory OAuth 2.0 サーバーでは、**[トークン エンドポイント URL]** の形式が以下のようになります。ここで、`<APPID>` の形式は `yourapp.onmicrosoft.com` です。

    https://login.windows.net/<APPID>/oauth2/token

既定の設定は、**[クライアント認証方式(Client authentication methods)]** が **[基本 (Basic)]**、**[アクセス トークン送信方式 (Access token sending method)]** が **[認証ヘッダー (Authorization header)]** です。これらの値は、**[既定のスコープ (Default scope)]** と共に、フォームのこのセクションで構成されます。

**[クライアントの資格情報]** セクションには **[クライアント ID]** と **[クライアント シークレット]** が含まれます。これらは OAuth 2.0 サーバーの作成と構成のプロセスで取得されます。**[クライアント ID]** と **[クライアント シークレット]** が指定された後に、**[認証コード]** の **redirect\_uri** が生成されます。この URI は、OAuth 2.0 サーバー構成で応答 URL を構成するために使用されます。

![新しいサーバー][api-management-oauth2-server-4]

**[認証コード付与タイプ (Authorization code grant types)]** が **[リソース所有者パスワード (Resource owner password)]** に設定された場合、**[リソース所有者パスワードの資格情報 (Resource owner password credentials)]** セクションがそれらの資格情報の指定に使用されます。そうでない場合は、そのセクションを空白のままにすることができます。

![新しいサーバー][api-management-oauth2-server-5]

このフォームが完了したら、**[保存]** をクリックして API Management OAuth 2.0 認証サーバーの構成を保存します。サーバーの構成が保存された後、次のセクションで説明されているように、この構成を使用するように API を構成できます。

## <a name="step2"> </a>OAuth 2.0 ユーザー認証を使用するように API を構成する

左側の **[API Management]** メニューで **[API]** をクリックし、必要な API の名前をクリックし、**[セキュリティ]** をクリックしてから、**[OAuth 2.0]** のボックスにチェックマークを入れます。

![ユーザー認証][api-management-user-authorization]

必要な **[認証サーバー (Authorization server)]** をドロップダウン リストで選択して、**[保存]** をクリックします。

![ユーザー認証][api-management-user-authorization-save]

## <a name="step3"> </a>開発者ポータルで OAuth 2.0 ユーザー認証をテストする

OAuth 2.0 認証サーバーを構成して、そのサーバーを使用するように API を構成した後、開発者ポータルに移動して API を呼び出すことにより、そのサーバーをテストできます。右上のメニューで、**[開発者ポータル]** をクリックします。

![開発者ポータル][api-management-developer-portal-menu]

上部のメニューで **[API]** をクリックし、**[Echo API]** を選択します。

![Echo API][api-management-apis-echo-api]

>[AZURE.NOTE]アカウントに対して構成されている (またはアカウントから見える) API が 1 つしかない場合、[API] をクリックすると、その API の操作に直接誘導されます。

**[GET Resource]** 操作を選択し、**[コンソールを開く]** をクリックして、ドロップダウンで **[認証コード]** を選択します。

![Open console][api-management-open-console]

**[認証コード]** が選択されると、OAuth 2.0 プロバイダーのサインイン フォームがあるポップアップ ウィンドウが表示されます。この例では、サインイン フォームは Azure Active Directory によって提供されています。

>[AZURE.NOTE]ポップアップが無効になっている場合は、それを有効にするように伝えるプロンプトがブラウザーによって出されます。ポップアップを有効にした後に、再び **[認証コード]** を選択すると、サインイン フォームが表示されます。

![サインイン][api-management-oauth2-signin]

サインインした後、**[要求ヘッダー]** には、要求を認証するための `Authorization : Bearer` ヘッダーが取り込まれます。

![要求ヘッダー トークン][api-management-request-header-token]

これで、残りのパラメーター用に必要な値を構成して、要求を送信できます。

## 次のステップ

OAuth 2.0 と API Management の詳細については、次のビデオとこの[記事](api-management-howto-protect-backend-with-aad.md)をご覧ください。

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[API Management インスタンスの作成]: api-management-get-started.md
[Get started with advanced API configuration]: api-management-get-started-advanced.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Azure API Management の使用]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

<!---HONumber=August15_HO6-->