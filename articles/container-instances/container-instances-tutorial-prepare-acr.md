---
title: "Azure Container Instances チュートリアル - Azure Container Registry の準備 | Microsoft Docs"
description: "Azure Container Instances チュートリアル - Azure Container Registry の準備"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, コンテナー, マイクロサービス, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: seanmck
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 5f3fc5f3624cf1ef881adf2af0cb69ad67d09ad3
ms.contentlocale: ja-jp
ms.lasthandoff: 07/26/2017

---

# <a name="deploy-and-use-azure-container-registry"></a>Azure Container Registry をデプロイして使用する

これは 3 つのパートで構成されるチュートリアルの 2 番目のタスクです。 [前のステップ](./container-instances-tutorial-prepare-app.md)では、[Node.js](http://nodejs.org) で記述されたシンプルな Web アプリケーションに対して、コンテナー イメージが作成されました。 このチュートリアルでは、このイメージを Azure Container Registry にプッシュします。 コンテナー イメージを作成していない場合、[チュートリアル 1 - コンテナー イメージの作成](./container-instances-tutorial-prepare-app.md)に関するページに戻ってください。 

Azure Container Registry は、Docker コンテナー イメージ用の Azure ベースのプライベート レジストリです。 このチュートリアルでは、Azure Container Registry インスタンスのデプロイ、およびこのインスタンスへのコンテナー イメージのプッシュについて説明します。 手順は次のとおりです。

> [!div class="checklist"]
> * Azure Container Registry インスタンスのデプロイ
> * Azure Container Registry のコンテナー イメージのタグ付け
> * Azure Container Registry へのイメージのアップロード

以降のチュートリアルでは、コンテナーをプライベート レジストリから Azure Container Instances にデプロイします。

## <a name="before-you-begin"></a>開始する前に

このチュートリアルでは、Azure CLI バージョン 2.0.4 以降を実行している必要があります。 バージョンを確認するには、`az --version` を実行します。 インストールまたはアップグレードする必要がある場合は、「[Azure CLI 2.0 のインストール]( /cli/azure/install-azure-cli)」を参照してください。

## <a name="deploy-azure-container-registry"></a>Azure Container Registry のデプロイ

Azure Container Registry をデプロイする場合、まず、リソース グループが必要です。 Azure リソース グループとは、Azure リソースのデプロイと管理に使用する論理コレクションです。

[az group create](/cli/azure/group#create) コマンドでリソース グループを作成します。 この例では、*myResourceGroup* という名前のリソース グループが *eastus* リージョンに作成されます。

```azurecli
az group create --name myResourceGroup --location eastus
```

[az acr create](/cli/azure/acr#create) コマンドで Azure Container Registry を作成します。 コンテナー レジストリの名前は**一意でなければなりません**。 次の例では、*mycontainerregistry082* という名前を使用します。

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

このチュートリアルの残りの部分では、選択したコンテナー レジストリ名のプレースホルダーとして `<acrname>` を使用します。

## <a name="get-azure-container-registry-information"></a>Azure Container Registry 情報の取得

コンテナー レジストリが作成されると、そのログイン サーバーとパスワードにクエリを実行できます。 次のコードでは、こうした値が返されます。 それぞれの値をメモします。各値はこのチュートリアルを通じて参照されます。

コンテナー レジストリ ログイン サーバー (レジストリ名で更新):

```azurecli
az acr show --name <acrName> --query loginServer
```

このチュートリアルの残りの部分では、コンテナー レジストリ ログイン サーバーの値のプレースホルダーとして `<acrLoginServer>` を使用します。

コンテナー レジストリ パスワード:

```azurecli
az acr credential show --name <acrName> --query passwords[0].value
```

このチュートリアルの残りの部分では、コンテナー レジストリ パスワードの値のプレースホルダーとして `<acrPassword>` を使用します。

## <a name="login-to-the-container-registry"></a>コンテナー レジストリへのログイン

イメージをプッシュする前に、コンテナー レジストリ インスタンスにログインする必要があります。 [docker login](https://docs.docker.com/engine/reference/commandline/login/) コマンドを使用して、操作を完了します。 docker login を実行する場合は、レジストリ ログイン サーバー名と資格情報を入力する必要があります。

```bash
docker login --username=<acrName> --password=<acrPassword> <acrLoginServer>
```

このコマンドは、完了すると 'Login Succeeded’(ログインに成功しました) というメッセージを返します。

## <a name="tag-container-image"></a>コンテナー イメージのタグ付け

プライベート レジストリからコンテナー イメージをデプロイするには、レジストリの `loginServer` 名でイメージにタグを付ける必要があります。

現在のイメージの一覧を表示するには、`docker images` コマンドを使用します。

```bash
docker images
```

出力:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

コンテナー レジストリの loginServer で *aci-tutorial-app* イメージにタグを付けます。 また、イメージ名の末尾に `:v1` を付加します。 このタグは、イメージのバージョン番号を示します。

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

タグ付けが完了したら、`docker images` を実行して操作を確認します。

```bash
docker images
```

出力:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Azure Container Registry へのイメージのプッシュ

*aci-tutorial-app* イメージをレジストリにプッシュします。

次の例を使用して、コンテナー レジストリ loginServer 名を環境の loginServer に置き換えます。

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Azure Container Registry のイメージの一覧表示

Azure Container Registry にプッシュされたイメージの一覧を返すには、[az acr repository list](/cli/azure/acr/repository#list) コマンドを使用します。 コンテナー レジストリ名でコマンドを更新します。

```azurecli
az acr repository list --name <acrName> --username <acrName> --password <acrPassword> --output table
```

出力:

```azurecli
Result
----------------
aci-tutorial-app
```

次に特定のイメージのタグを表示するには、[az acr repository show-tags](/cli/azure/acr/repository#show-tags) コマンドを使用します。

```azurecli
az acr repository show-tags --name <acrName> --username <acrName> --password <acrPassword> --repository aci-tutorial-app --output table
```

出力:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、Azure Container Registry が Azure Container Instances で使用できるように準備され、コンテナー イメージがプッシュされました。 次の手順を完了しました。

> [!div class="checklist"]
> * Azure Container Registry インスタンスのデプロイ
> * Azure Container Registry のコンテナー イメージのタグ付け
> * Azure Container Registry へのイメージのアップロード

次のチュートリアルでは、Azure Container Instances を使用してコンテナーを Azure にデプロイする方法について学習します。

> [!div class="nextstepaction"]
> [コンテナーを Azure Container Instances にデプロイする](./container-instances-tutorial-deploy-app.md)
