---
title: BLEU スコアとは - Custom Translator
titleSuffix: Azure Cognitive Services
description: BLEU は、同じ原文の自動翻訳と 1 つまたは複数の人間が作成した参考翻訳との違いの測定単位です。 BLEU アルゴリズムでは、自動翻訳の連続するフレーズと、参考翻訳に含まれる連続するフレーズとが比較され、一致数がカウントされ、重み付けされた形式で表示されます。
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.topic: article
ms.date: 11/13/2018
ms.author: v-rada
ms.openlocfilehash: b0fd9777b8c830a06195dbc22f0bb9081ff9753a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222016"
---
# <a name="what-is-a-bleu-score"></a>BLEU スコアとは

[BLEU (Bilingual Evaluation Understudy)](https://en.wikipedia.org/wiki/BLEU) は、同じ原文の自動翻訳と 1 つまたは複数の人間が作成した参考翻訳との違いの測定単位です。

## <a name="scoring-process"></a>スコアリング プロセス

BLEU アルゴリズムでは、自動翻訳の連続するフレーズと、参考翻訳に含まれる連続するフレーズとが比較され、一致数がカウントされ、重み付けされた形式で表示されます。 これらの一致は、位置とは独立しています。 一致度が高くなるほど、参考翻訳との類似度が高くなり、スコアも高くなります。 明瞭さと文法上の正確さは考慮されていません。

## <a name="how-bleu-works"></a>BLEU のしくみ

BLEU の長所は、すべての文に対して正確な人間の判断を下そうと試みるのではなく、テスト コーパス上の個々の文の誤判断を平均化することによって、人間の判断との相関性が高くなることです。

BLEU スコアの詳細なディスカッションについては、[こちら](https://youtu.be/-UqDljMymMg)を参照してください。

BLEU の結果は、ドメインの広さ、テスト データとトレーニングおよびチューニング データとの一貫性、トレーニングに使用できるデータの量によって大きく左右されます。 狭いドメインに対してモデルがトレーニングされ、トレーニング データとテスト データとの一貫性がある場合は、BLEU スコアが高くなることが予想されます。

>[!NOTE]
>BLEU スコア間の比較は、BLEU 結果が同じテスト セット、同じ言語ペア、および同じ MT エンジンと比較された場合にのみ、正当と認められます。 異なるテスト セットの BLEU スコアは、別と見なす必要があります。
