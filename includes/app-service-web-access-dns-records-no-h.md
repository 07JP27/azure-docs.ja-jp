---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: d001d76bca5b9a0837349b6e05b3b0a271ea3a73
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66136290"
---
> [!NOTE]
> Azure DNS を使用して、Azure Web Apps のカスタム DNS 名を構成できます。 詳細については、「[Azure DNS を使用して Azure サービス用のカスタム ドメイン設定を提供する](../articles/dns/dns-custom-domain.md#app-service-web-apps)」をご覧ください。
>
>

ドメイン プロバイダーの Web サイトにサインインします。

DNS レコードの管理ページを探します。 各ドメイン プロバイダーには独自の DNS レコード インターフェイスがあるので、プロバイダーのドキュメントを参照してください。 **[ドメイン名]**、**[DNS]**、**[ネーム サーバー管理]** というラベルが付いたサイトの領域を探します。 

通常、DNS レコードのページを見つけるには、アカウント情報を表示し、**[ドメイン]** などのリンクを探します。 そのページに移動し、**[ゾーン ファイル]**、**[DNS レコード]**、**[詳細構成]** のような名前のリンクを探します。

以下のスクリーンショットは、DNS レコード ページの例です。

![DNS レコード ページの例](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

このスクリーンショットの例では、**[追加]** を選択してレコードを作成します。 プロバイダーによっては、追加するレコード タイプごとに異なるリンクが用意されています。 この場合も、プロバイダーのドキュメントを参照してください。

> [!NOTE]
> 一部のプロバイダー (GoDaddy など) では、別の **[変更を保存]** リンクを選択するまで DNS レコードの変更が反映されません。 
