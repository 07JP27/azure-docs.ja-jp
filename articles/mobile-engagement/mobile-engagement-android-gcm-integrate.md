<properties
	pageTitle="Azure Mobile Engagement Android SDK の統合"
	description="Android SDK for Azure Mobile Engagement の最新の更新情報と更新手順について"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="dwrede"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="02/29/2016"
	ms.author="piyushjo" />

#GCM を Mobile Engagement に統合する方法

> [AZURE.IMPORTANT] このガイドの手順を実行する前に、「Engagement を Android に統合する方法」のドキュメントの統合手順を実行する必要があります。
>
> このドキュメントは、いつでも実施できるキャンペーンをサポートする Reach モジュールを統合した場合にのみ役立ちます。アプリケーションに Reach キャンペーンを統合するには、Engagement Reach を Android に統合する方法に関するドキュメントを先にお読みください。

##はじめに

GCM を統合すると、アプリケーションをプッシュできます。

SDK にプッシュされた GCM ペイロードのデータ オブジェクトには常に `azme` キーが含まれています。そのため、アプリケーションで別の目的で GCM を使用する場合、そのキーに基づいてプッシュをフィルター処理できます。

> [AZURE.IMPORTANT] Android 2.2 以降を実行中のデバイスで、Google Play がインストールされ、Google バックグラウンド接続が有効になっているデバイスのみを GCM でプッシュできます。ただし、このコードはサポートされていないデバイスで安全に統合できます (インテントのみを使用します)。

##GCM にサインアップして GCM Service を有効にする

まだ実行してない場合は、GCM Service を Google アカウントで有効にする必要があります。

このドキュメントの作成時点 (2014 年 2 月 5 日) では、[<http://developer.android.com/guide/google/gcm/gs.html>] で次の手順に従うことができます。

自分のアカウントで GCM を有効にするためにのみ、その手順に従ってください。「**API キーの取得**」セクションまで進んだら、Google の次の手順を実行するのではなく、このページに戻ってください。

この手順の説明では、**プロジェクト番号**が **GCM 送信者 ID** として使われています。この情報はこの手順の後半で必要になります。

> [AZURE.IMPORTANT] **プロジェクト番号**と**プロジェクト ID** を混同しないでください。プロジェクト ID は異なるものにすることができるようになりました (それは新しいプロジェクトの名前です)。Engagement SDK に統合する必要があるのは**プロジェクト番号**であり、[Google Developers Console] の [**Overview**] メニューに表示されるものです。

##SDK の統合

### デバイス登録の管理

各デバイスは Google サーバーに登録コマンドを送信する必要があります。そうしないと、デバイスに接続できません。

デバイスは、GCM 通知の登録を解除することもできます (アプリケーションをアンインストールすると、デバイスは自動的に登録解除されます)。

[GCM クライアント ライブラリ]を使用する場合は、android-sdk-gcm-receive を直接読むことができます。

登録インテントをまだ自分で送信していない場合は、Engagement にデバイスの自動登録を実行させることができます。

これを有効にするには、次を `AndroidManifest.xml` ファイルの `<application/>` タグ内に追加します。

			<!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
			<meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### 登録 ID を Engagement Push サービスに伝達し、通知を受信する

デバイスの登録 ID を Engagement Push サービスに伝達し、その通知を受信するためには、以下を `AndroidManifest.xml` ファイルの `<application/>` タグ内に追加します (これはデバイス登録を自分で管理する場合にも実行します)。

			<receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
			  android:exported="false">
			  <intent-filter>
			    <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
			  </intent-filter>
			</receiver>

			<receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
			  <intent-filter>
			    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
			    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
			    <category android:name="<your_package_name>" />
			  </intent-filter>
			</receiver>

次のアクセス権限が `AndroidManifest.xml` 内に設定されていることを確認します (`</application>` タグの後ろ)。

			<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
			<uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
			<permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##サーバー API キーへの Engagement のアクセス権限を与える

まだ実行していない場合は、**サーバー API キー**を [Google Developers Console] で作成します。

サーバー キーには **IP 制限を設定することはできません**。

このドキュメントの作成時点 (2014 年 2 月 5 日) では、手順は次のようになります。

-   これを行うには、[Google Developers Console] を開きます。
-   前の手順と同じプロジェクトを選びます (`AndroidManifest.xml` に統合した**プロジェクト番号**を持つプロジェクト)。
-   [APIs & auth] -> [Credentials] の順に移動し、[Public API access] セクションの [CREATE NEW KEY] をクリックします。
-   [Server key] を選びます。
-   次の画面は空白のままにし **(IP 制限なし)**、[Create] をクリックします。
-   生成された **API キー**をコピーします。
-   $/#application/YOUR\_ENGAGEMENT\_APPID/native-push に移動します。
-   GCM セクションで、生成してコピーした API キーを貼り付けて編集します。

これで、Reach 通知とポーリングを作成するときに [いつでも] を選択することができます。

> [AZURE.IMPORTANT] Engagement では**サーバー キー**が必要です。Android キーは Engagement サーバーでは使えません。

##テスト

「Android での Engagement の統合をテストする方法」を読んで、統合を検証してください。


[<http://developer.android.com/guide/google/gcm/gs.html>]: http://developer.android.com/guide/google/gcm/gs.html
[Google Developers Console]: https://cloud.google.com/console
[GCM クライアント ライブラリ]: http://developer.android.com/guide/google/gcm/gs.html#libs
[Google Developers Console]: https://cloud.google.com/console

<!---HONumber=AcomDC_0302_2016-->