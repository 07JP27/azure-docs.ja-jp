---
title: インクルード ファイル
description: インクルード ファイル
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8f511eafda70fca997a08b5907e606e572b8e219
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151151"
---
## <a name="copy-your-credentials-from-the-azure-portal"></a>Azure Portal で資格情報をコピーする

サンプル アプリケーションは、ストレージ アカウントへのアクセスを認証する必要があります。 認証するには、ストレージ アカウントの資格情報を接続文字列としてアプリケーションに追加します。 次の手順に従って、ストレージ アカウントの資格情報を表示します。

1. [Azure Portal](https://portal.azure.com) に移動します。
2. 自分のストレージ アカウントを探します。
3. ストレージ アカウントの概要の **[設定]** セクションで、**[アクセス キー]** を選択します。 ここでは、アカウント アクセス キーと各キーの完全な接続文字列を表示できます。   
4. **[Key1]** の **[接続文字列]** の値を見つけ、**[コピー]** ボタンを選択して接続文字列をコピーします。 すぐ後の手順で、接続文字列の値を環境変数に追加します。

    ![Azure portal から接続文字列をコピーする方法を示すスクリーンショット](media/storage-copy-connection-string-portal/portal-connection-string.png)
