---
title: Azure Stack での Azure App Service サーバー ロールのキャパシティ プランニング | Microsoft Docs
description: Azure Stack での Azure App Service サーバー ロールのキャパシティ プランニング
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 20b79b3c2581db94627746f52ed6837aa80b6be5
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447743"
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Azure Stack での Azure App Service サーバー ロールのキャパシティ プランニング

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

稼働準備済みの Azure App Service on Azure Stack のデプロイを設定するには、システムでサポートするキャパシティについて計画する必要があります。  

この記事では、実稼働環境のデプロイに使用すべき最小限のコンピューティング インスタンス数とコンピューティング SKU に関するガイダンスを示します。

これらのガイドラインを使用して、App Service のキャパシティ戦略を計画できます。

| App Service サーバー ロール | 推奨の最小インスタンス数 | 推奨のコンピューティング SKU|
| --- | --- | --- |
| コントローラー | 2 | A1 |
| フロントエンド | 2 | A1 |
| 管理 | 2 | A3 |
| Publisher | 2 | A1 |
| Web Worker - 共有 | 2 | A1 |
| Web Worker - 専用 | 層ごとに 2 個 | A1 |

## <a name="controller-role"></a>コントローラー ロール

**推奨の最小構成**: 2 つの A1 Standard のインスタンス

Azure App Service コントローラーでは通常、CPU、メモリ、ネットワーク リソースはそれほど消費されません。 ただし、高可用性のためには、2 つのコントローラーが必要です。 コントローラー 2 つは、許容される最大コント ローラー数でもあります。 2 番目の Web サイト コントローラーは、デプロイ中にインストーラーから直接作成できます。

## <a name="front-end-role"></a>フロントエンド ロール

**推奨の最小構成**: 2 つの A1 Standard のインスタンス

フロントエンドでは、Web worker の可用性に応じて、Web worker に要求をルーティングします。 高可用性のためには複数のフロントエンドが必要で、3 つ以上保有できます。 計画段階では、各コアが秒間約 100 リクエストを処理できると想定します。

## <a name="management-role"></a>管理ロール

**推奨の最小構成**: 2 つの A3 Standard のインスタンス

Azure App Service 管理ロールでは、App Service Azure Resource Manager および API のエンドポイント、ポータル拡張機能 (管理、テナント、Functions ポータル)、データ サービスを担当します。 通常、運用環境では、管理サーバー ロールに約 4 GB の RAM のみが必要となります。 ただし、多くの管理タスク (Web サイトの作成など) が実行される場合は、CPU レベルが高くなる可能性があります。 高可用性のためには、このロールに複数のサーバーが割り当てられている必要があり、サーバーごとに少なくとも 2 つのコアが必要です。

## <a name="publisher-role"></a>パブリッシャー ロール

**推奨の最小構成**: 2 つの A1 Standard のインスタンス

多くのユーザーが同時に公開する場合は、パブリッシャー ロールの CPU 使用率が高くなる可能性があります。 高可用性のためには、複数のパブリッシャー ロールを使用できるようにします。 パブリッシャーは、FTP または FTPS のトラフィックのみを処理します。

## <a name="web-worker-role"></a>Web Worker ロール

**推奨の最小構成**: 2 つの A1 Standard のインスタンス

高可用性のためには、少なくとも 4 つの Web worker ロールが必要です。つまり、共有 Web サイト モード用に 2 つと、提供する予定の各専用 worker 層用に 2 つです。 共有および専用のコンピューティング モードでは、さまざまなレベルのサービスがテナントに提供されます。 次のような顧客が多い場合は、より多くの Web worker が必要になる可能性があります。

- 専用コンピューティング モードの worker 層 (リソースを大量に使用) を使用している。
- 共有コンピューティング モードで稼働している。

ユーザーは、専用コンピューティング モード SKU 用の App Service プランを作成すると、その App Service プランで指定した数の Web worker を使用できなくなります。

ユーザーに従量課金プラン モデルで Azure Functions を提供するには、共有 Web worker をデプロイする必要があります。

使用する共有 Web worker ロールの数を決定する場合、次の考慮事項を検討します。

- **メモリ**:メモリは Web worker ロールの最も重要なリソースです。 メモリが不足していると、仮想メモリがディスクからスワップされるときの Web サイトのパフォーマンスに影響します。 各サーバーで、オペレーティング システム用に約 1.2 GB の RAM が必要となります。 このしきい値を超える RAM を Web サイトの実行で使用できます。
- **アクティブな Web サイトの割合**: 通常は、Azure App Service on Azure Stack デプロイ内のアプリケーションの約 5% がアクティブです。 ただし、特定の時点でのアクティブなアプリケーションの割合は、それよりも高い場合も低い場合もあります。 アクティブなアプリケーションの割合が 5% の場合、Azure App Service on Azure Stack のデプロイに配置するアプリケーションの最大数は、アクティブな Web サイトの数の 20 倍 (5 x 20 = 100) より少なくする必要があります。
- **平均メモリ占有領域**: 運用環境で計測されるアプリケーションの平均メモリ占有領域は約 70 MB です。 この占有領域を使用して、すべての Web worker ロールのコンピューターまたは VM 全体で割り当てられるメモリは、次のように計算できます。

   `Number of provisioned applications * 70 MB * 5% - (number of web worker roles * 1044 MB)`

   たとえば、10 個の Web worker ロールを実行している環境に 5,000 個のアプリケーションがある場合、各 Web worker ロール VM に 7,060 MB の RAM が必要です。

   `5,000 * 70 * 0.05 – (10 * 1044) = 7060 (= about 7 GB)`

   さらに worker インスタンスを追加する方法については、[worker ロールの追加](azure-stack-app-service-add-worker-roles.md)に関する記事をご覧ください。

## <a name="file-server-role"></a>ファイル サーバー ロール

ファイル サーバー ロールでは、開発とテスト用にスタンドアロン ファイル サーバーを使用できます。たとえば Azure Stack Development Kit (ASDK) で Azure App Service をデプロイするときに、 https://aka.ms/appsvconmasdkfstemplate のテンプレートを使用できます。 運用環境では、事前構成済みの Windows ファイル サーバーか、事前構成済みの Windows 以外のファイル サーバーを使用する必要があります。

運用環境では、ファイル サーバー ロールで集中的なディスク I/O が行なわれます。 ユーザー Web サイトのコンテンツとアプリケーション ファイルすべてが保存されるため、このロール用に次のリソースのいずれかを事前に構成しておく必要があります。

- Windows ファイル サーバー
- Windows ファイル サーバー クラスター
- Windows 以外のファイル サーバー
- Windows 以外のファイル サーバー クラスター
- NAS (ネットワーク接続ストレージ) デバイス

詳細については、[ファイル サーバーのプロビジョニング](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server)に関する記述を参照してください。

## <a name="next-steps"></a>次の手順

詳細については、次の記事を参照してください。

[Azure Stack 上の App Service を開始する前に](azure-stack-app-service-before-you-get-started.md)
