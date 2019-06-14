---
title: Facebook に接続する - Azure Logic Apps | Microsoft Docs
description: Facebook REST API と Azure Logic Apps を使用して、タイムラインとページを管理します
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 25595127d913d3cd093e0af3d7916e33fc7cb352
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105977"
---
# <a name="get-started-with-the-facebook-connector"></a>Facebook コネクタの使用
Facebook に接続し、タイムラインへの投稿、ページ フィードの取得などを行います。 Facebook では、次の操作を実行できます。

* Facebook から取得したデータに基づいてビジネス フローを構築できます。 
* 新しい投稿を取得したときにトリガーを使用できます。
* タイムラインへの投稿、ページ フィードの取得などのアクションを使用できます。 また、これらのアクションで応答を取得すると、他のアクションから出力を使用できます。 たとえば、タイムラインに新しい投稿がある場合、その投稿を取得して、Twitter フィードにプッシュすることができます。 

まず、ロジック アプリを作成します。[ロジック アプリの作成](../logic-apps/quickstart-create-first-logic-app-workflow.md)に関する記事を参照してください。

## <a name="create-a-connection-to-facebook"></a>Facebook への接続を作成する
ロジック アプリにこのコネクタを追加するとき、Facebook に接続するロジック アプリを承認する必要があります。

1. Facebook アカウントにサインインします。
2. **[Authorize]** を選択し、ロジック アプリが Facebook に接続して使用することを許可します。 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a>コネクタ固有の詳細

[コネクタの詳細](/connectors/facebook/)に関するページに、Swagger で定義されているトリガーとアクション、さらに制限が記載されています。

## <a name="more-connectors"></a>その他のコネクタ
[API リスト](apis-list.md)に戻ります。