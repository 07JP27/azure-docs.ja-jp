---
title: インクルード ファイル
description: インクルード ファイル
services: iot-hub
author: wesmc7777
ms.service: iot-hub
ms.topic: include
ms.date: 06/19/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 94cd178b925b0b55f0ed6ed38ed821820bb1e072
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2019
ms.locfileid: "67181286"
---
次の推奨記事に進む場合は、既に作成したリソースを維持して、再利用することができます。

それ以外の場合は、課金されないようにするために、この記事で作成した Azure リソースを削除してもかまいません。 

> [!IMPORTANT]
> リソース グループを削除すると、元に戻すことができません。 リソース グループとそこに含まれるすべてのリソースは完全に削除されます。 間違ったリソース グループやリソースをうっかり削除しないようにしてください。 IoT ハブを、保持したいリソースが含まれている既存のリソース グループ内に作成した場合は、リソース グループを削除するのではなく、IoT Hub リソースだけを削除してください。
>

名前でリソース グループを削除するには、以下の手順を実行します。

1. [Azure ポータル](https://portal.azure.com) にサインインし、 **[リソース グループ]** をクリックします。

2. **[名前でフィルター処理してください]** ボックスに、IoT ハブが含まれているリソース グループの名前を入力します。 

3. 結果一覧のでリソース グループの **[...]** をクリックし、 **[リソース グループの削除]** をクリックします。

    ![削除](./media/iot-hub-quickstarts-clean-up-resources/iot-hub-delete-resource-group.png)

4. リソース グループの削除の確認を求めるメッセージが表示されます。 確認のためにリソース グループの名前を再度入力し、 **[削除]** をクリックします。 しばらくすると、リソース グループとそこに含まれているすべてのリソースが削除されます。