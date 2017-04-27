---
title: "Azure Resource Health でサポートされるリソースの種類 |Microsoft Docs"
description: "Azure Resource Health でサポートされるリソースの種類"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/19/2017
ms.author: BernardoAMunoz
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: ece853d90f051828e07e48110c85dd9ad2dac5d8
ms.lasthandoff: 03/31/2017

---

# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Azure Resource Health で利用できるリソースの種類と正常性チェック
Resource Health で実行されるすべてのチェックをリソースの種類別にまとめた完全な一覧を次に示します。

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|実行されるチェック|
|---|
|<ul><li>すべてのキャッシュ ノードが稼働しているか</li><li>データセンターからキャッシュにアクセスできるか</li><li>キャッシュが接続の最大数に達しているか</li><li> キャッシュが使用可能なメモリを使い果たしているか </li><li>キャッシュで多数のページ フォールトが発生しているか</li><li>キャッシュに大きな負荷がかかっているか</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|実行されるチェック|
|---|
|<ul> <li>停止、削除、または正しく構成されていないエンドポイントがあるか</li><li>CDN 構成操作で補助ポータルにアクセスできるか</li><li>CDN エンドポイントに進行中の配信に関する問題があるか</li><li>ユーザーが CDN リソースの構成を変更できるか</li><li>構成の変更が期待されるレートで反映されるか</li><li>ユーザーが Azure Portal、PowerShell、または API を使用して CDN 構成を管理できるか</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|実行されるチェック|
|---|
|<ul><li>ホスト サーバーが稼働しているか</li><li>ホスト OS の起動が完了しているか</li><li>仮想マシン コンテナーがプロビジョニングされ、オンになっているか</li><li>ホストとストレージ アカウント間のネットワーク接続が存在するか</li><li>ゲスト OS の起動が完了しているか</li><li>進行中の定期的なメンテナンスはあるか</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|実行されるチェック|
|---|
|<ul><li>データセンターからアカウントにアクセスできるか</li><li>Cognitive Services リソース プロバイダーを利用できるか</li><li>Cognitive Services が適切な地域で利用できるか</li><li>リソースのメタデータを保持しているストレージ アカウントで読み取り操作を実行できるか</li><li>API 呼び出しのクォータに達しているか</li><li>API 呼び出しの読み取り制限に達しているか</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.compute/virtualmachines
|実行されるチェック|
|---|
|<ul><li>この仮想マシンをホストするサーバーは稼働しているか</li><li>ホスト OS の起動が完了しているか</li><li>仮想マシン コンテナーがプロビジョニングされ、オンになっているか</li><li>ホストとストレージ アカウント間のネットワーク接続が存在するか</li><li>ゲスト OS の起動が完了しているか</li><li>進行中の定期的なメンテナンスはあるか</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|実行されるチェック|
|---|
|<ul><li>ユーザーがリージョンの Data Lake Analytics にジョブを送信できるか</li><li>基本的なジョブがリージョンで実行され正常に完了するか</li><li>ユーザーがリージョンのカタログ項目を一覧表示できるか</li>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|実行されるチェック|
|---|
|<ul><li>ユーザーがリージョンの Data Lake Store にデータをアップロードできるか</li><li>ユーザーがリージョンの Data Lake Store にデータをダウンロードできるか</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|実行されるチェック|
|---|
|<ul><li>DocumentDB サービスが利用できないために処理されていないデータベースまたはコレクションの要求があるか</li><li>DocumentDB サービスが利用できないために処理されていないドキュメントの要求があるか</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.network/connections
|実行されるチェック|
|---|
|<ul><li>VPN トンネルが接続されているか</li><li>接続に構成の競合があるか</li><li>事前共有キーは正しく構成されているか</li><li>VPN でオンプレミスのデバイスにアクセスできるか</li><li>IPSec/IKE セキュリティ ポリシーに不一致があるか</li><li>S2S VPN 接続が正しくプロビジョニングされているか、障害が発生した状態にあるか</li><li>VNET-to-VNET 接続が正しくプロビジョニングされているか、障害が発生した状態にあるか</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|実行されるチェック|
|---|
|<ul><li>インターネットから VPN Gateway にアクセスできるか</li><li>VPN Gateway がスタンバイ モードか</li><li>VPN サービスがゲートウェイで実行されているか</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|実行されるチェック|
|---|
|<ul><li> 名前空間で、登録、インストール、送信などのランタイム操作を実行できるか</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|実行されるチェック|
|---|
|<ul><li>ホスト OS が稼働しているか</li><li>データ センターの外部から workspaceCollection にアクセスできるか</li><li>PowerBI リソース プロバイダーを利用できるか</li><li>PowerBI サービスが適切な地域で利用できるか</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|実行されるチェック|
|---|
|<ul><li>クラスターで診断操作を実行できるか</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|実行されるチェック|
|---|
|<ul><li> データベースにログインしたことがあるか</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|実行されるチェック|
|---|
|<ul><li>ジョブが実行されるホストがすべて稼働しているか</li><li>ジョブを開始できなかったか</li><li>進行中のランタイムのアップグレードはあるか</li><li>ジョブが期待されている状態になっているか (たとえば、顧客によって実行中または停止されている)</li><li>ジョブでメモリ不足例外が発生しているか</li><li>スケジュールされたコンピューティングの進行中の更新はあるか</li><li>実行マネージャー (コントロール プラン) を利用できるか</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|実行されるチェック|
|---|
|<ul><li>ホスト サーバーが稼働しているか</li><li>インターネット インフォメーション サービスが実行されているか</li><li>ロード バランサーが実行されているか</li><li>データセンターから Web サービス プランにアクセスできるか</li><li>serverFarm のサイト コンテンツをホストしているストレージ アカウントを利用できるか</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/sites
|実行されるチェック|
|---|
|<ul><li>ホスト サーバーが稼働しているか</li><li>インターネット インフォメーション サーバーが実行されているか</li><li>ロード バランサーが実行されているか</li><li>データセンターから Web アプリにアクセスできるか</li><li>サイト コンテンツをホストしているストレージ アカウントを利用できるか</li></ul>|

Resource Health について詳しくは、次のリソースをご覧ください。
-  [Azure Resource Health の概要](Resource-health-overview.md)
-  [Azure Resource Health に関してよく寄せられる質問](Resource-health-faq.md)


