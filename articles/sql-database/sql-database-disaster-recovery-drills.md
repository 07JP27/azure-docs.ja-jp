---
title: SQL Database の障害復旧訓練 | Microsoft Docs
description: Azure SQL Database を使用してディザスター リカバリーの演習を実行するためのガイダンスとベスト プラクティスについて説明します。
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: c861163670b05b01c9c6d64b81f6e83c979a2af8
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163037"
---
# <a name="performing-disaster-recovery-drill"></a>障害復旧訓練の実行
復旧ワークフローのためのアプリケーションの対応状況の検証は、定期的に実行することをお勧めします。 アプリケーションの動作、データの損失の影響やフェールオーバーによる中断を検証することをお勧めします。 これは、ビジネス継続性の証明として、ほとんどの業界標準で必要条件ともなっています。

ディザスター リカバリーの訓練は以下のもので構成されています。

* データ層の停止シミュレーション
* 復旧
* 復旧後のアプリケーションの整合性の検証

[ビジネス継続性のためにアプリケーションをどのように設計](sql-database-business-continuity.md)したかによって、実行する訓練のワークフローは異なります。 この記事では、Azure SQL Database のコンテキストでディザスター リカバリーの訓練を行う際のベスト プラクティスについて説明します。

## <a name="geo-restore"></a>geo リストア
ディザスター リカバリーの訓練を実施する際のデータ損失の可能性を回避するために、運用環境のコピーから作成したテスト環境を使用して訓練を実行し、アプリケーションのフェールオーバーのワークフローの確認にもそれを使用することをお勧めします。

#### <a name="outage-simulation"></a>障害のシミュレーション
障害をシミュレートするために、ソース データベースの名前を変更できます。 これによって、アプリケーションの接続エラーが発生します。

#### <a name="recovery"></a>復旧
* [こちら](sql-database-disaster-recovery.md)の説明に従って、データベースの geo リストアを別のサーバーに実行します。
* アプリケーションの構成を変更して、復旧したデータベースに接続し、「[復旧後のデータベースの構成](sql-database-disaster-recovery.md)」のガイドに従って、復旧を完了します。

#### <a name="validation"></a>検証
* 復旧後に、アプリケーションの整合性を検証して訓練を完了します (接続文字列、ログイン、基本的な機能のテスト、その他の標準的なアプリケーションのサインオフのプロシージャの検証など)。

## <a name="failover-groups"></a>フェールオーバー グループ
フェールオーバー グループを使用して保護されたデータベースの場合、訓練には、セカンダリ サーバーへの計画されたフェールオーバーが含まれます。 計画されたフェールオーバーでは、ロールが切り替わったときに、フェールオーバー グループにあるプライマリ データベースとセカンダリ データベースの同期を維持するようにします。 計画外のフェールオーバーとは異なり、この操作ではデータは失われないため、訓練を運用環境で実行できます。

#### <a name="outage-simulation"></a>障害のシミュレーション
障害をシミュレートするために、データベースに接続されている Web アプリケーションまたは仮想マシンを無効にできます。 これにより、Web クライアントの接続エラーが発生します。

#### <a name="recovery"></a>復旧
* ディザスター リカバリー リージョンのアプリケーション構成が (完全にアクセス可能な新しいプライマリになる) 前のセカンダリをポイントしていることを確認します。
* セカンダリ サーバーから、フェールオーバー グループの[計画されたフェールオーバー](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)を開始します。
* 「 [復旧後のデータベースの構成](sql-database-disaster-recovery.md) 」のガイドに従って、復旧を完了します。

#### <a name="validation"></a>検証
復旧後に、アプリケーションの整合性を検証して訓練を完了します (接続性、基本的な機能のテスト、訓練のサインオフに必要なその他の検証など)。

## <a name="next-steps"></a>次の手順
* ビジネス継続性の設計および復旧シナリオについては、[継続性のシナリオ](sql-database-business-continuity.md)を参照してください。
* Azure SQL Database 自動バックアップの詳細については、[SQL Database の自動バックアップ](sql-database-automated-backups.md)を参照してください。
* 自動バックアップを使用して復旧する方法については、 [サービス主導のバックアップからのデータベース復元](sql-database-recovery-using-backups.md)に関するページをご覧ください
* より迅速な復旧オプションについては、[アクティブ geo レプリケーションとフェールオーバー グループ](sql-database-geo-replication-overview.md)に関する記事を参照してください。  
