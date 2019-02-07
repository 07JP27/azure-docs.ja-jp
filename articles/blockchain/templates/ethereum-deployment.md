---
title: Ethereum プルーフオブワーク コンソーシアム ソリューション テンプレート
description: Ethereum Proof-of-Work Consortium ソリューション テンプレートを使用してマルチメンバー コンソーシアム型 Ethereum ネットワークをデプロイして構成する
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/28/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: 266e2be2775a6f9b74c714bd9112e38837bb6a6c
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098340"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Ethereum プルーフオブワーク コンソーシアム ソリューション テンプレート

Ethereum プルーフオブワーク コンソーシアム ソリューション テンプレートは、Azure と Ethereum の最小限の知識で、マルチメンバー コンソーシアム型 Ethereum ネットワークを簡単かつ短時間でデプロイおよび構成できるように設計されています。

各メンバーは、Azure Resource Manager テンプレートを使用して、Microsoft Azure Compute、ネットワーク、およびストレージ サービスを使用するネットワーク フットプリントをプロビジョニングできます。 各メンバーのネットワーク フットプリントは一連の負荷分散型トランザクション ノードで構成されます。アプリケーションまたはユーザーはそれらのノードと対話して、トランザクション、トランザクションを記録する一連のマイニング ノード、および VPN ゲートウェイを送信します。 デプロイ後、ゲートウェイを接続して、完全に構成されたマルチメンバー ブロックチェーン ネットワークを作成します。

## <a name="about-blockchain"></a>ブロックチェーンの概要

ブロックチェーン コミュニティに参加したばかりなら、このソリューションのリリースをきっかけにしてこの技術を学習してください。Azure 上で簡単に、また構成しながら学習できます。 ただし、マルチメンバー コンソーシアム ネットワークを構築する前に、まずこのガイド付きチュートリアルを参照して、より簡単なスタンドアロン型 Ethereum ネットワーク トポロジをデプロイすることをお勧めします。

### <a name="mining-node-details"></a>マイニング ノードの詳細

トランザクションをマイニングするノードは、トランザクションを受け入れるノードと分離されています。これは、2 つのアクションが同じリソースに対して競合しないようにするためです。

コンソーシアム メンバーは、マネージド ディスクでバッキングされた 1 つ以上のマイニング ノードを含む最大 5 つのリージョンをプロビジョニングできます。 リージョン内の 1 つ以上のノードが、ネットワーク内のノードの動的検出機能をサポートするブート ノードとして構成されます。 マイニング ノードは他のマイニング ノードと通信して、基礎となる分散型台帳の状態について合意を形成します。 これらのノードについて、アプリケーションは認識したり通信したりする必要はありません。 マイニング ノードはプライベート ネットワークに集中し、受信パブリック インターネット トラフィックからは隔離されるので、第 2 の保護層が追加されます。 送信トラフィックは許可されていますが、Ethereum 検出ポートに対する送信は許可されていません。

すべてのノードには安定したバージョンの Go Ethereum (Geth) クライアントがインストールされ、マイニング ノードとして構成されています。 カスタム ジェネシス ブロックを指定しなかった場合、すべてのノードは同じ Ethereum アドレスを使用し、その Ethereum アカウントのパスワードで保護されているキー ペアを使用します。 指定した Ethereum パスフレーズは、各マイニング ノードの既定アカウント (コインベース) を生成するために使用されます。 マイニング ノードなので、マイニングを行います。つまりこのアカウントに追加された料金を収集します。

コンソーシアム メンバーあたりのマイニング ノード数は、必要なネットワーク全体のサイズと、各メンバー専用に割り当てられるハッシュの計算能力によって変わります。 ネットワークが大きくなるほど、ノード数が多くなるので、有利な立場を得るには妥協が必要です。 このテンプレートは、仮想マシン スケール セットを使用してプロビジョニングされたマイニング ノードを、リージョンあたり最大 15 個までサポートします。

### <a name="transaction-node-details"></a>トランザクション ノードの詳細

コンソーシアム メンバーには、一連の負荷分散型トランザクション ノードもあります。 これらのノードには、仮想ネットワークの外部から到達可能です。そのため、アプリケーションはこれらのノードを使用して、ブロックチェーン ネットワーク内でトランザクションを送信したり、スマート コントラクトを実行したりすることができます。 すべてのノードには安定したバージョンの Go Ethereum (Geth) クライアントがインストールされ、分散型台帳の完全なコピーを維持するように構成されています。 カスタム ジェネシス ブロックを指定していない場合、これらのノードは同じ Ethereum アカウントを使用します。このアカウントは Ethereum アカウントのパスワードで保護されています。

