---
title: Microsoft Azure Data Box Gateway デバイスのアクセス、電源、接続モード | Microsoft Docs
description: Azure へのデータ転送に役立つ Azure Data Box Gateway デバイスのアクセス、電源、および接続モードを管理する方法について説明します
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: 0ad94799320e25d88f616117f1bfcf9f0513aadf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873021"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway-preview"></a>Azure Data Box Gateway (プレビュー) のアクセス、電源、接続モードを管理する

この記事では、Azure Data Box Gateway のアクセス、電源、および接続モードを管理する方法について説明します。 これらの操作は、ローカル Web UI または Azure portal を使用して実行されます。

この記事では、次のことについて説明します。

> [!div class="checklist"]
> * デバイスのアクセスを管理する
> * 接続モードを管理する
> * 電源を管理する

> [!IMPORTANT]
> Data Box Gateway はプレビュー段階にあります。 このソリューションを注文して展開する前に、[Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)を確認してください。

## <a name="manage-device-access"></a>デバイスのアクセスを管理する

Data Box Gateway デバイスへのアクセスは、デバイス管理者のパスワードを使用して制御されます。 管理者のパスワードは、ローカル Web UI を使用して変更できます。 また、Azure portal ではデバイス管理者のパスワードをリセットすることもできます。

### <a name="change-device-administrator-password"></a>デバイス管理者のパスワードを変更する

デバイス管理者のパスワードを変更するには、ローカル UI で次の手順のようにします。

1. ローカル Web UI で、**[メンテナンス] > [Password change]\(パスワード変更\)** に移動します。
2. 現在のパスワードを入力し、新しいパスワードを入力ます。 指定するパスワードは 8 ～ 16 文字にする必要があります。 パスワードには、大文字、小文字、数字、および特殊文字のうち 3 種類の文字を使用する必要があります。 新しいパスワードを確認入力します。

    ![パスワードの変更](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. **[パスワードの変更]** をクリックします。
 
### <a name="reset-device-administrator-password"></a>デバイス管理者のパスワードをリセットする

リセット ワークフローでは、ユーザーは古いパスワードを思い出す必要がなく、パスワードを忘れた場合に便利です。 このワークフローは、Azure portal で実行します。

1. Azure portal で、**[概要] > [管理パスワードのリセット]** に移動します。

    ![[パスワードのリセット]](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. 新しいパスワードを入力し、それを確認します。 指定するパスワードは 8 ～ 16 文字にする必要があります。 パスワードには、大文字、小文字、数字、および特殊文字のうち 3 種類の文字を使用する必要があります。 **[リセット]** をクリックします。

    ![[パスワードのリセット]](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>接続モードを管理する

既定の標準モードとは別に、デバイスは部分切断モードまたは切断モードで動作することもできます。 ここでは、これらの各モードについて説明します。

- **部分切断** – このモードでは、デバイスはどのようなデータも共有にアップロードできませんが、Azure portal からデバイスを管理することはできます。

    このモードは通常、従量制課金のサテライト ネットワークで使用され、目的はネットワーク帯域幅の消費量を最小限に抑えることです。 デバイス監視操作のため、最小限のネットワーク消費が発生する可能性があります。

- **切断** – このモードでは、デバイスはクラウドから完全に切断されており、クラウドのアップロードとダウンロードの両方が無効になっています。 デバイスは、ローカル Web UI からのみ管理できます。

    このモードは通常、デバイスをオフラインにするときに使用されます。

デバイスのモードを変更するには、次の手順のようにします。

1. デバイスのローカル Web UI で、**[構成] > [クラウドの設定]** に移動します。
2. **[Cloud upload and download]\(クラウドのアップロードとダウンロード\)** を無効にします。
3. デバイスを部分切断モードで実行するには、**[Azure portal management]\(Azure portal 管理\)** を有効にします。

    ![接続モード](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. デバイスを切断モードで実行するには、**[Azure portal management]\(Azure portal 管理\)** を無効にします。 デバイスは、ローカル Web UI からのみ管理できるようになります。

    ![接続モード](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>電源を管理する

ローカル Web UI を使用して、物理デバイスと仮想デバイスをシャットダウンおよび再起動できます。 再起動する前に、ホストの共有をオフラインにしてから、デバイスをオフラインにすることをお勧めします。 この操作により、データ破損の可能性を最小限に抑えられます。

1. ローカル Web UI で、**[メンテナンス] > [Power settings]\(電源設定\)** に移動します。
2. 何を行うかに応じて、**[Shutdown]\(シャットダウン\)** または **[Restart]\(再起動\)** をクリックします。

    ![電源設定](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. 確認を求められたら、**[はい]** をクリックして続行します。

> [!NOTE]
> 仮想デバイスをシャットダウンした場合は、ハイパーバイザーの管理からデバイスを起動する必要があります。
