---
title: "Azure PowerShell スクリプト - Azure Cosmos DB の DocumentDB API アカウントを作成する | Microsoft Docs"
description: "Azure PowerShell サンプル スクリプト - Azure Cosmos DB の DocumentDB API アカウントを作成する"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 13bbce6fd02979fb3b59f1dbe7f7010d3ca4b7ac
ms.contentlocale: ja-jp
ms.lasthandoff: 05/10/2017

---

# <a name="azure-cosmos-db-create-a-documentdb-api-account-using-powershell"></a>Azure Cosmos DB: PowerShell を使用して DocumentDB API アカウントを作成する

この PowerShell サンプル スクリプトでは、Azure Cosmos DB API アカウントを作成します。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>サンプル スクリプト

[!code-powershell[メイン](../../../powershell_scripts/cosmosdb/create-and-configure-database/create-and-configure-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Azure Cosmos DB アカウントを作成する")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ

スクリプト サンプルの実行後は、次のコマンドを使用してリソース グループとすべての関連リソースを削除することができます。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | すべてのリソースを格納するリソース グループを作成します。 |
| [New-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | データベースまたはエラスティック プールをホストする論理サーバーを作成します。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | 入れ子になったリソースすべてを含むリソース グループを削除します。 |
|||

## <a name="next-steps"></a>次のステップ

Azure PowerShell の詳細については、[Azure PowerShell のドキュメント](https://docs.microsoft.com/powershell/)を参照してください。

Azure Cosmos DB のその他の PowerShell サンプル スクリプトについては、[Azure Cosmos DB の PowerShell スクリプト](../powershell-samples.md)に関する記事をご覧ください。
