---
title: Azure Data Box Gateway (プレビュー) リリース ノート | Microsoft Docs
description: Azure Data Box Gateway の現行プレビュー リリースの重大な未解決の問題と解決策について説明します。
services: databox-edge-gateway
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 7619056ace5d9b3cf083a40a6cfa06a0cac0561e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949691"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Azure Data Box Gateway (プレビュー) リリース ノート

## <a name="overview"></a>概要

このリリース ノートには、Microsoft Azure Data Box Gateway プレビュー リリースに関する未解決の重大な問題と解決済みの問題が示されています。

リリース ノートは継続的に更新されます。対応策を必要とする重大な問題が見つかった場合は、それらの問題が追加されます。 Data Box Gateway をデプロイする前に、リリース ノートに含まれる情報を十分に確認してください。

プレビュー リリースは、**Data Box Gateway Preview Version 2.0** ソフトウェア バージョンに対応しています。

## <a name="issues-fixed-in-preview-release"></a>プレビュー リリースで修正された問題

次の表に、このリリースで修正された問題の概要を示します。

| いいえ。 | 問題 |
| --- | --- |
| 1 | このリリースでは、別のツール (AzCopy) によってアップロードされたファイルが最新の情報に更新された後で、ファイル サイズが増えるように更新されると、"*[Error 400: InvalidBlobOrBlock (The specified blob or block content is invalid.)]\(Error 400: InvalidBlobOrBlock (指定された BLOB またはブロック コンテンツが無効です。)\)*" エラーが検出されます。|


## <a name="known-issues-in-preview-release"></a>プレビュー リリースの既知の問題

次の表に、Data Box Gateway 現行プレビュー リリースの既知の問題の概要を示します。

| いいえ。 | Feature | 問題 | 対応策/コメント |
| --- | --- | --- | --- |
| **1.** |更新プログラム |以前のプレビュー リリースで作成された Data Box Gateway デバイスをこのバージョンに更新することはできません。 |新しいリリースから仮想ディスク イメージをダウンロードし、新しいデバイスを構成してデプロイします。 詳しくは、[Azure Data Box Gateway のデプロイ準備](data-box-gateway-deploy-prep.md)に関する記事をご覧ください。 |
| **2.** |プロビジョニング済みのデータ ディスク |特定の指定されたサイズのデータ ディスクをプロビジョニングし、対応する Data Box Gateway を作成した後は、データ ディスクを縮小することはできません。 ディスクを縮小しようとすると、デバイスのローカル データすべてが失われます。 | |
| **3.** |更新 |このリリースでは、一度に 1 つの共有しか最新の情報に更新できません。 | |
| **4.** |名前の変更 |オブジェクト名の変更はサポートされていません。 |この機能がワークフローで重要な場合は、Microsoft サポートにお問い合わせください。 |
| **5.** |コピー| 読み取り専用ファイルがデバイスにコピーされた場合に、読み取り専用プロパティが保持されません。 | |
| **6.** |ログ| このリリースのバグのため、*error.xml* に、エラー コード 110 のインスタンスが認識できない項目名と一緒に含まれることがあります。 | |
| **7.** |アップロード | このリリースのバグのため、特定のファイルのアップロード中にエラー コード 2003 のインスタンスが検出されることがあります。 | |
| **8.** |削除 | このリリースのバグのため、NFS 共有が削除される場合に共有を削除できないことがあります。 共有の状態として *Deleting* が表示されます。  |これが発生するのは、サポートされないファイル名を共有が使用している場合のみです。 |
| **9.** |オンライン ヘルプ |Azure portal の [ヘルプ] リンクがドキュメントにリンクしないことがあります。|[ヘルプ] リンクは一般公開リリースでは機能します。 |



## <a name="next-steps"></a>次の手順

- [Azure Data Box Gateway のデプロイを準備](data-box-gateway-deploy-prep.md)します。


