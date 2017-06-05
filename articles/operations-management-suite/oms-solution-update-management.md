---
title: "OMS の更新管理ソリューション | Microsoft Docs"
description: "この記事の目的は、このソリューションを使用して Windows コンピューターと Linux コンピューターの更新プログラムを管理する方法の理解を助けることです。"
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/23/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: ff8d911750a551f4a099fcba13841c98881104a9
ms.contentlocale: ja-jp
ms.lasthandoff: 05/25/2017


---
# <a name="update-management-solution-in-oms"></a>OMS の更新管理ソリューション
OMS の更新管理ソリューションを使用すると、Windows コンピューターと Linux コンピューターの更新プログラムを管理することができます。  すべてのエージェント コンピューターで利用可能な更新プログラムの状態をすばやく評価し、サーバーに必要な更新プログラムをインストールするプロセスを開始することができます。 

## <a name="solution-components"></a>ソリューションのコンポーネント

OMS で管理されるコンピューターでは、評価と更新プログラムのデプロイに次のコンポーネントを使用します。 

* Linux または Windows 用の OMS エージェント
* PowerShell Desired State Configuration (DSC) (Linux の場合) 
* Automation Hybrid Runbook Worker 
* Microsoft Update または Windows Server Update Services (Windows コンピューターの場合)

下の図は、動作とデータ フローの概念図です。ワークスペースに接続されたすべての Windows Server および Linux コンピューターがこのソリューションによってどのように評価され、更新プログラムが適用されるかを示しています。    

