---
title: Azure CLI のサンプル - データ ディスクを接続して使用する | Microsoft Docs
description: Azure CLI のサンプル
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f7acfb068d000eb76ea526ea7fb3244c585beebf
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887667"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-the-azure-cli"></a>Azure CLI を使用して、仮想マシン スケール セットがあるデータ ディスクを接続して使用する
このスクリプトでは、仮想マシン スケール セットを作成し、データ ディスクを接続して準備します。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>サンプル スクリプト
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.sh "Create a virtual machine scale set with data disks")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ
次のコマンドを実行して、リソース グループ、スケール セット、すべての関連リソースを削除します。

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>スクリプトの説明
このスクリプトでは、次のコマンドを使用して、リソース グループ、仮想マシン スケール セット、およびすべての関連リソースを作成します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | すべてのリソースを格納するリソース グループを作成します。 |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | 仮想マシン スケール セットを作成し、仮想ネットワーク、サブネット、およびネットワーク セキュリティ グループに接続します。 複数の VM インスタンスにトラフィックを分散するために、ロード バランサーも作成されます。 このコマンドでは、使用する VM イメージと管理者の資格情報も指定します。  |
| [az vmss disk attach](/cli/azure/vmss/disk#az_vmss_disk_attach) | データ ディスクを作成し、仮想マシン スケール セットに接続します。 |
| [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) | Azure カスタム スクリプト拡張機能をインストールして、各 VM インスタンス上にデータ ディスクを準備するスクリプトを実行します。 |
| [az group delete](/cli/azure/ad/group#delete) | 入れ子になったリソースすべてを含むリソース グループを削除します。 |

## <a name="next-steps"></a>次の手順
Azure CLI の詳細については、[Azure CLI のドキュメント](https://docs.microsoft.com/cli/azure/overview)のページをご覧ください。

その他の仮想マシン スケール セット用の Azure CLI サンプル スクリプトは、[Azure 仮想マシン スケール セットのドキュメント](../cli-samples.md)にあります。