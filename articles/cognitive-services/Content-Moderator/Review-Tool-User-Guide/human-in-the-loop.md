---
title: コンテンツ モデレーションに目視レビューを組み込む - Content Moderator
titlesuffix: Azure Cognitive Services
description: マシンと人間が連携してコンテンツ モデレーションの最良の結果を提供する方法
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.date: 01/10/2019
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.author: sajagtap
ms.openlocfilehash: 7d2e87ac5353f103e654f4dcb6656916e4b271ae
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264735"
---
# <a name="learn-about-the-review-tool"></a>レビュー ツールについて学習する

コンテンツ モデレーションでは、人間とマシンが連携した場合に最良の結果が得られます。 マシンは、現実世界のコンテキスト内で予測の信頼性を支援または抑制する必要のある目視レビューを効果的に補強します。 結果は、人間またはマシンが単独で動作している場合よりも高いパフォーマンスを実現するハイブリッド コンテンツ モデレーション プロセスです。

## <a name="what-it-does"></a>実行内容

マシンによるモデレーション API と組み合わせて使用する場合、目視レビュー ツールでは、コンテンツ モデレーションのライフ サイクルに関連してこれらの重要なタスクを実行できます。

1. 基になるモデレーション API 結果からの目視レビューの作成を自動化します
2. 1 つのツール (レビュー ツールおよび API) を使用して、複数の形式 (テキスト、画像、および動画) をモデレートします
3. コンテンツ カテゴリまたはエクスペリエンス レベルによって編成された複数のレビュー チームにコンテンツ レビューを割り当てまたはエスカレートします。
4. 既定のワークフローを使用するか、柔軟なルールを持つカスタム ワークフローを定義し、コードは記述しません。
5. 単純にコネクタを作成することで、目視レビューを API またはビジネス プロセスに追加します。
6. 既定のコネクタを使用して、Microsoft PhotoDNA、テキスト分析、および Face API からの結果をレビューします。
7. コンテンツ モデレーション プロセスの主要なパフォーマンス メトリックを取得します。

![Content Moderator ビデオ レビュー ツール](../images/video-review-default-view.png)