#### <a name="windows-server"></a>Windows Server
![Windows Server の更新管理プロセスのフロー](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Linux の更新管理プロセスのフロー](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

コンピューターが更新プログラムの対応状態を確認するためにスキャンを実行した後、OMS エージェントによって情報が OMS に一括して転送されます。 Windows コンピューターでは、対応スキャンは既定で 12 時間ごとに実行されます。  このスキャン スケジュールに加えて、Microsoft Monitoring Agent (MMA) の再起動後 15 分以内、更新プログラムのインストール前、および更新プログラムのインストール後に、更新プログラムの対応状態を確認するためのスキャンが開始されます。  Linux コンピューターでは、対応スキャンは既定で 3 時間ごとに実行されます。また、MMA エージェントが再起動された場合も 15 分以内に対応スキャンが開始されます。  

その後、対応情報は処理されて、ソリューションに含まれるダッシュボードに要約が表示されます。また、対応情報はユーザー定義や事前定義のクエリを使用して検索することもできます。  このソリューションは、同期先として構成されたソースに基づいて、コンピューターがどの程度最新の状態であるかをレポートします。  Windows コンピューターが WSUS にレポートするよう構成されている場合、WSUS が Microsoft Update と最後に同期したタイミングによっては、更新の結果が Microsoft Updates の示す内容と一致しない場合があります。  Linux コンピューターでも、レポート先の構成がローカル リポジトリとパブリック リポジトリとで更新結果が異なる場合があります。   

更新が必要なコンピューターへのソフトウェア更新プログラムのデプロイとインストールに、スケジュールされたデプロイを使用できます。  Windows コンピューターの場合、デプロイの範囲に含まれるのは、必須の更新プログラムのみで、"*オプション*" に分類されている更新プログラムは含まれません。  スケジュールされたデプロイでは、適用可能な更新プログラムを受け取る対象コンピューターを定義する際に、コンピューターを明示的に指定するか、特定のコンピューター セットのログ検索に基づく[コンピューター グループ](../log-analytics/log-analytics-computer-groups.md)を選択します。  また、スケジュールを指定する際は、更新プログラムのインストールを許可する期間を承認し、指定します。  更新プログラムは、Azure Automation の Runbook によってインストールされます。  これらの Runbook は表示できません。また、これらは構成不要です。  "更新プログラムの展開" を作成すると、対象に含めたコンピューターに対して、指定した時間にマスター更新 Runbook を開始するスケジュールが作成されます。  このマスター Runbook は、必要な更新プログラムのインストールを実行する子 Runbook を各エージェントで開始します。       

更新プログラムの展開で指定した日時に、対象のコンピューターでデプロイが並行して実行されます。  まず、スキャンが実行され、その更新プログラムが必須であることが確認されてからインストールされます。  WSUS クライアント コンピューターの場合、更新プログラムが WSUS で承認されていないと更新プログラムの展開は失敗するので、注意してください。  適用された更新プログラムの結果は OMS に転送され、そこで処理されてダッシュボードに要約が表示されます。また、イベントを検索することもできます。     

## <a name="prerequisites"></a>前提条件
* このソリューションでサポートされるのは、Windows Server 2008 以降に対する更新プログラムの評価と、Windows Server 2008 R2 以降に対する更新プログラムの展開の実行です。  Server Core と Nano Server のインストール オプションはサポートされていません。

    > [!NOTE]
    > Windows Server 2008 R2 に更新プログラムをデプロイするためには、.NET Framework 4.5 および WMF 5.0 以降が必要です。
    >  
* Windows クライアント オペレーティング システムはサポートされていません。  
* Windows エージェントは、Windows Server Update Services (WSUS) サーバーと通信するか Microsoft Update にアクセスできるように構成する必要があります。  
  
    > [!NOTE]
    > Windows エージェントは、System Center Configuration Manager で同時に管理することはできません。  
    >
* CentOS 6 (x86/x64) および 7 (x64)
* Red Hat Enterprise 6 (x86/x64) および 7 (x64)
* SUSE Linux Enterprise Server 11 (x86/x64) および 12 (x64)
* Ubuntu 12.04 LTS 以降 (x86/x64)  
* Linux エージェントは、更新リポジトリへのアクセスが必要です。  

    > [!NOTE]
    > このソリューションでは、OMS Agent for Linux が複数の OMS ワークスペースにレポートする構成はサポートされていません。  
    > 

OMS Agent for Linux をインストールして最新バージョンをダウンロードする方法の詳細については、[Operations Management Suite Agent for Linux](https://github.com/microsoft/oms-agent-for-linux) に関するページを参照してください。  Windows 用 OMS エージェントをインストールする方法の詳細については、[Windows 用 Operations Management Suite エージェント](../log-analytics/log-analytics-windows-agents.md)に関するページを参照してください。  

## <a name="solution-components"></a>ソリューションのコンポーネント
このソリューションは以下のリソースで構成されています。これらのリソースは、Automation アカウントに追加され、エージェントに直接接続されるか、Operations Manager に接続された管理グループに接続されます。 

### <a name="management-packs"></a>管理パック
System Center Operations Manager 管理グループが OMS ワークスペースに接続されている場合は、以下の管理パックが Operations Manager にインストールされます。  これらの管理パックは、このソリューションを追加した後、直接接続された Windows コンピューターにもインストールされます。 管理パックに伴って構成や管理が必要となるものはありません。 

* Microsoft System Center Advisor 更新プログラム評価インテリジェンス パック (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* 更新プログラムの展開 MP

ソリューション管理パックの更新方法の詳細については、「 [Operations Manager を Log Analytics に接続する](../log-analytics/log-analytics-om-agents.md)」を参照してください。

### <a name="hybrid-worker-groups"></a>ハイブリッド worker グループ
このソリューションを有効にすると、ソリューションに含まれている Runbook をサポートするために、OMS ワークスペースに直接接続された Windows コンピューターが自動的に Hybrid Runbook Worker として構成されます。  ソリューションで管理されている各 Windows コンピューターは、<*ホスト名 FQDN_GUID*> という命名規則に従って、Automation アカウントの Hybrid Runbook Worker グループ ブレードに表示されます。  アカウントの Runbook でこれらのグループを対象として指定することはできません。指定すると失敗します。 これらのグループは、管理ソリューションをサポートすることのみを目的としています。   

ただし、このソリューションと Hybrid Runbook Worker グループ メンバーシップの両方に同じアカウントを使用すれば、Windows コンピューターを Automation アカウントの Hybrid Runbook Worker に追加して Automation Runbook をサポートすることができます。  この機能は、Hybrid Runbook Worker のバージョン 7.2.12024.0 に追加されました。  

## <a name="configuration"></a>構成
OMS ワークスペースに更新管理ソリューションを追加し、エージェントがレポートしていることを確認するには、次の手順を実行します。 ワークスペースに既に接続されている Windows エージェントは、そのままの構成で自動的に追加されます。 

このソリューションのデプロイには、次の方法を使用できます。

* Azure Portal の Azure Marketplace でオートメーションと制御サービスまたは更新管理ソリューションを選択する
* OMS ワークスペースで OMS ソリューション ギャラリーから追加する

既に Automation アカウントと OMS ワークスペースが同じリソース グループとリージョンにリンクされている場合は、[オートメーションと制御] を選択すると、構成を確認し、ソリューションをインストールするだけで、両方のサービスでソリューションが構成されます。  Azure Marketplace で更新管理ソリューションを選択した場合も、同じようにして構成されます。  サブスクリプションにどちらかのサービスがデプロイされている場合は、**[新しいソリューションの作成]** ブレードの手順に従って、事前に選択された他の推奨ソリューションをインストールする必要があるかどうかを確認します。  また、必要に応じて、[OMS ソリューションの追加](../log-analytics/log-analytics-add-solutions.md)に関するページで説明されているプロセスをソリューション ギャラリーで実行して、更新管理ソリューションを OMS ワークスペースに追加できます。  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-to-oms"></a>OMS エージェントと Operations Manager 管理グループが OMS に接続されていることを確認する

直接接続された OMS Agent for Linux と Windows 用 OMS エージェントが OMS と通信していることを確認するには、数分経ってから、次のログ検索を実行します。

* Linux - `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`  

* Windows - `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Windows コンピューターでは、次の内容を調べて、OMS とエージェントの接続を確認できます。

1.  コントロール パネルで [Microsoft Monitoring Agent] を開き、**[Azure Log Analytics (OMS)]** タブで、エージェントに "**Microsoft Monitoring Agent は Microsoft Operations Management Suite サービスに正常に接続しました**" というメッセージが表示されていることを確認します。   
2.  Windows イベント ログを開き、**アプリケーションとサービス ログ\Operations Manager** に移動して、ソースのサービス コネクタでイベント ID 3000 および 5002 を検索します。  これらのイベントは、コンピューターが OMS ワークスペースに登録され、構成を受信していることを示しています。  

エージェントが OMS サービスと通信できない場合、ファイアウォールまたはプロキシ サーバーを介してインターネットと通信するよう構成されていれば、「[Log Analytics のプロキシ設定とファイアウォール設定の構成](../log-analytics/log-analytics-proxy-firewall.md)」を参照して、ファイアウォールまたはプロキシ サーバーが正しく構成されていることを確認します。
  
新しく追加された Linux エージェントは、評価が完了した後、状態が "**更新済み**" と表示されます。  このプロセスには最大で 6 時間かかります。 

Operations Manager 管理グループが OMS と通信していることを確認する方法については、「[Operations Manager と OMS の統合を検証する](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms)」を参照してください。

## <a name="data-collection"></a>データ収集
### <a name="supported-agents"></a>サポートされているエージェント
次の表は、このソリューションの接続先としてサポートされているソースとその説明です。

| 接続されているソース | サポートの有無 | 説明 |
| --- | --- | --- |
| Windows エージェント |はい |ソリューションは、Windows エージェントからシステムの更新プログラムに関する情報を収集し、必要な更新プログラムのインストールを開始します。 |
| Linux エージェント |あり |ソリューションは、Linux エージェントからシステムの更新プログラムに関する情報を収集し、サポート対象のディストリビューションに対して必要な更新プログラムのインストールを開始します。 |
| Operations Manager 管理グループ |はい |ソリューションは、接続された管理グループ内のエージェントからシステムの更新プログラムに関する情報を収集します。<br>Operations Manager エージェントから Log Analytics への直接接続は必要ありません。 データは管理グループから OMS リポジトリに転送されます。 |
| Azure ストレージ アカウント |なし |Azure Storage には、システムの更新プログラムに関する情報が含まれていません。 |

### <a name="collection-frequency"></a>収集の頻度
管理対象の各 Windows コンピューターでは、1 日 2 回スキャンが実行されます。 Windows API が 15 分ごとに呼び出され、最後の更新時間のクエリによって状態が変更されたかどうかが確認されます。更新されている場合は対応スキャンが開始されます。  管理対象の各 Linux コンピューターでは、3 時間ごとにスキャンが実行されます。 

ダッシュボードが管理対象コンピューターの更新されたデータを表示するのに、30 分～ 6 時間かかります。   

## <a name="using-the-solution"></a>ソリューションの使用
OMS ワークスペースに更新管理ソリューションを追加すると、OMS のダッシュボードに **[Update Management (更新管理)]** タイルが追加されます。 このタイルには、ご利用の環境におけるコンピューターの数と更新プログラムの対応状態が数字とグラフで表示されます。<br><br>
![更新管理の概要タイル](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>更新プログラムの評価の表示
**[Update Management (更新管理)]** タイルをクリックすると、**[Update Management (更新管理)]** ダッシュボードが表示されます。<br><br> ![更新管理の概要ダッシュボード](./media/oms-solution-update-management/update-management-dashboard.png)<br> 

このダッシュボードには、更新状態の詳細な内訳が表示されます。更新状態は、オペレーティング システムの種類と更新プログラムの分類ごとに分けられています。更新プログラムの分類は、重要な更新プログラム、セキュリティ更新プログラム、その他 (定義更新など) です。 **[Update Deployments (更新プログラムの展開)]** タイルをクリックすると、[Update Deployments (更新プログラムの展開)] ページが表示されます。このページで、スケジュール、現在実行されているデプロイ、完了したデプロイを表示したり、新しいデプロイのスケジュールを設定したりすることができます。  

特定のタイルをクリックすると、すべてのレコードを返すログ検索が実行されます。また、カテゴリと定義済みの条件を指定してクエリを実行し、**[一般的な更新クエリ]** 列に表示された一覧から任意のレコード 1 つを選択することもできます。    

## <a name="installing-updates"></a>更新プログラムのインストール
ご利用のワークスペースにある Linux コンピューターと Windows コンピューターのすべてで更新プログラムが評価されたら、"*更新プログラムの展開*" を作成して、必要な更新プログラムがインストールされるようにすることができます。  "更新プログラムの展開" とは、1 台以上のコンピューターに対して、必要な更新プログラムをスケジュールに従ってインストールすることです。  デプロイの範囲に含めるコンピューターまたはコンピューター グループと、デプロイの日時を指定します。  コンピューター グループの詳細については、[Log Analytics のコンピューター グループ](../log-analytics/log-analytics-computer-groups.md)に関するページを参照してください。  更新プログラムの展開にコンピューター グループを含めると、スケジュールの作成時にグループ メンバーシップが 1 回だけ評価されます。  その後で加えられたグループへの変更は反映されません。  この問題を回避するには、スケジュールされた更新プログラムの展開を削除して、もう一度作成します。 

> [!NOTE]
> Azure Marketplace からデプロイされた Windows VM は、既定で Windows Update Service から自動更新を受信するように設定されています。  このソリューションまたは Windows VM をワークスペースに追加した後も、この動作は変更されません。  このソリューションで更新プログラムを能動的に管理しない場合は、既定の動作 (更新プログラムが自動的に適用される) が適用されます。  

Azure Marketplace から利用できるオンデマンドの Red Hat Enterprise Linux (RHEL) イメージから作成した仮想マシンは、Azure にデプロイされた [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) にアクセスするよう登録されています。  その他の Linux ディストリビューションは、サポートされている方式に従ったディストリビューション オンライン ファイル リポジトリから更新する必要があります。  

### <a name="viewing-update-deployments"></a>更新プログラムの展開の表示
**[Update Deployment (更新プログラムの展開)]** タイルをクリックすると、既存の更新プログラムの展開の一覧が表示されます。  これらは状態別 (**スケジュール**、**実行中**、**Completed (完了)**) にグループ化されています。<br><br> ![更新プログラムの展開スケジュールのページ](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

各更新プログラムの展開に表示されるプロパティについては、次の表で説明します。

| プロパティ | Description |
| --- | --- |
| 名前 |更新プログラムの展開の名前。 |
| スケジュール |スケジュールの種類。  使用できるオプションは、*[1 回限り]*、*[定期的に毎週]*、*[定期的に毎月]* です。 |
| 開始時刻 |更新プログラムの展開の開始予定日時。 |
| 時間 |更新プログラムの展開の実行が許可されている時間 (分)。  この時間内にすべての更新プログラムがインストールされない場合、残りの更新プログラムは次の更新プログラムの展開まで待つ必要があります。 |
| サーバー |更新プログラムの展開の影響を受けるコンピューターの数。  |
| 状態 |更新プログラムの展開の現在の状態。<br><br>次のいずれかの値になります。<br>- 開始されていません<br>- 実行中<br>- 完了しました |

完了した更新プログラムの展開を選択すると、その詳細画面が表示されます。この画面には、次の表にある列が含まれます。  更新プログラムの展開がまだ開始されていない場合は、これらの列に値が設定されません。<br><br> ![更新プログラムの展開結果の概要](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| 分割 | Description |
| --- | --- |
| **コンピューター ビュー** | |
| Windows コンピューター |更新プログラムの展開に含まれる Windows コンピューターの数を状態別に表示します。  状態をクリックするとログ検索が実行され、更新プログラムの展開でその状態の更新レコードがすべて返されます。 |
| Linux コンピューター |更新プログラムの展開に含まれる Linux コンピューターの数を状態別に表示します。  状態をクリックするとログ検索が実行され、更新プログラムの展開でその状態の更新レコードがすべて返されます。 |
| コンピューターのインストール状態 |更新プログラムの展開に含まれるコンピューターと、インストールに成功した更新プログラムの割合を示します。 エントリの 1 つをクリックするとログ検索が実行され、適用していない緊急更新プログラムがすべて返されます。 |
| **更新ビュー** | |
| Windows の更新プログラム |更新プログラムの展開に含まれる Windows の更新プログラムと各更新プログラムのインストール状態が一覧表示されます。  いずれかの更新プログラムを選択すると、その更新プログラムに関するすべての更新レコードを返すログ検索が実行されます。状態をクリックすると、デプロイのすべての更新レコードを返すログ検索が実行されます。 | 
| Linux の更新プログラム |更新プログラムの展開に含まれる Linux の更新プログラムと各更新プログラムのインストール状態が一覧表示されます。  いずれかの更新プログラムを選択すると、その更新プログラムに関するすべての更新レコードを返すログ検索が実行されます。状態をクリックすると、デプロイのすべての更新レコードを返すログ検索が実行されます。 | 

### <a name="creating-an-update-deployment"></a>更新プログラムの展開の作成
新しい更新プログラムの展開を作成するには、画面上部にある **[追加]** ボタンをクリックして **[New Update Deployment (新しい更新プログラムの展開)]** ページを開きます。  次の表にあるプロパティの値を指定する必要があります。

| プロパティ | Description |
| --- | --- |
| 名前 |更新プログラムの展開を識別する一意の名前。 |
| タイム ゾーン |開始時刻に使用するタイム ゾーン。 |
| スケジュールの種類 | スケジュールの種類。  使用できるオプションは、*[1 回限り]*、*[定期的に毎週]*、*[定期的に毎月]* です。  
| 開始時刻 |更新プログラムの展開を開始する日時。 **注:** すぐにデプロイする必要がある場合は、最短で現在の時刻から 30 分後にデプロイを実行できます。 |
| 時間 |更新プログラムの展開の実行が許可されている時間 (分)。  この時間内にすべての更新プログラムがインストールされない場合、残りの更新プログラムは次の更新プログラムの展開まで待つ必要があります。 |
| [Computers (コンピューター)] |更新プログラムの展開に含める対象のコンピューターまたはコンピューター グループの名前。  ドロップダウン リストから 1 つ以上のエントリを選択します。 |

<br><br> ![[New Update Deployment (新しい更新プログラムの展開)] ページ](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>時間範囲
既定では、更新管理ソリューションの分析は、過去 1 日間に生成された接続されているすべての管理グループから収集されたデータが対象となります。 

データの時間範囲を変更するには、ダッシュボードの上部にある **[Data based on (データの時間範囲)]** を選択します。 過去 7 日間、過去 1 日間、または過去 6 時間以内に作成または更新されたレコードを選択できます。 **[Custom]** (カスタム) を選択して、独自の日付範囲を指定することもできます。

## <a name="log-analytics-records"></a>Log Analytics のレコード
更新管理ソリューションにより 2 種類のレコードが OMS リポジトリに作成されます。

### <a name="update-records"></a>Update レコード
**Update** という種類のレコードは、各コンピューターでインストールされるか必要とされる更新プログラムごとに作成されます。 Update レコードには、次の表に示したプロパティがあります。

| プロパティ | 説明 |
| --- | --- |
| 型 |*Update* |
| SourceSystem |更新プログラムのインストールを承認したソース。<br>次のいずれかの値になります。<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Linux Servers (パッケージ マネージャーから取得) |
| Approved |更新プログラムのインストールが承認されているかどうかを指定します。<br> Linux サーバーの場合、修正プログラムの適用は OMS で管理されないため、現在、これは省略可能です。 |
| Classification (Windows の場合) |更新プログラムの分類。<br>次のいずれかの値になります。<br>- アプリケーション<br>- Critical Updates (緊急更新プログラム)<br>- Definition Updates (定義の更新)<br>- Feature Packs (Feature Pack)<br>- Security Updates (セキュリティ更新プログラム)<br>- Service Packs (Service Pack)<br>- Update Rollups (更新プログラムのロールアップ)<br>- 更新プログラム |
| Classification (Linux の場合) |更新プログラムの分類。<br>次のいずれかの値になります。<br>- Critical Updates (緊急更新プログラム)<br>- Security Updates (セキュリティ更新プログラム)<br>- Other Updates (他の更新プログラム) |
| コンピューター |コンピューターの名前。 |
| InstallTimeAvailable |同じ更新プログラムをインストールした他のエージェントからインストール時刻を入手できるかどうかを指定します。 |
| InstallTimePredictionSeconds |同じ更新プログラムをインストールしたその他のエージェントに基づいた、インストールの推定時間 (秒)。 |
| KBID |更新プログラムについて説明するサポート技術情報の ID。 |
| ManagementGroupName |SCOM エージェントの管理グループの名前。  その他のエージェントの場合、これは AOI-<workspace ID> です。 |
| MSRCBulletinID |更新プログラムについて説明するマイクロソフト セキュリティ情報の ID。 |
| MSRCSeverity |マイクロソフト セキュリティ情報の重大度。<br>次のいずれかの値になります。<br>- Critical (重大)<br>- Important (重要)<br>- Moderate (警告) |
| 省略可能。 |更新プログラムが省略可能かどうかを指定します。 |
| 製品 |更新プログラムの対象製品の名前。  **[表示]** をクリックすると、ブラウザーで記事が開きます。 |
| PackageSeverity |Linux ディストリビューションのベンダーによって報告された、この更新プログラムで修正される脆弱性の重大度。 |
| PublishDate |更新プログラムがインストールされた日時。 |
| RebootBehavior |更新プログラムによって再起動を強制するかどうかを指定します。<br>次のいずれかの値になります。<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |更新プログラムのリビジョン番号。 |
| SourceComputerId |コンピューターを一意に識別する GUID。 |
| TimeGenerated |レコードの最終更新日時。 |
| タイトル |更新プログラムのタイトル。 |
| UpdateID |更新プログラムを一意に識別する GUID。 |
| UpdateState |このコンピューターに更新プログラムがインストールされているかどうかを示します。<br>次のいずれかの値になります。<br>- Installed - 更新プログラムはこのコンピューターにインストールされています。<br>- Needed - 更新プログラムはインストールされていないため、このコンピューターに必要です。 |

**Update** という種類のレコードを返すログ検索を実行する際に、**Updates** を選択すると、検索から返された最新情報が一連のタイルに集約されて表示されます。 **[インストールされていない更新プログラムと適用済みの更新プログラム]** タイルと **[必須およびオプションの更新プログラム]** タイルでエントリをクリックすると、ビューをその更新プログラムのセットに限定することができます。 **[リスト]** ビューまたは **[テーブル]** ビューを選択すると、個別のレコードが返されます。<br> 

![ログ検索の更新ビュー (レコードの種類 Update)](./media/oms-solution-update-management/update-la-view-updates.png)  

**[テーブル]** ビューでは、任意のレコードの **KBID** をクリックすると、ブラウザーが起動してそのサポート技術情報の記事が表示されます。 これにより、特定の更新プログラムの詳細をすぐに確認できます。<br> 

![ログ検索のテーブル ビュー (タイルのレコードの種類 Updates)](./media/oms-solution-update-management/update-la-view-table.png)

**[リスト]** ビューでは、KBID の横にある **[表示]** リンクをクリックすると、サポート技術情報の記事が開きます。<br>

![ログ検索のテーブル ビュー (タイルのレコードの種類 Updates)](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>UpdateSummary レコード
**UpdateSummary** という種類のレコードは、Windows エージェント コンピューターごとに作成されます。 このレコードは、更新プログラムについてコンピューターがスキャンされるたびに更新されます。 **UpdateSummary** レコードには、次の表に示したプロパティがあります。

| プロパティ | 説明 |
| --- | --- |
| 型 |UpdateSummary |
| SourceSystem |OpsManager |
| コンピューター |コンピューターの名前。 |
| CriticalUpdatesMissing |コンピューターに適用されていない緊急更新プログラムの数。 |
| ManagementGroupName |SCOM エージェントの管理グループの名前。 その他のエージェントの場合、これは AOI-<workspace ID> です。 |
| NETRuntimeVersion |コンピューターにインストールされている .NET ランタイムのバージョン。 |
| OldestMissingSecurityUpdateBucket |このコンピューターで適用されていない最も古いセキュリティ更新プログラムが公開されてからの時間を分類するバケット。<br>次のいずれかの値になります。<br>- それより以前<br>- 180 日前<br>- 150 日前<br>- 120 日前<br>- 90 日前<br>- 60 日前<br>- 30 日前<br>- 最近 |
| OldestMissingSecurityUpdateInDays |このコンピューターで適用されていない最も古いセキュリティ更新プログラムが公開されてからの日数。 |
| OsVersion |コンピューターにインストールされているオペレーティング システムのバージョン。 |
| OtherUpdatesMissing |コンピューターに適用されていないその他の更新プログラムの数。 |
| SecurityUpdatesMissing |このコンピューターに適用されていないセキュリティ更新プログラムの数。 |
| SourceComputerId |コンピューターを一意に識別する GUID。 |
| TimeGenerated |レコードの最終更新日時。 |
| TotalUpdatesMissing |コンピューターに適用されていない更新プログラムの合計数。 |
| WindowsUpdateAgentVersion |コンピューター上の Windows Update エージェントのバージョン番号。 |
| WindowsUpdateSetting |コンピューターで重要な更新プログラムをインストールする方法に関する設定。<br>次のいずれかの値になります。<br>- 無効<br>- Notify before installation (インストール前に通知)<br>- Scheduled installation (スケジュールに従ってインストール) |
| WSUSServer |WSUS サーバーの URL (コンピューターがそれを使用するように構成されている場合)。 |

## <a name="sample-log-searches"></a>サンプル ログ検索
次の表は、このソリューションによって収集された更新レコードを探すログ検索の例です。 

| クエリ | Description |
| --- | --- |
|更新プログラムが必要な Windows ベースのサーバー コンピューター |`Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false | measure count() by Computer` |
|更新プログラムが必要な Linux サーバー | `Type:Update OSType=Linux UpdateState!="Not needed" | measure count() by Computer` |
| 更新プログラムがインストールされていないすべてのコンピューター |`Type=Update UpdateState=Needed Optional=false | select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| 特定のコンピューターにインストールされていない更新プログラム (コンピューター名は実際の名前に置き換えてください) |`Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" | select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate` |
| All computers with missing critical or security updates (緊急更新プログラムまたはセキュリティ更新プログラムがインストールされていないすべてのコンピューター) |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates"`) |
| Critical or security updates needed by machines where updates are manually applied (更新プログラムが手動で適用されるコンピューターに必要な、緊急更新プログラムまたはセキュリティ更新プログラム) |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | Distinct Computer} | Distinct KBID` |
| Error events for machines that have missing critical or security required updates (必要とされている緊急更新プログラムまたはセキュリティ更新プログラムがインストールされていないコンピューターのエラー イベント) |`Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false | Distinct Computer}` |
| 更新プログラムのロールアップがインストールされていないすべてのコンピューター |`Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed| select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| 全コンピューターのインストールされていない個別の更新プログラム |`Type=Update UpdateState=Needed Optional=false | Distinct Title` |
| 更新実行に失敗した更新プログラムがある Windows ベースのサーバー コンピューター | `Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Title, UpdateRunName` |
| 更新実行に失敗した更新プログラムがある Linux サーバー |`Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Product, UpdateRunName` |
| WSUS コンピューター メンバーシップ |`Type=UpdateSummary | measure count() by WSUSServer` |
| 自動更新の構成 |`Type=UpdateSummary | measure count() by WindowsUpdateSetting` |
| 自動更新が無効にされたコンピューター |`Type=UpdateSummary WindowsUpdateSetting=Manual` |
| List of all the Linux machines which have a package update available (パッケージ更新プログラムが使用可能なすべての Linux マシンの一覧) |`Type=Update and OSType=Linux and UpdateState!="Not needed" | measure count() by Computer` |
| List of all the Linux machines which have a package update available which addresses Critical or Security vulnerability (重大な脆弱性またはセキュリティの脆弱性に対処するパッケージ更新プログラムが使用可能なすべての Linux マシンの一覧) |`Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") | measure count() by Computer` |
| List of all packages that have an update available (更新プログラムが使用可能なすべてのパッケージの一覧) |Update and OSType=Linux and UpdateState!="Not needed" |
| List of all packages that have an update available which addresses Critical or Security vulnerability (重大な脆弱性またはセキュリティの脆弱性に対処する更新プログラムが使用可能なすべてのパッケージの一覧) |`Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates")` |
| コンピューターに変更を加えた更新プログラムの展開の一覧 |`Type:UpdateRunProgress | measure Count() by UpdateRunName` |
|この更新実行で更新されたコンピューター (更新プログラムの展開名は実際の名前に置き換えてください) |`Type:UpdateRunProgress UpdateRunName="DeploymentName" | measure Count() by Computer` |
| List of all the “Ubuntu” machines with any update available (任意の更新プログラムが使用可能なすべての "Ubuntu" マシンの一覧) |`Type=Update and OSType=Linux and OSName = Ubuntu &| measure count() by Computer` |

## <a name="troubleshooting"></a>トラブルシューティング 

このセクションでは、更新管理ソリューションに関する問題のトラブルシューティングに役立つ情報について説明します。  

### <a name="how-do-i-troubleshoot-update-deployments"></a>更新プログラムの展開のトラブルシューティングを行うにはどうすればよいですか。
このソリューションをサポートしている OMS ワークスペースにリンクされた Automation アカウントの [ジョブ] ブレードで、スケジュールされた更新プログラムの展開に含まれた更新プログラムをデプロイする Runbook の結果を表示できます。  Runbook **Patch-MicrosoftOMSComputer** は、管理されたコンピューター 1 台を対象とする子 Runbook です。詳細ストリームを確認すると、そのデプロイに関する詳細情報を把握できます。  出力に、適用可能な必須の更新プログラム、ダウンロード状態、インストール状態、その他の詳細情報が表示されます。<br><br> ![更新プログラムの展開ジョブの状態](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

詳細については、[Automation Runbook の出力とメッセージ](../automation/automation-runbook-output-and-messages.md)に関するページを参照してください。   
  
## <a name="next-steps"></a>次のステップ
* [Log Analytics](../log-analytics/log-analytics-log-searches.md) でログ検索を使用して、詳細な更新プログラムデータを確認します。
* 管理対象のコンピューターで更新プログラムが準拠しているかどうかを示す[独自のダッシュボードを作成](../log-analytics/log-analytics-dashboards.md)します。
* 緊急更新プログラムがコンピューターにインストールされていないと検出された場合またはコンピューターで自動更新が無効になっている場合、[アラートを作成](../log-analytics/log-analytics-alerts.md)します。  


