---
title: Azure Time Series Insights 環境のリテンション期間の構成方法 | Microsoft Docs
description: この記事では、Azure Time Series Insights 環境のリテンション期間の構成方法について説明します。
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: f5f34983d1818679450249aa40bbb325e743cc57
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239000"
---
# <a name="configuring-retention-in-time-series-insights"></a>Time Series Insights のリテンション期間の構成

この記事では、Azure Time Series Insights の**データ リテンション期間**と**ストレージ制限を超過したときの動作**の構成方法について説明します。

## <a name="summary"></a>まとめ

各 Time Series Insights (TSI) 環境には、**データ リテンション期間**を構成するための設定があります。 この値は 1 から 400 日間となっています。 環境の記憶域容量に達したとき、またはリテンション期間 (1 から 400 日) が終了したときのいずれか早い方でデータが削除されます。

TSI 環境ごとに、 **[ストレージ制限を超過したときの動作]** という追加設定があります。 この設定によって、環境の最大容量に到達したときのイングレスと消去の動作が制御されます。 次の 2 つの動作の選択肢があります。

- **[Purge old data]\(古いデータを消去\)** (既定値)
- **[Pause ingress]\(イングレスを一時停止\)**

これらの設定の詳細については、[Time Series Insights のリテンション期間](time-series-insights-concepts-retention.md)に関するページを参照してください。  

## <a name="configure-data-retention"></a>データ リテンション期間の構成

1. [Azure Portal](https://portal.azure.com) にサインインします。

1. 既存の Time Series Insights 環境を見つけます。 Azure Portal の左側のメニューにある **[すべてのリソース]** を選択します。 Time Series Insights 環境を選択します。

1. **[設定]** という見出しの下にある **[構成]** を選択します。

1. **[Data retention time]\(データ リテンション期間\)** を選択し、スライダー バーを使用するか、テキスト ボックスに数値を入力してリテンション期間を構成します。

1. **[容量]** の構成は、データ イベントの最大量やデータ格納用の記憶域の最大容量に影響するため、この設定をメモしておきます。

1. **[ストレージ制限を超過したときの動作]** を設定します。 **[Purge old data]\(古いデータを消去\)** と **[Pause ingress]\(イングレスを一時停止\)** のいずれかを選択します。

1. **[保存]** を選択して、変更した構成を保存します。

## <a name="next-steps"></a>次の手順

- 詳細については、[Time Series Insights のリテンション期間](time-series-insights-concepts-retention.md)に関するページを参照してください。