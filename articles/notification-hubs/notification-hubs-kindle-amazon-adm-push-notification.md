---
title: "Azure Notification Hubs の使用 (Kindle アプリ) | Microsoft Docs"
description: "このチュートリアルでは、Azure Notification Hubs を使用して Kindle アプリケーションにプッシュ通知を送信する方法について学習します。"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7206f152ed7270abc62536a9ee164f7227833bcc


---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Notification Hubs の使用 (Kindle アプリ)
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>概要
このチュートリアルでは、Azure Notification Hubs を使用して Kindle アプリケーションにプッシュ通知を送信する方法について説明します。
Amazon Device Messaging (ADM) を使用してプッシュ通知を受信する空の Kindle アプリケーションを作成します。

## <a name="prerequisites"></a>前提条件
このチュートリアルには、次のものが必要です。

* <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android サイト</a>から Android SDK を入手します (Eclipse の使用を想定)。
* 「<a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">開発環境を設定する</a>」の手順に従い、Kindle の開発環境を設定します。

## <a name="add-a-new-app-to-the-developer-portal"></a>開発者ポータルへの新しいアプリの追加
1. 最初に、 [Amazon 開発者ポータル]でアプリを作成します。
   
    ![][0]
2. **アプリケーション キー**をコピーします。
   
    ![][1]
3. ポータルで、アプリの名前をクリックし、 **[Device Messaging]** タブをクリックします。
   
    ![][2]
4. **[Create a New Security Profile (新しいセキュリティ プロファイルの作成)]** をクリックし、新しいセキュリティ プロファイル (たとえば、**TestAdm セキュリティ プロファイル**) を作成します。 その後、 **[保存]**をクリックします。
   
    ![][3]
5. **[セキュリティ プロファイル]** をクリックして、作成したセキュリティ プロファイルを表示します。 後で使用するために **[Client ID (クライアント ID)]** と **[Client Secret (クライアント シークレット)]** の値をコピーします。
   
    ![][4]

## <a name="create-an-api-key"></a>API キーの作成
1. 管理者特権でコマンド プロンプトを開きます。
2. Android SDK フォルダーに移動します。
3. 次のコマンドを入力します。
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. **keystore** パスワードに「**android**」と入力します。
5. **MD5** フィンガープリントをコピーします。
6. 開発者ポータルに戻り、**[Messaging (メッセージング)]** タブで **[Android/Kindle]** をクリックし、アプリのパッケージの名前 (たとえば、**com.sample.notificationhubtest**)、**MD5** の値を入力し、**[Generate API Key (API キーの生成)]** をクリックします。

## <a name="add-credentials-to-the-hub"></a>ハブへの資格情報の追加
ポータルで、クライアント シークレットとクライアント ID を通知ハブの **[構成]** タブに追加します。

## <a name="set-up-your-application"></a>アプリケーションの設定
> [!NOTE]
> アプリケーションを作成する場合は、API レベル 17 以上を使用します。
> 
> 

ADM ライブラリを Eclipse プロジェクトに追加します。

1. ADM ライブラリを取得するには、 [SDK をダウンロード]します。 SDK zip ファイルを解凍します。
2. Eclipse で、プロジェクトを右クリックし、 **[プロパティ]**をクリックします。 左側の **[Java Build Path (Java のビルド・パス)]** を選択し、上部の **[Libraries (ライブラリ)]** タブを選択します。 **[Add External Jar (外部 Jar の追加)]** をクリックし、Amazon SDK を解凍したディレクトリから `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` ファイルを選択します。
3. NotificationHubs Android SDK (リンク) をダウンロードします。
4. パッケージを解凍し、`notification-hubs-sdk.jar` ファイルを Eclipse の `libs` フォルダーにドラッグします。

アプリケーション マニフェストを編集して、ADM をサポートするようにします。

1. ルート マニフェスト要素に Amazon 名前空間を追加します。

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. マニフェスト要素の下の最初の要素としてアクセス許可を追加します。 **[YOUR PACKAGE NAME]** を、アプリケーションを作成するときに使用したパッケージに置き換えます。
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. 次の要素をアプリケーション要素の最初の子として挿入します。 忘れずに、**[YOUR SERVICE NAME]** を次のセクションで作成する ADM メッセージ ハンドラーの名前 (パッケージを含む) に置き換え、**[YOUR PACKAGE NAME]** をアプリの作成時に使ったパッケージ名に置き換えてください。
   
        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />
   
        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />
   
            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >
   
            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>ADM メッセージ ハンドラーの作成
1. `com.amazon.device.messaging.ADMMessageHandlerBase` を継承する新しいクラスを作成し、次の図に示すように `MyADMMessageHandler` という名前を付けます。
   
    ![][6]
2. 次の `import` ステートメントを追加します。
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. 作成したクラスに次のコードを追加します。 ハブ名および接続文字列 (listen) を置き換えます。
   
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
          private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }
   
        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }
   
            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }
   
            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();
   
                mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
   
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                  new Intent(ctx, MainActivity.class), 0);
   
            NotificationCompat.Builder mBuilder =
                  new NotificationCompat.Builder(ctx)
                  .setSmallIcon(R.mipmap.ic_launcher)
                  .setContentTitle("Notification Hub Demo")
                  .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                  .setContentText(msg);
   
             mBuilder.setContentIntent(contentIntent);
             mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
4. 次のコードを `OnMessage()` メソッドに追加します。
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. 次のコードを `OnRegistered` メソッドに追加します。
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. 次のコードを `OnUnregistered` メソッドに追加します。
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. `MainActivity` メソッドで、次の import ステートメントを追加します。
   
        import com.amazon.device.messaging.ADM;
8. `OnCreate` メソッドの末尾に次のコードを追加します。
   
        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                         MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>アプリへの API キーの追加
1. Eclipse で、プロジェクトのディレクトリ資産に **api_key.txt** という名前の新しいファイルを作成します。
2. ファイルを開き、Amazon 開発者ポータルで生成した API キーをコピーします。

## <a name="run-the-app"></a>アプリの実行
1. エミュレーターを起動します。
2. エミュレーターで、上部からスワイプして **[設定]**、**[マイ アカウント]** の順にクリックし、有効な Amazon アカウントで登録します。
3. Eclipse で、アプリを実行します。

> [!NOTE]
> 問題が発生した場合は、エミュレーター (またはデバイス) の時間を確認します。 時刻値は正確である必要があります。 Kindle エミュレーターの時刻を変更するには、Android SDK platform-tools ディレクトリで次のコマンドを実行します。
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>メッセージの送信
.NET を使用してメッセージを送信するには:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon 開発者ポータル]: https://developer.amazon.com/home.html
[SDK をダウンロード]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png



<!--HONumber=Nov16_HO2-->