トランザクション ノードは、高可用性を維持するために、可用性セット内で負荷分散されています。 このテンプレートは、仮想マシン スケール セットを使用してプロビジョニングされたトランザクション ノードを最大 5 個までサポートします。

### <a name="log-analytics-details"></a>Log Analytics の詳細

デプロイごとに、新しい Log Analytics インスタンスを作成するか、既存のインスタンスに参加できます。 Log Analytics により、デプロイされたネットワークを構成する各仮想マシンのさまざまなパフォーマンス メトリックを監視できます。

## <a name="deployment-architecture"></a>デプロイメント アーキテクチャ

### <a name="description"></a>説明

このソリューション テンプレートは、単一または複数のリージョンベースのマルチメンバー Ethereum コンソーシアム ネットワークをデプロイできます。 各リージョンの仮想ネットワークは、VNET ゲートウェイと接続リソースを使用して、チェーン トポロジ内の他のリージョンに接続されます。 また、レジストラーがプロビジョニングされます。レジストラーには、各リージョンにデプロイされたすべてのマイニング ノードとトランザクション ノードに必要な情報が含まれています。

### <a name="standalone-and-consortium-leader-overview"></a>スタンドアロンとコンソーシアム リーダーの概要

![スタンドアロンとコンソーシアム リーダーの概要](./media/ethereum-deployment/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>コンソーシアム メンバーへの参加の概要

![コンソーシアム メンバーへの参加の概要](./media/ethereum-deployment/member-overview.png)

## <a name="getting-started"></a>使用の開始

このプロセスでは、複数の仮想マシン スケール セットとマネージド ディスクのデプロイに対応できる Azure サブスクリプションが必要です。 必要に応じて、無料の Azure アカウントを作成して始めてください。

サブスクリプションを取得したら、Azure portal を開きます。 **[+ リソースの作成]** を選択し、Marketplace で [すべて表示] を選択して、「**Ethereum Proof-of-Work Consortium**」(Ethereum プルーフオブワーク コンソーシアム) を検索します。

テンプレートのデプロイでは、ネットワークで最初のメンバーのフットプリントを構成する手順が示されます。 デプロイ フローは、基本、Operations Management Suite、デプロイ リージョン、ネットワークのサイズとパフォーマンス、Ethereum の設定という 5 つの手順に分かれています。

### <a name="basics"></a>基本

**[基本]** では、サブスクリプション、リソース グループ、基本仮想マシンのプロパティなど、デプロイの標準パラメーターの値を指定します。

![基本](./media/ethereum-deployment/sample-deployment.png)

パラメーター名|説明| 使用できる値|既定値
---|---|---|---
Create a new network or join existing network? (ネットワークの新規作成または既存のネットワークに参加)|新しいネットワークを作成するか、既存のコンソーシアム ネットワークに参加します|[新規作成] または [Join Existing]\(既存に参加\)|Create New
Deploy a network that will be part of a consortium? (コンソーシアムに今後参加するネットワークをデプロイする)|コンソーシアム ネットワークが、今後、デプロイがこのネットワークに参加することを許可しています (*[新規作成]* を選択した場合に表示されます)|[スタンドアロン] または [Consortium]\(コンソーシアム\)|スタンドアロン
Resource Prefix (リソース プレフィックス) |リソースの名前付けのベースとして使用される文字列 (2 から 4 文字の英数字)。 一部のリソースには一意のハッシュがソースの先頭に追加されていますが、リソース固有の情報が付加されます。|長さ 2 から 4 文字の英数字|NA
VM ユーザー名| デプロイされた各 VM の管理者ユーザー名 (英数字のみ)|1 から 64 文字 |gethadmin
認証の種類|仮想マシンに対して認証する方法。 |[パスワード] または [SSH 公開キー]|パスワード
パスワード ([認証の種類] = [パスワード])|デプロイされた各仮想マシンの管理者アカウントのパスワード。 パスワードには、小文字、大文字、数字、特殊文字の 4 種類のうち 3 種類を使用する必要があります。 <br />VM にはすべて、最初の段階で同じパスワードが与えられます。プロビジョニング後にそのパスワードを変更できます。|12 から 72 文字|NA
SSH キー ([認証の種類] = [公開キー])|リモート ログインに使用される Secure Shell キー。|| NA
サブスクリプション| コンソーシアム ネットワークをデプロイするサブスクリプション||NA
リソース グループ| コンソーシアム ネットワークをデプロイするリソース グループ||NA
場所| リソース グループの Azure リージョン。 ||NA

### <a name="operations-management-suite"></a>Operations Management Suite

Operations Management Suite (OMS) で、ネットワークの OMS リソースを構成できます。 OMS では、ネットワークから役に立つメトリックとログが収集および表示され、ネットワークの正常性やデバッグの問題をすばやく確認することができます。 容量に達すると、OMS は無料提供されなくなります。

![新しい OMS の作成](./media/ethereum-deployment/new-oms.png)

パラメーター名|説明| 使用できる値|既定値
---|---|---|---
Connect to existing OMS (既存の OMS に接続)|新しい Log Analytics インスタンスを作成するか、既存のインスタンスに参加します|[新規作成] または [Join existing]\(既存に参加\)|[新規作成] / Log Analytics Location (Log Analytics のリージョン)|新しい Log Analytics がデプロイされるリージョン (*[新規作成]* を選択すると表示されます)
Existing OMS Workspace Id (既存の OMS ワークスペース ID)|既存のインスタンスのワークスペース ID (*[Join existing]\(既存に参加\)* を選択すると表示されます) / OMS Service Tier (OMS サービス プラン)|新しいインスタンスの価格レベルを選択します。 詳細については https://azure.microsoft.com/pricing/details/log-analytics/ を参照してください (*[Join existing]\(既存に参加\)* を選択すると表示されます)|[無料]、[スタンドアロン]、[Per Node]\(ノードごと\)|無料
Existing OMS Primary Key (既存の OMS の主キー)|既存の OMS インスタンスへの接続に使用される主キー (*[Join existing]\(既存に参加\)* を選択すると表示されます)

### <a name="deployment-regions"></a>Deployment regions (デプロイ リージョン)

次に、**[Deployment regions]\(デプロイ リージョン\)** で、コンソーシアム ネットワークをデプロイする **[Number of region(s)]\(リージョン数\)** を入力し、指定したリージョン数に基づいて Azure リージョンを選択します。 最大 5 つのリージョンにデプロイできます。

![Deployment regions (デプロイ リージョン)](./media/ethereum-deployment/deployment-regions.png)

パラメーター名| 説明| 使用できる値 |既定値
---|---|---|---
Number of region(s) (リージョン数)| コンソーシアム ネットワークをデプロイするリージョンの数|1、2、3、4、5| 2
First region (リージョン 1)| コンソーシアム ネットワークをデプロイする 1 つ目のリージョン|許可されているすべての Azure リージョン| 米国西部
Second region (リージョン 2) |コンソーシアム ネットワークをデプロイする 2 つ目のリージョン (リージョン数を 2 以上に指定した場合にのみ表示されます)|許可されているすべての Azure リージョン| 米国東部
Third region (リージョン 3)| コンソーシアム ネットワークをデプロイする 3 つ目のリージョン (リージョン数を 3 以上に指定した場合にのみ表示されます)|許可されているすべての Azure リージョン| 米国中央部
Fourth region (リージョン 4)| コンソーシアム ネットワークをデプロイする 4 つ目のリージョン (リージョン数を 4 以上に指定した場合にのみ表示されます)|許可されているすべての Azure リージョン| 米国東部 2
Fifth region (リージョン 5)| コンソーシアム ネットワークをデプロイする 5 つ目のリージョン (リージョン数を 5 に指定した場合にのみ表示されます)|許可されているすべての Azure リージョン| 米国西部 2

### <a name="network-size-and-performance"></a>Network size and performance (ネットワークのサイズとパフォーマンス)

次に、**[Network Size and Performance]\(ネットワークのサイズとパフォーマンス\)** で、コンソーシアム ネットワークのサイズを入力します。 たとえば、マイニング ノードとトランザクション ノードの数やサイズです。

![Network size and performance (ネットワークのサイズとパフォーマンス)](./media/ethereum-deployment/network-size-performance.png)

パラメーター名 |説明 |使用できる値| 既定値
---|---|---|---
Number of mining nodes (マイニング ノード数)|リージョンごとのデプロイされるマイニング ノードの数|2 から 15| 2
Mining node storage performance (マイニング ノード ストレージのパフォーマンス)|デプロイされる各マイニング ノードをバッキングするマネージド ディスクの種類。|Standard または Premium|標準
Mining node virtual machine size (マイニング ノードの仮想マシンのサイズ)|マイニング ノードに使用される仮想マシンのサイズ。|Standard A、 <br />Standard D、 <br />Standard D-v2、 <br />Standard F シリーズ、 <br />Standard DS、 <br />Standard FS|Standard D1v2
Number of load balanced transaction nodes (負荷分散型トランザクション ノード数)|ネットワークの一部としてプロビジョニングするトランザクション ノードの数。|1 から 5| 2
Transaction node storage performance (トランザクション ノード ストレージのパフォーマンス)|デプロイされる各トランザクション ノードをバッキングするマネージド ディスクの種類。|Standard または Premium|標準
Transaction node virtual machine size (トランザクション ノードの仮想マシンのサイズ)|トランザクション ノードに使用される仮想マシンのサイズ。|Standard A、 <br />Standard D、 <br />Standard D-v2、 <br />Standard F シリーズ、 <br />Standard DS、 <br />Standard FS|Standard D1v2

### <a name="ethereum-settings"></a>Ethereum settings (Ethereum の設定)

次に、**[Ethereum settings]\(Ethereum の設定\)** で、ネットワーク ID と Ethereum アカウントのパスワード、ジェネシス ブロックなど、Ethereum 関連の構成設定を指定します。

![Ethereum settings (Ethereum の設定)](./media/ethereum-deployment/standalone-leader-deployment.png)

パラメーター名 |説明 |使用できる値|既定値
---|---|---|---
ConsortiumMember ID (コンソーシアム メンバー ID)|コンソーシアム ネットワークに参加する各メンバーに関連付けられている ID。競合を回避するように IP アドレス 空間を構成するために使用されます。 <br /><br />メンバー ID は、同じネットワーク内の複数の組織全体で一意である必要があります。 同じ組織が複数のリージョンにデプロイする場合でも、一意のメンバー ID が必要です。<br /><br />他の参加メンバーと共有する必要があるため、このパラメーターの値を書き留めておきます。|0 から 255
Ethereum Network ID (Ethereum ネットワーク ID)|デプロイされているコンソーシアム Ethereum ネットワークのネットワーク ID。 各 Ethereum ネットワークには独自のネットワーク ID があり、1 はパブリック ネットワークの ID を示します。 マイニング ノードに対するネットワーク アクセスは制限されていますが、競合を防ぐために大きな数を指定することをお勧めします。|5 から 999,999,999| 10101010
Custom genesis block (カスタム ジェネシス ブロック)|ジェネシス ブロックを自動的に生成するか、カスタム ブロックを指定するオプション。|はい/いいえ| いいえ 
Ethereum Account Password (Ethereum アカウントのパスワード) ([Custom genesis block]\(カスタム ジェネシス ブロック\) = [いいえ])|各ノードにインポートされた Ethereum アカウントをセキュリティで保護するために使用される管理者パスワード。 パスワードには、1 文字の大文字、1 文字の小文字、および 1 文字の数字を含める必要があります。|12 文字以上|NA
Ethereum private key passphrase (Ethereum 秘密キーのパスフレーズ) ([Custom genesis block]\(カスタム ジェネシス ブロック\) = [いいえ])|生成される既定の Ethereum アカウントに関連付けられた、ECC 秘密キーの生成に使用されるパスフレーズ。 事前に生成された秘密キーを明示的に渡す必要はありません。<br /><br />秘密キーを強固にして、他のコンソーシアム メンバーと重複しないように、十分なランダム性を持つパスフレーズを検討してください。 パスフレーズには、少なくとも1 文字の大文字、1 文字の小文字、および 1 文字の数字を含める必要があります。<br /><br />2 人のメンバーが同じパスフレーズを使用すると、生成されるアカウントは同じになります。 単一の組織がリージョンをまたがってデプロイする際に、すべてのノードで単一のアカウント (コイン ベース) を共有する場合は、同じパスフレーズを使用すると便利です。|12 文字以上|NA
Genesis block (ジェネシス ブロック) ([Custom genesis block]\(カスタム ジェネシス ブロック\) = [はい])|カスタム ジェネシス ブロックを表す JSON 文字列。 ジェネシス ブロックの形式の詳細については、「Custom Networks」(カスタム ネットワーク) を参照してください。<br /><br />カスタム ジェネシス ブロックを指定した場合でも、Ethereum アカウントは作成されます。 ジェネシス ブロックには、マイニングを待たずに、事前に資金を提供した Ethereum アカウントを指定することを検討してください。|有効な JSON |NA
Shared Key for Connection (接続の共有キー)|VNET ゲートウェイ間の接続に使用される共有キー。| 12 文字以上|NA
Consortium Data URL (コンソーシアム データの URL)|他のメンバーのデプロイによって提供された、関連するコンソーシアム構成データを指す URL。 <br /><br />この情報は、デプロイを持っている接続済みメンバーから提供されます。 他のネットワークをデプロイ済みの場合、URL は CONSORTIUM-DATA というテンプレート デプロイ出力に指定されています。||NA
VNet Gateway to Connect to (接続先の VNet ゲートウェイ)|接続先の VNet ゲートウェイのリソース パス。<br />この情報は、デプロイを持っている接続済みメンバーから提供されます。 他のネットワークをデプロイ済みの場合、URL は CONSORTIUM_MEMBER_GATEWAY_ID というテンプレート デプロイ出力に指定されています。 注:同じメンバーのコンソーシアム データ URL と VNet ゲートウェイ リソースを使用する必要があります。||NA
Endpoint of Peer information registrar (ピア情報レジストラーのエンドポイント)|他のメンバーのデプロイから提供されるピア情報のエンドポイント|コンソーシアムの最初のメンバーの有効なエンドポイント|NA
Key of Peer information registrar (ピア情報レジストラーのキー)|他のメンバーのデプロイから提供されるピア情報の主キー|コンソーシアムの最初のメンバーの有効な主キー|NA

### <a name="summary"></a>まとめ

[概要] の各項目をクリックして、指定した内容を確認し、基本的なデプロイ前検証を実行します。

![まとめ](./media/ethereum-deployment/summary.png)

法律条項とプライバシー条項を確認し、**[購入]** をクリックしてデプロイします。 デプロイに複数のリージョンがある場合、またはコンソーシアム ネットワーク用のデプロイの場合、他のメンバーとのネットワーク接続をサポートするために、このテンプレートで必要な VPN ゲートウェイを事前にデプロイします。 ゲートウェイのデプロイには最大 45 から 50 分かかります。

## <a name="post-deployment-sanity-checks"></a>デプロイ後のサニティ チェック

### <a name="administrator-page"></a>管理者ページ

デプロイが正常に完了し、すべてのリソースがプロビジョニングされたら、管理者ページにアクセスし、ブロックチェーン ネットワークを表示し、デプロイ状態のサニティ チェックを実行できます。 管理者ページの URL は、ロードバランサーの DNS 名です。この URL は、ADMIN_SITE というテンプレート デプロイの出力セクションに指定されています。

この情報を見つけるには、デプロイされたリソース グループを選択します。 次に、**[概要]** を選択し、**[デプロイ]** のすぐ下にある、成功した数を示すリンクをクリックします。

![リソース グループの概要](./media/ethereum-deployment/resource-overview.png)

新しい画面にデプロイの履歴が表示されます。 最初のデプロイ リソース (たとえば、microsoft-azure-blockchain.azure-blockchain-servi...) を選択し、ページの後半にある **[出力]** セクションを探します。 管理ページの URL は、ADMIN_SITE というテンプレート デプロイ出力パラメーターに指定されています。

![リソースのデプロイ](./media/ethereum-deployment/resource-deployments.png)

![デプロイの出力](./media/ethereum-deployment/deployment-output.png)

管理者ページにアクセスするには、**ADMIN-SITE** の出力をコピーして別のタブで開きます。

管理者ページで [Ethereum Node Status section.]\(Ethereum ノードの状態\) セクションを見て、デプロイされたトポロジの概要を把握することができます。 すべてのノードのホスト名、そのピア数、表示された最新のブロックが表示されます。 各ノードのピア数は、1 以上、*合計ノード数*と構成済み最大ピア数未満です。 ピア数で、ネットワーク内にデプロイできるノードの数が制限されない点に注意してください。
場合によっては、ピア数が変動し、(ノードの合計 - 1) 未満になることがあります。 数の変動は、ノードが正常ではないという印とは限りません。台帳のフォークによって、ピア数に軽微な変動が発生する可能性があるためです。 最後に、ネットワーク内の各ノードから表示された最新のブロックを調べて、システムのフォークまたはラグを判断できます。

![ノードの状態](./media/ethereum-deployment/node-status.png)

ノードの状態は 10 秒ごとに更新されます。 ブラウザーを使用してページを再度読み込むか、**[再読み込み]** ボタンをクリックしてビューを更新します。

### <a name="oms-portal"></a>OMS ポータル

OMS ポータルを表示するには、デプロイ出力のリンク (OMSPORTALURL) 先にアクセスするか、デプロイ済みのリソース グループの OMS リソースを選択します。

![OMS の URL](./media/ethereum-deployment/oms-url.png)

![OMS のリソース](./media/ethereum-deployment/oms-resource.png)

ポータルには、まず概要として高レベルのネットワーク統計情報が表示されます。

![OMS の概要](./media/ethereum-deployment/oms-overview.png)

概要をクリックすると、ノードごとの統計情報を確認できるポータルが表示されます。

![ノードごとの統計情報](./media/ethereum-deployment/per-node-stats.png)

### <a name="accessing-nodes"></a>ノードへのアクセス

指定した管理者ユーザー名とパスワード/SSH キーを利用して、SSH 経由でトランザクション ノードの仮想マシンにリモート接続できます。 トランザクション ノード VM には独自のパブリック IP アドレスがないため、ロード バランサーを通過し、ポート番号を指定する必要があります。 最初のトランザクション ノードにアクセスするために実行する SSH コマンドは、**SSH_TO_FIRST_TX_NODE** (サンプル デプロイの場合は ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com) というテンプレート デプロイ出力パラメーターに指定されています。 トランザクション ノードを追加するには、ポート番号を 1 ずつ増やします (たとえば、最初のトランザクション ノードはポート 4000 です)。

マイニング ノードが実行されている仮想マシンには、外部からアクセスできないため、いずれかのトランザクション ノードを経由する必要があります。 トランザクション ノードに対する SSH セッションが確立したら、トランザクション ノードに秘密キーをインストールするか、パスワードを使用して、任意のマイニング ノードに対する SSH セッションを開始します。

**注**

ホスト名は、管理サイトまたは Azure portal から取得できます。 Azure portal では、仮想マシン スケール セット (VMSS) リソース内にあるノードのホスト名は、**[インスタンス]** 以下に表示されます。これは実際のホスト名とは異なります。 たとえば、Azure portal のホスト名は **mn-asdfmv-reg1_0** のように表示されますが、実際のホスト名は **mn-asdfmv-reg** などです。

例: 

Azure portal のホスト名| 実際のホスト名
---|---
mn-ethwvu-reg1_0| mn-ethwvu-reg1000000
mn-ethwvu-reg1_1 |mn-ethwvu-reg1000001
mn-ethwvu-reg1_2 |mn-ethwvu-reg1000002

## <a name="adding-a-new-consortium-member"></a>新しいコンソーシアム メンバーの追加

### <a name="sharing-data"></a>データの共有

コンソーシアムの最初のメンバー (または接続メンバー) になったら、他のメンバーが参加して接続を確立できるように、いくつかの情報を他のメンバーに提供する必要があります。 具体的には次の処理が行われます。

1. **共有コンソーシアム構成データ**:2 人のメンバー間の Ethereum 接続を調整するために使用される一連のデータがあります。 ジェネシス ブロック、コンソーシアム ネットワーク ID、ブート ノードなどの必要な情報は、リーダーまたは他のデプロイ済みメンバーのトランザクション ノード上のファイルに書き込まれます。 このファイルの場所は、**CONSORTIUM-DATA** というテンプレート デプロイ出力パラメーターに指定されています。
2. **ピア情報のエンドポイント**:ピア情報レジストラーのエンドポイント。リーダーまたは他のメンバーのデプロイから、既に Ethereum ネットワークに接続されているすべてのノードの情報を取得します。 DB には、ネットワーク内の接続されている各ノードに関する情報 (ノードのホスト名、プライベート IP アドレスなど) が格納されています。これは、**PEER_INFO_ENDPOINT** というテンプレート デプロイ出力パラメーターに指定されています。
3. **ピア情報の主キー**:ピア情報レジストラーの主キーは、リーダーまたは他のメンバーのピア情報の主キーにアクセスするために使用されます。 これは、**PEER_INFO_PRIMARY_KEY** というテンプレート デプロイ出力パラメーターに指定されています。


4. **VNET ゲートウェイ**:各メンバーは、既存のメンバーを介してブロックチェーン ネットワーク全体への接続を確立します。 VNET に接続するには、接続先メンバーの VNET ゲートウェイへのリソース パスが必要です。 これは、**CONSORTIUM_MEMBER_GATEWAY_ID** というテンプレート デプロイ出力パラメーターに指定されています。
5. **共有キー:** 接続が確立されているコンソーシアム ネットワークの 2 人のメンバー間で事前に確立されたシークレット。 これは、デプロイのコンテキスト外で合意された英数字の文字列 (1 から 128 文字) です (たとえば、**MySharedKeyAbc123**)。

### <a name="acceptance-of-new-member"></a>新しいメンバーの受け入れ

この手順は、参加するメンバーがネットワークを正常にデプロイした後に実行する必要があります。 メンバーがネットワークに参加してトランザクション トラフィックを表示する前に、既存のメンバーが VPN Gateway 上で最終的な構成を実行し、接続を受け入れる必要があります。 つまり、参加するメンバーの Ethereum ノードは、接続が確立するまで実行されません。 この構成は、PowerShell または xPlat CLI を使用して行うことができます。 PowerShell モジュールと xPlat CLI スクリプトは、コンソーシアム データと共にトランザクション ノードに格納されます。 スクリプトの場所は、それぞれ **PAIR-GATEWAY-PS-MODULE** および **PAIR-GATEWAY-AZURE-CLISCRIPT** というデプロイ出力パラメーターに指定されています。

**PowerShell/CLI のセットアップ**

Azure PowerShell コマンドレットと Azure xPlat CLI のその他の基本的な使用方法については、Azure ドキュメントを参照してください。

Azure コマンドレットの最新バージョンがローカルにインストールされ、セッションが開かれている必要があります。 Azure サブスクリプションの資格情報を使用してセッションにログインしてください。

**PowerShell: 接続を確立する**

PowerShell モジュールをダウンロードし、ローカルに保存します。 PowerShell モジュールの場所は、**PAIR-GATEWAY-PS-MODULE** テンプレート デプロイ出力パラメーターに指定されています。

まだ有効ではない場合は、ローカル セッションに対して **Set-ExecutionPolicy** コマンドレットを実行して、符号なしモジュールを実行できるようにします。

**Set-ExecutionPolicy Unrestricted CurrentUser**

次に、以下を実行して、リーダーのデプロイを行った Azure サブスクリプションにログインします

**Login-AzureRmAccount**

次に、モジュールをインポートします。

**Import-Module <filepath to downloaded file>**

最後に、適切な入力内容で関数を実行します。

- **MyGatewayResourceId:** ゲートウェイのリソース パス。 これは、**CONSORTIUM_MEMBER_GATEWAY_ID** というテンプレート デプロイ出力パラメーターに指定されています。
- **OtherGatewayResourceId:** 参加するメンバーのゲートウェイのリソース パス。 これは、参加するメンバーから提供されます。**CONSORTIUM_MEMBER_GATEWAY_ID** というテンプレート デプロイ出力パラメーターに指定されています。
- **ConnectionName:** このゲートウェイ接続を識別するための名前。
- **共有キー:** 接続が確立されているコンソーシアム ネットワークの 2 人のメンバー間で事前に確立されているシークレット。

**CreateConnection** - MyGatewayResourceId <resource path of your Gateway> -OtherGatewayResourceId <参加するメンバーのゲートウェイのリソース パス> -ConnectionName myConnection -SharedKey "MySharedKeyAbc123"

**xPlat CLI:接続を確立する**

Azure CLI スクリプトをダウンロードし、ローカルに保存します。 Azure CLI スクリプトの場所は、**PAIR-GATEWAY-AZURE-CLI-SCRIPT** というテンプレート デプロイ パラメーターに指定されています。

適切な入力内容でスクリプトを実行します。

- **MyGatewayResourceId:** ゲートウェイのリソース パス。 これは、**CONSORTIUM_MEMBER_GATEWAY_ID** というテンプレート デプロイ出力パラメーターに指定されています。
- **OtherGatewayResourceId:** 参加するメンバーのゲートウェイのリソース パス。 これは、参加するメンバーから提供されます。デプロイの **CONSORTIUM_MEMBER_GATEWAY_ID** というテンプレート デプロイ出力パラメーターに指定されています。
- **ConnectionName:** このゲートウェイ接続を識別するための名前。
- **共有キー:** 接続が確立されているコンソーシアム ネットワークの 2 人のメンバー間で事前に確立されているシークレット。
- **[場所]:** ゲートウェイ リソースがデプロイされる Azure リージョン。

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

**リーダー**と**メンバー**間の接続を正常に構成されると、アーキテクチャは次のようになります。

![リーダーとメンバーのアーキテクチャ](./media/ethereum-deployment/leader-member.png)

## <a name="fund-new-member-account"></a>新しいメンバー アカウントへの資金提供

### <a name="generated-genesis-block"></a>生成されたジェネシス ブロック

最初のメンバーの場合、デプロイによってジェネシス ブロックが生成された場合 ([Custom Genesis Block]\(カスタム ジェネシス ブロック\) = [いいえ])、Ethereum アカウントに 1 兆 Ether の資金が提供されます。 マイニング ノードがマイニングのブロックを開始した時点では、他のメンバーは事前に資金が提供されていないアカウントを持ち、Ether が蓄積されるまで待つ必要があります。 新しいメンバーが Ether を待たないようにするには、参加するメンバーの Ethereum アカウントに明示的に資金を供給する必要があります。

これを行うには、参加するメンバーから、そのメンバーの管理者 Web サイトに表示されている Ethereum アカウントの情報を提供してもらう必要があります。 次に、こちらの管理者 Web サイトを使用して、提供されたアカウントを入力して、自分のアカウントから参加するメンバーのアカウントに Ether を提供します。

### <a name="custom-genesis-block"></a>Custom genesis block (カスタム ジェネシス ブロック)

指定した Ethereum アカウントでカスタム ジェネシス ブロックが提供された場合、MetaMask などのツールを使用して、指定されたアカウントから管理者 Web サイトに表示されている事前生成された Ethereum アカウントに Ether を送金できます。 MetaMask の使用手順については、「[Ethereum アカウントを作成する](#create-ethereum-account)」を参照してください。

アカウントなしでカスタム ジェネシス ブロックが提供された場合、または事前に割り当てられたアカウントにアクセスできない場合は、マイニング ノードがマイニングを開始し、アカウント (コインベース) に Ether が生成されるまで待つ必要があります。 資金の生成速度は、カスタム ジェネシス ブロックで指定した難易度によって変わります。

## <a name="create-ethereum-account"></a>Ethereum アカウントを作成する

追加のアカウントを作成するには、さまざまなソリューションを使用できます。 そのようなソリューションの一例として MetaMask があります。MetaMask は、ID コンテナーと、Ethereum ネットワーク パブリック、テスト、またはカスタムへの接続を提供する Chrome の拡張機能です。 MetaMask は、ネットワークにアカウントを登録するトランザクションを構成しています。

このトランザクションは、他のトランザクションと同様に、トランザクション ノードの 1 つに接続され、最終的には以下に示すように 1 つのブロックにマイニングされます。

![トランザクション ノード](./media/ethereum-deployment/transaction-node.png)

Chrome にこの拡張機能をインストールするには、Google Chrome のカスタマイズと制御 (オーバーフロー ボタン) をクリックし、[その他のツール]、[拡張機能]、[その他の拡張機能] の順に選択し、MetaMask を検索します。

![MetaMask 拡張機能](./media/ethereum-deployment/metamask-extension.png)

インストールしたら、MetaMask を開いて新しいコンテナーを作成します。 既定では、コンテナーは Morden Test Network に接続されます。 デプロイしたプライベート コンソーシアム ネットワーク、具体的にはトランザクション ノードの前面にあるロード バランサーに接続するには、これを変更します。 テンプレート出力から、ポート 8545 で公開されている Ethereum RPC エンドポイント (`ETHEREUM-RPC-ENDPOINT` という名前) を取得して、以下のようにカスタム RPC に入力します。

![MetaMask の設定](./media/ethereum-deployment/metamask-settings.png)

コンテナーを作成すると、アカウントを含むウォレットが作成されます。 追加のアカウントを作成するには、**[Switch Accounts]\(アカウントの切り替え\)** を選択してから **+** ボタンを選択します。

![MetaMask のアカウント作成](./media/ethereum-deployment/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>最初の Ether 割り当てを開始する

管理者ページを使用して、事前に割り当てられたアカウントから別の Ethereum アカウントに Ether を送金するトランザクションを構築できます。 この Ether の送金は、以下に示すように、トランザクション ノードに送信され、ブロックにマイニングされるトランザクションです。

![トランザクション ノード](./media/ethereum-deployment/transaction-node-mined.png)

MetaMask ウォレットのクリップボード アイコンを使用して、Ether の送金先 Ethereum アカウントのアドレスをコピーし、管理者ページに戻ります。 コピーしたアカウントを入力フィールドに貼り付けて、事前に割り当てられた Ethereum アカウントから新しく作成したアカウントに 1000 Ether を送金します。 **[Submit]\(送信\)** をクリックし、トランザクションがブロックにマイニングされるまで待ちます。

![ノードの状態](./media/ethereum-deployment/node-status2.png)

トランザクションがマイニング ブロックにコミットされると、アカウントの MetaMask のアカウント残高に 1000 Ether 送金が反映されます。

![MetaMask の送金](./media/ethereum-deployment/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>アカウント間の Ether の送金

この時点で、プライベート コンソーシアム ネットワーク内でトランザクションを実行する準備ができました。 最も簡単なトランザクションは、あるアカウントから別のアカウントに Ether を送金することです。 このようなトランザクションを構成するには、もう一度 MetaMask を使用して、前述の最初のアカウントから 2 番目のアカウントに送金します。

MetaMask の **Wallet 1** から [Send]\(送信\) をクリックします。 **[Recipient Address]\(受信者のアドレス\)** 入力フィールドに作成した 2 つ目のウォレットのアドレスをコピーし、**[Amount]\(金額\)** 入力フィールドに送金する Ether の金額を入力します。 **[Send]\(送信\)** をクリックし、トランザクションに同意します。

![MetaMask のトランザクションの送信](./media/ethereum-deployment/metamask-send.png)

この場合も、トランザクションがマイニングされ、ブロックにコミットされると、アカウントの残高に反映されます。 ただし、トランザクションを処理するマイニング料金を支払う必要があるため、**Wallet 1** の残高からは 15 を超える Ether が差し引かれます。

![Wallet 1](./media/ethereum-deployment/wallet1.png)

![Wallet 2](./media/ethereum-deployment/wallet2.png)

## <a name="next-steps"></a>次の手順

これで、プライベート コンソーシアム ブロックチェーン ネットワークに対するアプリケーションとスマート コントラクトの開発に集中的に取り組むことができます。
