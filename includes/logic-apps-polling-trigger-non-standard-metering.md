---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/09/2018
ms.author: estfan
ms.openlocfilehash: 3fa71085d649ace95aa24ac87c8714a7268f5386
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2019
ms.locfileid: "67181768"
---
より正確な使用コストを見積もるには、ポーリング間隔にのみ基づいて計算するのではなく、それぞれの日に到着する可能性のあるメッセージまたはイベントの数を考慮します。 イベントまたはメッセージがトリガー条件を満たしている場合、多くのトリガーは、条件を満たす任意のすべての他の待機イベントまたはメッセージを直ちに読み取ろうとします。 この動作は、長いポーリング間隔を選択した場合でも、ワークフロー開始の対象となる待機イベントまたはメッセージの数に基づいてトリガーが起動することを意味します。 この動作に続くトリガーには、Azure Service Bus、Azure Event Hub などがあります。

したがって、たとえば、毎日エンドポイントをチェックするトリガーを設定したとします。 トリガーはエンドポイントを確認し、条件を満たす 15 のイベントを検出した場合、トリガーが起動し、対応するワークフローを 15 回実行します。 Logic Apps によって、これらの 15 のワークフローによって実行されるすべてのアクションを、ワークフローを実行するすべてのアクションが測定されます。 潜在的なコストを計算するには、[Azure 料金計算ツール](https://azure.microsoft.com/pricing/calculator/)をお試しください。