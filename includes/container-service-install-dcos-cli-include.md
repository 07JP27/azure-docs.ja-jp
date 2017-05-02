---
title: "DC/OS CLI のインストール |Microsoft Docs"
description: "DC/OS CLI をインストールします。"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "コンテナー, マクロサービス, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: e664ce9426a2852a35dfdade5d41a9ce8b37a3b7
ms.openlocfilehash: a8ea47f158c0d666340815d2e039995c7483257f


---
> [!NOTE]
> これは、DC/OS ベースの ACS クラスターを操作することを目的としています。 Swarm ベースの ACS クラスターには、行う必要はありません。
> 
> 

まず、 [DC/OS ベースの ACS クラスターに接続](../articles/container-service/container-service-connect.md)します。 この作業を終了したら、次のコマンドを使用して、クライアント コンピューターに DC/OS CLI をインストールできます。

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

以前のバージョンの Python を使用している場合は、いくつかの "InsecurePlatformWarnings" が表示される場合があります。 これらは無視してかまいません。

シェルを再起動せずに開始できるように、次のコマンドを実行します。

```bash
source ~/.bashrc
```

この手順は、新しいシェルを起動する場合は必要ありません。

これで、CLI がインストールされていることを確認できるようになりました。

```bash
dcos --help
```
