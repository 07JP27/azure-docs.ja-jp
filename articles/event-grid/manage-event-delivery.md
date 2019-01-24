---
title: Azure Event Grid サブスクリプションの配信不能と再試行に関する方針
description: Event Grid のイベント配信オプションをカスタマイズする方法について説明します。 配信不能メッセージの送信先を設定し、配信の再試行時間を指定します。
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/06/2019
ms.author: spelluru
ms.openlocfilehash: 8660f09c41cf6226f2ffb173508d37c260522b80
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474253"
---
# <a name="dead-letter-and-retry-policies"></a>配信不能と再試行に関する方針

イベント サブスクリプションの作成時には、イベント配信の設定をカスタマイズできます。 この記事では、配信不能の場所の設定方法と、再試行の設定のカスタマイズ方法を示します。 これらの機能については、「[Event Grid によるメッセージの配信と再試行](delivery-and-retry.md)」をご覧ください。

## <a name="install-preview-feature"></a>プレビュー機能のインストール

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>配信不能の場所の設定

配信不能の場所を設定するには、エンドポイントに配信できなかったイベントを保持するためストレージ アカウントが必要です。 この例では、既存のストレージ アカウントのリソース ID を取得します。 ここでは、配信不能エンドポイント用にそのストレージ アカウント内のコンテナーを使用するイベント サブスクリプションを作成します。

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

配信不能処理を無効にするには、コマンドを再実行してイベント サブスクリプションを作成しますが、`deadletter-endpoint` の値は指定しないでください。 イベント サブスクリプションを削除する必要はありません。

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$containername = "testcontainer"

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzureRmStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

配信不能処理を無効にするには、コマンドを再実行してイベント サブスクリプションを作成しますが、`DeadLetterEndpoint` の値は指定しないでください。 イベント サブスクリプションを削除する必要はありません。

## <a name="set-retry-policy"></a>再試行ポリシーの設定

Event Grid サブスクリプションの作成時には、Event Grid がイベント配信を試行する期間の値を設定できます。 既定では、Event Grid は、24 時間 (1440 分)、または 30 回の試行を行います。 Event Grid サブスクリプションには、これらの値のいずれかを設定できます。 イベントの有効期限の値は 1 から 1440 までの整数にする必要があります。 最大再試行回数は 1 から 30 までの整数にする必要があります。

[再試行のスケジュール](delivery-and-retry.md#retry-schedule-and-duration)を構成することはできません。

### <a name="azure-cli"></a>Azure CLI

イベントの Time to Live を 1440 分以外の値に設定するには、次のコマンドを使用します。

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

最大再試行回数を 30 以外の値に設定するには、次のコマンドを使用します。

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

`event-ttl` と `max-deliver-attempts` の両方を設定した場合、Event Grid は期限切れについて最初の設定を使用して、イベント配信を停止するタイミングを判別します。

### <a name="powershell"></a>PowerShell

イベントの Time to Live を 1440 分以外の値に設定するには、次のコマンドを使用します。

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

最大再試行回数を 30 以外の値に設定するには、次のコマンドを使用します。

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

`EventTtl` と `MaxDeliveryAttempt` の両方を設定した場合、Event Grid は期限切れについて最初の設定を使用して、イベント配信を停止するタイミングを判別します。

## <a name="next-steps"></a>次の手順

* Azure 関数アプリを利用して配信不能イベントを処理するサンプル アプリケーションについては、「[Azure Event Grid Dead Letter Samples for .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/)」 (.NET 向け Azure Event Grid 配信不能サンプル) を参照してください。
* イベント配信と再試行については、「[Event Grid によるメッセージ配信と再試行](delivery-and-retry.md)」をご覧ください。
* Event Grid の概要については、[Event Grid の紹介](overview.md)に関する記事を参照してください。
* Event Grid の使用をすぐに開始するには、[Azure Event Grid でのカスタム イベントの作成とルーティング](custom-event-quickstart.md)に関する記事を参照してください。
