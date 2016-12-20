---
title: "Linux 仮想マシンにタグを付ける方法 | Microsoft Docs"
description: "リソース マネージャーのデプロイメント モデルを使用して Azure で作成した Linux 仮想マシンのタグ付けについて説明します。"
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 6e39b13de1808ebb1d0571ab0c1c620261046d0d


---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Azure で Linux 仮想マシンにタグを付ける方法
この記事では、Azure で Resource Manager デプロイ モデルを通して Linux 仮想マシンにタグを付けるさまざまな方法について説明します。 タグはユーザー定義のキーと値ペアです。リソースまたはリソース グループに直接設定できます。 現在、Azure では、1 つのリソースまたはリソース グループにつき最大 15 個のタグを付けることができます。 タグは、リソースの作成時に付けたり、既存のリソースに追加したりすることができます。 タグは、Resource Manager デプロイ モデル経由で作成されたリソースでのみサポートされます。

[!INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Azure CLI を使用してタグを付ける
最初に、[Azure CLI をインストールおよび構成](../xplat-cli-azure-resource-manager.md)し、Resource Manager モードであることを確認します (`azure config mode arm`)。

このコマンドを使用すると、タグを含め、指定した仮想マシンのすべてのプロパティを表示できます。

        azure vm show -g MyResourceGroup -n MyTestVM

Azure CLI を使用して新しい VM タグを追加するには、タグ パラメーター **-t** を指定して `azure vm set` コマンドを実行できます。

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

すべてのタグを削除するには、`azure vm set` コマンドで **–T** パラメーターを使用できます。

        azure vm set – g MyResourceGroup –n MyTestVM -T


ここでは、Azure CLI およびポータルを使用してリソースにタグを適用しました。次は、課金ポータルの使用量の詳細でタグを確認してみましょう。

[!INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>次のステップ
* Azure リソースへのタグ付けについて詳しくは、「[Azure Resource Manager の概要]」「[Azure Resource Manager の概要]」と「[タグを使用した Azure リソースの整理]」「[タグを使用した Azure リソースの整理]」をご覧ください。
* タグが Azure リソースの使用の管理にどのように役立つかを確認するには、[Azure の課金内容の確認][Azure の課金内容の確認]に関するページと「[Microsoft Azure リソースの消費を把握する]」「[Microsoft Azure リソースの消費を把握する]」をご覧ください。

[Azure CLI 環境]: ./xplat-cli-azure-resource-manager.md
[Azure Resource Manager の概要]: ../azure-resource-manager/resource-group-overview.md
[タグを使用した Azure リソースの整理]: ../resource-group-using-tags.md
[Azure の課金内容の確認]: ../billing/billing-understand-your-bill.md
[Microsoft Azure リソースの消費を把握する]: ../billing-usage-rate-card-overview.md



<!--HONumber=Nov16_HO3-->


