---
title: "Azure Portal で Linux VM の FQDN を作成する | Microsoft Docs"
description: "Azure Portal で仮想マシンに基づいて、Resource Manager の完全修飾ドメイン名 (FQDN) を作成する方法を説明します。"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: a672b0afbc622890843e056883cb437749d3cff9
ms.contentlocale: ja-jp
ms.lasthandoff: 07/06/2017


---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Linux VM 用の Azure Portal での完全修飾ドメイン名の作成

[Azure ポータル](https://portal.azure.com)で仮想マシン (VM) を作成すると、仮想マシン用のパブリック IP リソースが自動的に作成されます。 この IP アドレスを使用して、VM にリモートでアクセスします。 ポータルでは[完全修飾ドメイン名](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) は作成されませんが、VM の作成後に追加できます。 この記事では、DNS 名または FQDN を作成する手順を示します。

## <a name="create-fqdn"></a>FQDN を作成する
この記事では、既に VM が作成されていることを前提としています。 必要であれば、[ポータルで VM を作成する](quick-create-portal.md)か、[Azure CLI を使用して VM を作成する](quick-create-cli.md)ことができます。 VM が起動したら、次の手順を実行します。

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

`ssh azureuser@mydns.westus.cloudapp.azure.com`など、この DNS 名を使用して、VM にリモートで接続できるようになります。

## <a name="next-steps"></a>次のステップ
これで、VM がパブリック IP と DNS 名を持つようになったため、nginx、MongoDB、Docker などの、共通のアプリケーション フレームワークやサービスをデプロイできるようになりました。

Azure デプロイの構築に関するヒントについては、[Resource Manager の使用](../../azure-resource-manager/resource-group-overview.md)に関する記事から確認することもできます。


