---
title: ビデオ内の正確な場面の検出 - Video Indexer
titlesuffix: Azure Cognitive Services
description: このトピックでは、Video Indexer を使用して、ビデオ内の正確な場面を見つける方法を示します。
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: juliako
ms.openlocfilehash: 9806f9c3f22429d61ac765773e4b6971d683a6b1
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984510"
---
# <a name="find-exact-moments-within-videos"></a>ビデオ内の正確な場面の検出

このトピックでは、ビデオ内の正確な場面を見つけるための検索オプションを示します。

1. [Video Indexer](https://www.videoindexer.ai/) Web サイトに移動してサインインします。
2. アカウントのすべてのビデオから検索します。

    次の例では、Channel9 によって作成された Scott Hanselman が出演しているビデオをすべて検索しました。

    ![Search](./media/video-indexer-search/video-indexer-search01.png)
    
3. ビデオの要約された分析情報を検索します。

    ビデオの **[再生]** をクリックして、ビデオ内を検索します。 次に、**[検索]** タブを選択して、ビデオ内を検索します。たとえば、ここでは "ID 保護" というテキストが使用されている場面をすべて検索しました。 

    ![Search](./media/video-indexer-search/video-indexer-search02.png)

    結果の 1 つをクリックすると、ビデオのその場面がプレーヤーで表示されます。 アプリケーション内で、プレーヤー/分析情報の表示と同期を実現できます。 詳細については、「[アプリケーションに Video Indexer ウィジェットを埋め込む](video-indexer-embed-widgets.md)」を参照してください。 

    
4. ビデオの詳細な内訳を検索します。

    見つけたビデオに基づいて独自の内訳を作成する場合は、**[編集]** ボタンを押します。 このページにはビデオの完全な内訳が表示されます。 内訳内を検索して、関心のある行のみを表示することができます。 詳細については、「[Video Indexer の分析情報を表示および編集する](video-indexer-view-edit.md)」を参照してください。

    この例では、"ID 保護" というテキストを検索しました。 また、下の画面に示すように追加のフィルターも適用しました。

    ![Search](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>次の手順 

処理したいビデオを見つけたら、次のいずれかのトピックの説明に従ってビデオの処理を続行できます。 

- [既存のビデオに基づいて新しいビデオの分析情報を作成する](video-indexer-create-new.md)
- [Video Indexer REST API を使用してコンテンツを処理する](video-indexer-use-apis.md)
- [アプリケーションにビジュアル ウィジェットを埋め込む](video-indexer-embed-widgets.md)

## <a name="see-also"></a>関連項目

[Video Indexer の概要](video-indexer-overview.md)
