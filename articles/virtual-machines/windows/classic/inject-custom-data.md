---
title: "Azure で Windows VM にデータを挿入する | Microsoft Docs"
description: "このトピックでは、Azure の仮想マシンのインスタンスを作成する際に、仮想マシンにカスタム データを挿入する方法のほか、Windows と Linux の場合について、カスタム データの場所を探して利用する方法について説明します。"
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 48759f76-eaa0-4202-ada0-706d3f9a9467
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/23/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 7836577f16940b618a2912012ba8a8e7160980e8
ms.lasthandoff: 03/31/2017


---
# <a name="injecting-custom-data-into-an-azure-virtual-machine"></a>Azure の仮想マシンにカスタム データを挿入する
> [!IMPORTANT] 
> Azure には、リソースの作成と操作に関して、 [Resource Manager とクラシック](../../../resource-manager-deployment-model.md)の 2 種類のデプロイメント モデルがあります。 この記事では、クラシック デプロイ モデルの使用方法について説明します。 最新のデプロイでは、リソース マネージャー モデルを使用することをお勧めします。 Resource Manager モデルを使用したカスタム スクリプト拡張機能の使用方法については、[こちら](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)をご覧ください。

[!INCLUDE [virtual-machines-common-classic-inject-custom-data](../../../../includes/virtual-machines-common-classic-inject-custom-data.md)]


