<properties
   pageTitle="Azure AD ユーザー アカウントを構成する | Microsoft Azure"
   description="この記事では、ARM および ASM に対して認証される、Azure Automation の Runbook のために Azure AD ユーザー アカウントを構成する方法について説明します。"
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Azure Active Directory ユーザー, Azure サービス管理, Azure AD ユーザー アカウント" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte" />

# Azure サービス管理と Azure リソース マネージャーによる Runbook の認証

この記事では、Azure サービス管理 (ASM) および Azure リソース マネージャー (ARM) のリソースに対して実行する Azure Automation Runbook のために Azure AD ユーザー アカウントを構成するのに必要な手順を説明します。このアカウントは ARM ベースの Runbook の認証 ID として引き続きサポートされますが、新しい Azure 実行アカウントを使用する方法を推奨します。

## 新しい Azure Active Directory ユーザーを作成する

1. 管理する Azure サブスクリプションのサービス管理者として Azure クラシック ポータルにログインします。
2. **[Active Directory]** を選択し、組織のディレクトリの名前を選択します。
3. **[ユーザー]** タブをクリックし、コマンド領域の **[ユーザーの追加]** を選択します。
4. **[このユーザーに関する情報の入力]** ページの **[ユーザーの種類]** から **[組織内の新しいユーザー]** を選択します。
5. ユーザー名を入力します。  
6. Active Directory ページの Azure サブスクリプションに関連付けられているディレクトリ名を選択します。
7. **[ユーザー プロファイル]** ページで、姓と名、わかりやすい名前を入力し、**[ロール]** ボックスの一覧からユーザーを選択します。**Multi-Factor Authentication を有効**にしないでください。
8. ユーザーの完全な名前と一時的なパスワードを書き留めておきます。
9. **[設定]、[管理者]、[追加]** の順に選択します。
10. 作成したユーザーの完全なユーザー名を入力します。
11. このユーザーで管理するサブスクリプションを選択してください。
12. ログアウトして、先ほど作成したアカウントで再度 Azure にログインする。ユーザーのパスワードを変更するように促されます。


## Azure クラシック ポータルで Automation アカウントを作成する
このセクションでは、以下の手順を実行して、ASM および ARM モードにてリソースを管理する Runbook で使用される新しい Azure Automation アカウントを Azure ポータルで作成します。

>[AZURE.NOTE] Azure クラシック ポータルで作成した Automation アカウントは、Azure クラシック ポータルおよび Azure ポータルの両方で、どちらのコマンドレット セットでも管理できます。アカウントを作成してしまえば、そのアカウントのリソースの作り方や管理方法に違いはありません。引き続き Azure クラシック ポータルを使用する場合、Automation アカウントの作成には Azure ポータルではなく、Azure クラシック ポータルを使用することをお勧めします。


1. 管理する Azure サブスクリプションのサービス管理者として Azure クラシック ポータルにログインします。
2. **[Automation]** を選択します。
3. **[Automation]** ページで、**[Automation アカウントの作成]** を選択します。
4. **[Automation アカウントの作成]** ボックスで、新しい Automation アカウントの名前を入力し、ドロップダウン リストから **[リージョン]** を選択します。  
5. **[OK]** をクリックして設定を受け入れて、アカウントを作成します。
6. アカウントが作成され、**[Automation]** ページに表示されます。
7. アカウントをクリックします。ダッシュボード ページが表示されます。  
8. Automation のダッシュボード ページで、**[資産]** を選択します。
9. **[資産]** ページで、ページの下部にある **[設定の追加]** を選択します。
10. **[設定の追加]** ページで、**[資格情報の追加]** を選択します。
11. **[資格情報の定義]** ページで、**[資格情報の種類]** ボックスの一覧の **[Windows PowerShell 資格情報]** を選択し、資格情報の名前を指定します。
12. 次の **[資格情報の定義]** ページで、前に作成した AD ユーザー アカウントのユーザー名を **[ユーザー名]** フィールドに入力し、**[パスワード]** および **[パスワードの確認]** フィールドにパスワードを入力します。**[OK]** をクリックして変更を保存します。

## Azure ポータルで Automation アカウントを作成する

このセクションでは、以下の手順を実行して、ARM モードでリソースを管理する Runbook で使用される新しい Azure Automation アカウントを Azure ポータルで作成します。

1. 管理する Azure サブスクリプションのサービス管理者として Azure ポータルにログインします。
2. **[Automation アカウント]** を選択します。
3. [Automation アカウント] ブレードで **[追加]** をクリックします。<br>![Add Automation Account](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
2. **[Automation アカウントの追加]** ブレードの **[名前]** ボックスに、新しい Automation アカウントの名前を入力します。
5. 複数のサブスクリプションがある場合は、新しいアカウントに対して 1 つ指定し、新規または既存の**リソース グループ**と、Azure データ センターの**場所**を指定します。
3. **[Azure 実行アカウントの作成]** オプションで **[いいえ]** を選択し、**[作成]** ボタンをクリックします。  

    >[AZURE.NOTE] 実行アカウントを作成しなかった場合 (先ほどのオプションで **[いいえ]** を選択した場合)、**[Automation アカウントの追加]** ブレードに警告メッセージが表示されます。アカウントが作成されてサブスクリプションの**共同作成者**ロールに割り当てられますが、サブスクリプションのディレクトリ サービスには対応する認証 ID が割り当てられず、サブスクリプション内のリソースにアクセスすることはできません。このアカウントを参照する Runbook は認証を通過できず、ARM リソースに対するタスクを実行することができません。

    ![Add Automation Account Warning](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

4. Azure によって Automation アカウントが作成されている間、メニューの **[通知]** で進行状況を追跡できます。

資格情報の作成が完了したら、資格情報資産を作成し、作成済みの AD ユーザー アカウントに Automation アカウントを関連付ける必要があります。Automation アカウントは作成しただけで、認証 ID に関連付けられていません。「[Azure Automation での資格情報資産](../automation/automation-credentials.md#creating-a-new-credential)」に説明されている手順を実行し、**ユーザー名**の値を**ドメイン\\ユーザー**の形式で入力します。

## Runbook での資格情報の使用

[Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) アクティビティを使用して Runbook の資格情報を取得した後、それを [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) で使用して Azure サブスクリプションに接続できます。資格情報が複数の Azure サブスクリプションの管理者のものである場合は、[Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) も使用して正しいものを指定する必要があります。これについては、ほとんどの Azure Automation Runbook の先頭に一般に出現する以下のサンプル Windows PowerShell で示されています。

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
	Add-AzureAccount –Credential $cred
	Select-AzureSubscription –SubscriptionName "My Subscription"

Runbook のすべての[チェックポイント](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints)の後で、これらの行を繰り返す必要があります。Runbook が中断された後、別のワーカーで再開した場合は、もう一度認証を実行する必要があります。

## 次のステップ
* Runbook のさまざまな種類について、また、独自の Runbook を作成する手順について確認します。記事「[Azure Automation の Runbook の種類](../automation/automation-runbook-types.md)」を参照してください。

<!---HONumber=AcomDC_0511_2016-->