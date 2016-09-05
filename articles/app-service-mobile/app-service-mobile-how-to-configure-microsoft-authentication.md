<properties
	pageTitle="App Services アプリケーションに Microsoft アカウント認証を構成する方法"
	description="App Services アプリケーションに Microsoft アカウント認証を構成する方法について説明します。"
	authors="mattchenderson"
	services="app-service"
	documentationCenter=""
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="mahender"/>

# Microsoft アカウント ログインを使用するように App Service アプリケーションを構成する方法

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

このトピックでは、認証プロバイダーとして Microsoft アカウントを使用するように Azure App Services を構成する方法を示します。

## <a name="register-microsoft-account"> </a>Microsoft アカウントにアプリを登録する

1. [Azure ポータル]にログオンし、アプリケーションに移動します。**URL** をコピーします。この URL は、後でアプリに Microsoft アカウント アプリを構成するために使用します。

2. Microsoft アカウント デベロッパー センターの [マイ アプリケーション] ページに移動し、必要に応じて、Microsoft アカウントでログオンします。

3. **[アプリの追加]** をクリックし、アプリケーション名を入力して、**[アプリケーションの作成]** をクリックします。

4. **アプリケーション ID** をメモします。この情報は後で必要になります。

5. [プラットフォーム] で **[プラットフォームの追加]** をクリックし、[Web] を選択します。

6. [リダイレクト URI] で、アプリケーションのエンドポイントを指定し、**[保存]** をクリックします。
 
	>[AZURE.NOTE]リダイレクト URI は、アプリケーションの URL にパス _/.auth/login/microsoftaccount/callback_ を追加したものです。たとえば、「`https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`」のように入力します。HTTPS スキームを使用していることを確認します。

7. [アプリケーション シークレット] で **[新しいパスワードを生成]** をクリックします。表示される値をメモします。ページから移動すると、このパスワードが再度表示されることはありません。


    > [AZURE.IMPORTANT] パスワードは重要なセキュリティ資格情報です。このパスワードを他のユーザーと共有したり、クライアント アプリケーション内で配信したりしないでください。

## <a name="secrets"> </a>Microsoft アカウントの情報を App Service アプリケーションに追加する

1. [Azure ポータル] に戻り、アプリケーションに移動して、**[設定]**、**[認証/承認]** の順にクリックします。

2. [認証/承認] 機能が有効になっていない場合は、スイッチを **[オン]** に切り替えます。

3. **[Microsoft アカウント]** をクリックします。前の手順で取得したアプリケーション ID とパスワードの値を貼り付けます。アプリケーションで必要なスコープを有効にします (省略可能)。次に、 **[OK]** をクリックします

    ![][1]

	App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。アプリケーション コードでユーザーを承認する必要があります。

4. (省略可能) Microsoft によって認証されたユーザーしかサイトにアクセスできないように制限するには、**[要求が認証されていないときに実行するアクション]** を **[Microsoft アカウント]** に設定します。この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Microsoft アカウントにリダイレクトされます。

5. **[保存]** をクリックします。

これで、アプリケーションで認証に Microsoft アカウントを使用する準備ができました。

## <a name="related-content"></a>関連コンテンツ

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[マイ アプリケーション]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure ポータル]: https://portal.azure.com/

<!---HONumber=AcomDC_0824_2016-->