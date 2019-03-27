---
title: 管理グループを作成して Azure リソースを整理する - Azure のガバナンス
description: ポータル、Azure PowerShell、および Azure CLI を使用して、複数のリソースを管理する Azure 管理グループを作成する方法について説明します。
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 01bfd10b2f37a7990ab9a1badfcb09422baa391a
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342203"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>リソースの整理と管理のための管理グループを作成する

管理グループは、複数のサブスクリプションのアクセス、ポリシー、コンプライアンスを管理するのに役立つコンテナーです。 これらのコンテナーを作成して、[Azure Policy](../policy/overview.md) と [Azure ロール ベースのアクセス制御](../../role-based-access-control/overview.md)で使用できる効果的で効率的な階層を構築します。 管理グループについて詳しくは、「[Azure 管理グループでリソースを整理する](overview.md)」をご覧ください。

ディレクトリに作成される最初の管理グループは、完了までに最大 15 分かかる場合があります。 Azure 内でディレクトリの管理グループ サービスを初めて設定する際に実行するプロセスがあります。 プロセスが完了すると、通知を受け取ります。

## <a name="create-a-management-group"></a>管理グループの作成

管理グループを作成するには、ポータル、PowerShell、または Azure CLI を使用します。 現時点では、Resource Manager テンプレートを使用して管理グループを作成することはできません。

### <a name="create-in-portal"></a>ポータルで作成する

1. [Azure Portal](https://portal.azure.com) にログインします。

1. **[すべてのサービス]** > **[管理グループ]** を選択します。

1. メイン ページで、**[新しい管理グループ]** を選択します。

   ![メイン グループ](./media/main.png)

1. [管理グループ ID] フィールドに入力します。

   - **[管理グループ ID]** は、この管理グループでコマンドを送信するために使用するディレクトリの一意識別子です。 この識別子は、このグループを識別するために Azure システム全体で使用されるため、作成後は編集できません。
   - 表示名フィールドは、Azure Portal 内で表示される名前です。 管理グループの作成時には別の表示名は省略可能なフィールドで、いつでも変更できます。  

   ![Create](./media/create_context_menu.png)  

1. **[保存]** を選択します。

### <a name="create-in-powershell"></a>PowerShell で作成する

PowerShell で、New-AzureRmManagementGroup コマンドレットを使用します。

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso'
```

**GroupName** は、作成される一意識別子です。 この ID は、このグループを参照するために他のコマンドで使用され、後で変更することはできません。

Azure Portal 内で管理グループを別の名前で表示する場合は、**DisplayName** パラメーターを文字列と共に追加します。 たとえば、Contoso という GroupName と "Contoso Group" という表示名を持つ管理グループを作成する場合は、次のコマンドレットを使用します。

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group' -ParentId 'ContosoTenant'
```

この管理グループを別の管理の下に作成するには **ParentId** パラメーターを使用します。

### <a name="create-in-azure-cli"></a>Azure CLI で作成する

Azure CLI で、az account management-group create コマンドを使用します。

```azurecli-interactive
az account management-group create --name 'Contoso'
```

## <a name="next-steps"></a>次の手順

管理グループについて詳しくは、以下をご覧ください。

- [管理グループを作成して Azure リソースを整理する](create.md)
- [管理グループを変更、削除、または管理する方法](manage.md)
- [Azure PowerShell Resources モジュールで管理グループを確認する](https://aka.ms/mgPSdocs)
- [REST API で管理グループを確認する](https://aka.ms/mgAPIdocs)
- [Azure CLI で管理グループを確認する](https://aka.ms/mgclidoc)
