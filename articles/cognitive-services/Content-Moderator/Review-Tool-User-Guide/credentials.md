---
title: Azure Content Moderator の資格情報の管理 - Content Moderator
titlesuffix: Azure Cognitive Services
description: API を使用するために必要な Content Moderator 資格情報を管理します。
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 3d183870938f44cd71f1dab4105eec390ba12d94
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266147"
---
# <a name="manage-content-moderator-service-credentials"></a>Content Moderator サービスの資格情報を管理する

Content Moderator 資格情報は、次の場所で作成されます。

- [Azure ポータル](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Content Moderator レビュー ツール](https://contentmoderator.cognitive.microsoft.com/)

この記事では、それらがある場所と相互の関連について説明します。

## <a name="the-azure-portal"></a>Azure ポータル

Azure portal ダッシュボードで、Content Moderator アカウントを選択します。 **[リソース管理]** で、**[キー]** を選択します。 キーをコピーするには、キーの右側にあるアイコンを選択します。

![Azure portal での Content Moderator キー](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>レビュー ツールおよびレビュー API で Azure アカウントを使用する
レビュー API で Azure キーを使用するには、次のスクリーンショットの **[プロパティ]** 画面に表示されているリソース ID をコピーし、次の **[リソース ID]** セクションに表示されているようにレビュー ツールの資格情報画面で **[Whitelisted Resource Id(s)]\(ホワイト リソース ID\)** フィールドにそれを入力します。 

> [!NOTE]
> Content Moderator サブスクリプションのリージョンは、チームを認識し、チームのデータにアクセスするために、レビュー チームのリージョンと一致する必要があります。 たとえば、このページのイメージで、**West US** リージョン **(4)** には、Content Moderator Azure サブスクリプションとレビュー チームが含まれています。
>
> レビュー ツール内の 2 つの場所を Azure サブスクリプションのキーとリソース ID に置き換えた後、[資格情報] 画面に表示されている**試用版 Ocp-Apim-Subscription-Key** は使用されなくなりますが、いつでも使用できます。
> 試用版のキーでは、1 要求/秒 (RPS) で 1 か月あたり最大 5,000 トランザクションに制限されます。

![Azure portal での Content Moderator リソース ID](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>レビュー ツールでワークフローを使った Azure アカウントを使用します

Content Moderator 内で使用可能なワークフローに Azure キーを使用するには、次の **[ワークフロー]** セクションに示されているように、**[ワークフロー設定]** セクションの **[Ocp-Apim-Subscription-Key]** フィールドにそれを入力します。 **+** を押してリソース ID を保存します。

![レビュー ツールでの Content Moderator ワークフロー資格情報](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>レビュー ツール

レビュー ツールのダッシュボードの **[設定]** タブで、**[資格情報]** を選択します。

![レビュー ツールでの Content Moderator 資格情報](images/credentials-trial-resource-workflow.PNG)

次のセクションでは、前の画像をさらに詳しく調べます。

### <a name="api"></a>API

最初の部分には、レビュー チームの作成の一環として生成された**レビュー API エンドポイント**、**チーム ID**、および **Ocp-Apim-Subscription-Key (Content Moderator の試用キー)** が一覧表示されます。 これらは、レビュー API を含むすべての Content Moderator API の呼び出しに使用します。

また、API エンドポイントのリージョン識別子にも注意してください。 たとえば、**westus** は "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0" 内のリージョンです

![レビュー ツールでの Content Moderator キー](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Resource ID

フィールドのこのセットは、前のセクションの「[レビュー ツールおよびレビュー API で Azure アカウントを使用する](credentials.md#use-the-azure-account-with-the-review-tool-and-review-api)」で説明されています。 このフィールドは、前のセクションで説明したように、このフィールドに Azure リソース ID を追加する場合を除き、通常は空白です。

### <a name="workflows"></a>Workflows

フィールドのこのセットは、前のセクションの「[レビュー ツールでワークフローを使った Azure アカウントを使用します](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool)」で説明されています。 既定では、ワークフローを実行するために、レビュー ツールが自動生成された試用版キーを使用し、それがまず始めに表示されます。 他の 2 つのフィールドでは、それぞれ画面の文字と画像の評価の操作で用語と画像の一覧を使用できます。

## <a name="next-steps"></a>次の手順

* [ワークフロー](workflows.md)で Content Moderator 資格情報を使用する方法について説明します。
