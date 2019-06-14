---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 96cba4e077be8b7658c270b09b177a845e16c8b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "66169975"
---
この記事では、以下のことを前提としています。

1. オンプレミスのネットワークと Azure Virtual Network の間に**サイト間 VPN** または **ExpressRoute** 接続が既に確立されている。
2. 仮想マシンがフェールオーバーした Azure サブスクリプションに新しい仮想マシンを作成するアクセス許可をユーザー アカウントが持っている。
3. サブスクリプションでは、新しいプロセス サーバー仮想マシンの起動に少なくとも 4 つのコアを使用できる。
4. 使用可能な**構成サーバーのパスフレーズ**を持っている。

> [!TIP]
> 仮想マシンがフェールオーバーした Azure Virtual Network から (オンプレミスで実行されている) 構成サーバーのポート 443 に接続できることを確認します。
