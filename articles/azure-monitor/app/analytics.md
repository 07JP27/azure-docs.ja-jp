---
title: Analytics の使用 - Azure Application Insights の強力な検索およびクエリ　ツール | Microsoft Docs
description: 'Application Insights の強力な診断検索ツールである Analytics の概要。 '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 24a911978d6c9f9a33c81254a5e2f5951029ff3a
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120888"
---
# <a name="analytics-in-application-insights"></a>Application Insights の Analytics
Analytics は、[Application Insights](../../azure-monitor/app/app-insights-overview.md) の強力な検索およびクエリのツールです。 Analytics は Web ツールであるため、セットアップは不要です。 Application Insights をお使いのアプリの一つで構成済みの場合、アプリの [[概要]](../../azure-monitor/app/app-insights-dashboards.md) ブレードで Analytics を開いてアプリのデータを分析できます。

![portal.azure.com で Application Insights リソースを開き、[Analytics] をクリックします。](./media/analytics/001.png)

無料のデモ環境である [Analytics プレイグラウンド](https://go.microsoft.com/fwlink/?linkid=859557)を使って、多くのサンプル データを利用することもできます。
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Analytics でのデータ クエリ
一般的なクエリではテーブル名を最初に記述し、その後に一連の "*演算子*" を `|` で区切って続けます。
例として、直近の 3 時間のうちにアプリが受信したさまざまな国からの要求の回数を確認してみます。
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

最初にテーブル名 *requests* を記述し、必要に応じてパイプでつないだ要素を追加します。  まずは時間のフィルターを定義して、直近の3時間のレコードだけを確認するようにします。
次に国ごとのレコード数 (該当のデータは *client_CountryOrRegion* 列で確認できます) をカウントします。 最後に結果を円グラフで表示します。
<br>

![Query results](./media/analytics/030.png)

言語には、以下のような多くの魅力的な機能があります。

* [フィルター](/azure/kusto/query/whereoperator)。カスタム プロパティとメトリックを含む、すべてのフィールドで未加工のアプリ テレメトリをフィルター処理します。
* [結合](/azure/kusto/query/joinoperator)。複数のテーブルを結合します。要求をページ ビュー、依存関係の呼び出し、例外およびログ トレースに関連付けます。
* 強力な統計 [集計](/azure/kusto/query/summarizeoperator)。
* 迅速かつ強力な視覚化。
* PowerShell などのプログラムによるクエリの実行に使用できる [REST API](https://dev.applicationinsights.io/)。

[言語の完全なリファレンス](https://go.microsoft.com/fwlink/?linkid=856079)ではサポートされているすべてのコマンドを詳しく説明しており、定期的に情報を更新しています。

## <a name="next-steps"></a>次の手順
* [Analytics ポータル](https://go.microsoft.com/fwlink/?linkid=856587)の概要
* [クエリ作成](https://go.microsoft.com/fwlink/?linkid=856078)の概要
* もっとも一般的なイディオムの変換については [SQL ユーザー用の早見表](https://aka.ms/sql-analytics)をご確認ください。
* [プレイグラウンド](https://analytics.applicationinsights.io/demo)で Analytics を試す (ご使用のアプリからまだ Application Insights にデータが送信されていない場合)。
* [紹介ビデオ](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)を見る。
