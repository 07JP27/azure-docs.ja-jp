---
title: "Azure SQL のサーバーとデータベースを作成し、管理する | Microsoft Docs"
description: "Azure SQL Database のサーバーとデータベース概念について説明し、Azure ポータル、PowerShell、Azure CLI、Transact-SQL、REST API を利用してサーバーとデータベースを作成し、管理する方法について説明します。"
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: d25ffba701ff772899552bf6a5023fa36e28059d
ms.contentlocale: ja-jp
ms.lasthandoff: 07/06/2017


---

# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Azure SQL Database のサーバーとデータベースを作成し、管理する

Azure SQL データベースは Microsoft Azure で管理されるデータベースであり、[Azure リソース グループ](../azure-resource-manager/resource-group-overview.md)内に作成されます。[さまざまなワークロードに対して一連のコンピューティング リソースとストレージ リソース](sql-database-service-tiers.md)が定義されます。 Azure SQL データベースは、特定の Azure リージョン内に作成される、Azure SQL Database の論理サーバーと関連付けられます。 

## <a name="an-azure-sql-database-can-be-a-single-pooled-or-partitioned-database"></a>Azure SQL データベースはプールされ、パーティション分割されるシングル データベース

Azure SQL データベースには以下が相当します。

- 単一のデータベースとその[独自のリソースのセット](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU)
- [一連のリソースを共有する](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) [SQL エラスティック プール](sql-database-elastic-pool.md)の一部 (eDTU)
- [シャード化されたデータベースのスケール アウトされたセット](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling)の一部 (単一またはプールされたデータベースのいずれか)
- [マルチテナント SaaS デザイン パターン](sql-database-design-patterns-multi-tenancy-saas-applications.md)に参加しているデータベースのセットの一部 (これらのデータベースは、単一またはプールされたデータベースのいずれか (またはその両方)) 

> [!TIP]
> 有効なデータベース名については、「[Database Identifiers (データベース識別子)](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers)」を参照してください。 
>
 
- Microsoft Azure SQL Database で使用される既定のデータベース照合順序は **SQL_LATIN1_GENERAL_CP1_CI_AS** です。**LATIN1_GENERAL** は英語 (米国)、**CP1** はコード ページ 1252、**CI** は大文字と小文字の区別なし、**AS** はアクセントの区別ありを表しています。 照合順序を設定する方法の詳細については、「[COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx)」を参照してください。
- Microsoft Azure SQL Database では、表形式データ ストリーム (TDS) プロトコル クライアント バージョン 7.3 以降をサポートしています。
- TCP/IP 接続のみが許可されます。

## <a name="what-is-an-azure-sql-logical-server"></a>Azure SQL 論理サーバーとは何か

論理サーバーは、[SQL エラスティック プール](sql-database-elastic-pool.md)、[ログイン](sql-database-manage-logins.md)、[ファイアウォール規則](sql-database-firewall-configure.md)、[監査規則](sql-database-auditing.md)、[脅威検出ポリシー](sql-database-threat-detection.md)、[フェールオーバー グループ](sql-database-geo-replication-overview.md)など、複数のデータベースの中央管理ポイントとして機能します。 論理サーバーは、そのリソース グループとは別のリージョンに入ることができます。 Azure SQL データベースを作成するには、先に論理サーバーが存在している必要があります。 サーバー上のすべてのデータベースは、論理サーバーと同じリージョン内で作成されます。 


> [!IMPORTANT]
> SQL Database におけるサーバーとは、オンプレミスでなじみのある SQL Server インスタンスとは別の論理コンストラクトです。 具体的には、SQL Database サービスは論理サーバーに関連したデータベースの場所について保証しません。また、インスタンス レベルのアクセスまたは機能を公開しません。
> 

論理サーバーを作成するとき、サーバーのログイン アカウント/パスワードを指定します。このアカウントには、そのサーバー上のマスター データベースとそのサーバーで作成されるすべてのデータベースに対する管理特権が与えられます。 この初回アカウントは SQL ログイン アカウントです。 Azure SQL Database は認証のために SQL 認証と Azure Active Directory 認証をサポートしています。 ログインと認証の詳細については、「[Azure SQL Database におけるデータベースとログインの管理](sql-database-manage-logins.md)」をご覧ください。 Windows 認証はサポートされません。 

> [!TIP]
> 有効なリソース グループとサーバーの名前については、[名前付け規則と制限](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)に関するページを参照してください。
>

Azure SQL Database 論理サーバーは、

- Azure サブスクリプション内に作成されますが、含まれているリソースとともに別のサブスクリプションに移動できます
- データベース、エラスティック プール、およびデータ ウェアハウスの親リソースです
- データベース、エラスティック プール、データ ウェアハウスの名前空間を提供します
- 強力な有効期間のセマンティクスが含まれる論理コンテナーです。サーバーを削除すると、包含データベース、エラスティック プール、データ ウェアハウスが削除されます
- [Azure ロール ベースのアクセス制御 (RBAC)](/active-directory/role-based-access-control-what-is.md) に参加する - サーバー内のデータベース、エラスティック プール、データ ウェアハウスはサーバーからアクセス権を継承します
- Azure のリソース管理目的での、データベース、エラスティック プール、データ ウェアハウスの上位要素です (データベースとプールの URL スキーマを参照してください)
- 領域内にリソースを併置します
- データベース アクセスの接続エンドポイント (<serverName>.database.windows.net) を提供します
- マスター データベースに接続することで、含まれているリソースに関するメタデータへの DMV 経由のアクセスを提供します 
- データベースに適用される管理ポリシーの範囲 (ログイン、ファイアウォール、監査、脅威の検出など) を提供します 
- 親サブスクリプション内のクォータによって制限されます (既定でサブスクリプションあたり 6 サーバー - [サブスクリプションの制限についてはここを参照してください](../azure-subscription-service-limits.md))
- 含まれるリソースのデータベースのクォータと DTU クォータの範囲 (45,000 DTU など) を提供します
- 含まれるリソースで有効な機能のバージョン管理の範囲です 
- サーバー レベルのプリンシパルのログインによってサーバー上のすべてのデータベースを管理できます
- サーバー上の 1 つまたは複数のデータベースへのアクセスが付与された、オンプレミスの SQL Server のインスタンスでのログインと同様のログインを含めることができます。また、限定された管理者権限を付与できます。 詳細については、[ログイン](sql-database-manage-logins.md)に関する記事を参照してください。

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>SQL Database ファイアウォールで保護される Azure SQL データベース

データを保護するために、[SQL Database ファイアウォール](sql-database-firewall-configure.md)は、Azure サブスクリプション接続経由でサーバーに直接接続する以外の、データベース サーバーまたはそのデータベースに対するあらゆるアクセスを禁止します。 追加の接続を有効にするには、[1 つまたは複数のファイアウォール ルールを作成する](sql-database-firewall-configure.md#creating-and-managing-firewall-rules)必要があります。 SQL エラスティック プールの作成と管理については、[エラスティック プール](sql-database-elastic-pool.md)に関する記事を参照してください。

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Azure Portal を利用して Azure SQL Server、データベース、ファイアウォールを管理する

サーバー自体を作成する前に、あるいは作成するときに Azure SQL データベースのリソース グループを作成できます。 新しい SQL サーバーのフォームは新しい SQL サーバーか新しいデータベースを作成するときに表示されます。 

### <a name="create-a-blank-sql-server-logical-server"></a>空の SQL サーバー (論理サーバー) を作成する

[Azure ポータル](https://portal.azure.com)を利用して Azure SQL Database サーバーを作成するには、空の SQL サーバー (論理サーバー) フォームに移動します。 次のスクリーンショットでは、空の論理 SQL サーバーを作成するためのフォームを開く方法の 1 つを確認できます。 

   ![論理サーバー作成フォームへの入力](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

別の方法でこのフォームを表示しても、フォームの情報は同じになります。

### <a name="create-a-blank-or-sample-sql-database"></a>空またはサンプルの SQL データベースを作成する

[Azure ポータル](https://portal.azure.com)を利用して Azure SQL データベースを作成するには、空の SQL データベース フォームに移動し、要求された情報を指定します。 データベース自体を作成する前に、あるいは作成するときに Azure SQL データベースのリソース グループや論理サーバーを作成できます。 Adventure Works LT に基づいて空のデータベースやサンプル データベースを作成できます。 

  ![データベースの作成 -1](./media/sql-database-get-started-portal/create-database-1.png)

> [重要] データベースの価格レベルを選択する方法については、「[サービス層](sql-database-service-tiers.md)」を参照してください。
>

### <a name="manage-an-existing-sql-server"></a>既存の SQL Server を管理する

既存のサーバーを管理するには、さまざまな方法を利用してサーバーに移動します。たとえば、特定の SQL データベース ページ、**SQL サーバー** ページ、**すべてのリソース** ページから移動します。 次のスクリーンショットでは、サーバーの**概要**ページからサーバーレベルのファイアウォールの設定を開始する方法を確認できます。 

   ![論理サーバーの概要](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

既存のデータベースを管理するには、**SQL データベース** ページに移動し、管理するデータベースをクリックします。 次のスクリーンショットでは、データベースの**概要**ページからデータベースにサーバーレベルのファイアウォールを設定する方法を確認できます。 

   ![サーバーのファイアウォール規則](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> データベースのパフォーマンス プロパティを構成する方法については、「[サービス層](sql-database-service-tiers.md)」を参照してください。
>

> [!TIP]
> Azure ポータル クイック スタート チュートリアルについては、「[Azure ポータルで Azure SQL データベースを作成する](sql-database-get-started-portal.md)」を参照してください。
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>PowerShell を利用して Azure SQL Server、データベース、ファイアウォールを管理する

Azure PowerShell を利用して Azure SQL のサーバー、データベース、ファイアウォールを作成し、管理するには、次の PowerShell コマンドレットを使用します。 PowerShell をインストールまたはアップグレードする必要がある場合は、[Azure PowerShell モジュールのインストール](/powershell/azure/install-azurerm-ps)に関するページを参照してください。 SQL エラスティック プールの作成と管理については、[エラスティック プール](sql-database-elastic-pool.md)に関する記事を参照してください。

| コマンドレット | Description |
| --- | --- |
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|リソース グループを作成します。
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|サーバーを作成します。|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|サーバーに関する情報を返します。|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/set-azurermsqlserver)|サーバーのプロパティを変更します。|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|サーバーを削除します。|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|サーバーレベルのファイアウォール規則を作成します。 |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|サーバーのファイアウォール規則を取得します。|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|サーバーのファイアウォール規則を変更します。|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|サーバーからファイアウォール規則を削除します。|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|データベースを作成します。 |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|1 つまたは複数のデータベースを取得します。|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|データベースのプロパティを設定するか、既存のデータベースをエラスティック プールに移動します。|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|データベースを削除します。|

> [!TIP]
> PowerShell クイック スタート チュートリアルについては、「[PowerShell を使用して単一の Azure SQL データベースを作成する](sql-database-get-started-portal.md)」を参照してください。
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Azure CLI を利用して Azure SQL Server、データベース、ファイアウォールを管理します。

[Azure CLI](/cli/azure/overview) を利用して Azure SQL のサーバー、データベース、ファイアウォールを作成し、管理するには、次の [Azure CLI SQL Database](/cli/azure/sql/db) コマンドを使用します。 [Cloud Shell](/azure/cloud-shell/overview) を使用して CLI をブラウザーで実行することも、macOS、Linux、または Windows に[インストール](/cli/azure/install-azure-cli)することもできます。 SQL エラスティック プールの作成と管理については、[エラスティック プール](sql-database-elastic-pool.md)に関する記事を参照してください。

| コマンドレット | Description |
| --- | --- |
|[az group create](/cli/azure/group#create)|リソース グループを作成します。|
|[az sql server create](/cli/azure/sql/server#create)|サーバーを作成します。|
|[az sql server list](/cli/azure/sql/server#list)|サーバーを一覧表示します。|
|[az sql server list-usages](/cli/azure/sql/server#list-usages)|サーバーの使用状況を返します。|
|[az sql server show](/cli/azure/sql/server#show)|サーバーを取得します。|
|[az sql server update](/cli/azure/sql/server#update)|サーバーを更新します。|
|[az sql server delete](/cli/azure/sql/server#delete)|サーバーを削除します。|
|[az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule#create)|サーバーのファイアウォール規則を作成します。|
|[az sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#list)|サーバーのファイアウォール規則を一覧表示します。|
|[az sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#show)|ファイアウォール規則の詳細を表示します。|
|[az sql server firewall-rule update](/cli/azure/sql/server/firewall-rule#update)|ファイアウォール規則を更新します。|
|[az sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#delete)|ファイアウォール規則を削除します。|
|[az sql db create](/cli/azure/sql/db#create) |データベースを作成します。|
|[az sql db list](/cli/azure/sql/db#list)|サーバー内のすべてのデータベースとデータ ウェアハウス、またはエラスティック プール内のすべてのデータベースを一覧表示します。|
|[az sql db list-editions](/cli/azure/sql/db#list-editions)|利用可能なサービス目標と容量の上限を一覧表示します。|
|[az sql db list-usages](/cli/azure/sql/db#list-usages)|データベースの使用状況を返します。|
|[az sql db show](/cli/azure/sql/db#show)|データベースまたはデータ ウェアハウスを取得します。|
|[az sql db update](/cli/azure/sql/db#update)|データベースを更新します。|
|[az sql db delete](/cli/azure/sql/db#delete)|データベースを削除します。|

> [!TIP]
> Azure CLI クイック スタート チュートリアルについては、「[Azure CLI を使用して単一の Azure SQL データベースを作成する](sql-database-get-started-cli.md)」を参照してください。
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Transact-SQL を利用して Azure SQL Server、データベース、ファイアウォールを管理する

Transact-SQL を利用して Azure SQL のサーバー、データベース、ファイアウォールを作成し、管理するには、次の T-SQL コマンドレットを使用します。 これらのコマンドは、Azure Portal、[SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio)、[Visual Studio Code](https://code.visualstudio.com/docs)、または Azure SQL Database サーバーに接続して Transact-SQL コマンドを渡すことができるその他のプログラムを使用して実行できます。 SQL エラスティック プールの管理については、[エラスティック プール](sql-database-elastic-pool.md)に関する記事を参照してください。

> [!IMPORTANT]
> Transact-SQL を利用してサーバーを作成、更新、削除することはできません。
>

| コマンド | Description |
| --- | --- |
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|SQL Database サーバーにサーバーレベルのファイアウォール設定を作成するか、更新します。 このストアド プロシージャは、マスター データベースのサーバーレベル プリンシパル ログインでのみ利用できます。 サーバーレベルのファイアウォール規則は、Azure レベルのアクセス許可を持つユーザーにより最初のサーバーレベルのファイアウォール規則が作成された後で Transact-SQL を使用しなければ作成できません。|
|[sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Microsoft Azure SQL Database に関連付けられているサーバーレベルのファイアウォール設定に関する情報を返します。|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|SQL Database サーバーからサーバーレベルのファイアウォール設定を削除します。 このストアド プロシージャは、マスター データベースのサーバーレベル プリンシパル ログインでのみ利用できます。|
|[CREATE DATABASE (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|新しいデータベースを作成します。 新しいデータベースを作成するには、マスター データベースに接続する必要があります。|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Azure SQL データベースを変更します。 |
|[ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Azure SQL Data Warehouse を変更します。|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|データベースを削除します。|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Azure SQL Database または SQL Data Warehouse のデータベースレベルのファイアウォール規則を作成または更新します。 マスター データベースと SQL Database のユーザー データベースにデータベース ファイアウォール規則を構成できます。 データベース ファイアウォール規則は、包含データベース ユーザーの利用時に便利です。 |
|[sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Microsoft Azure SQL Database に関連付けられているデータベースレベルのファイアウォール設定に関する情報を返します。 |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Azure SQL Database または SQL Data Warehouse からデータベースレベルのファイアウォール設定を削除します。 |
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Azure SQL Database または Azure SQL Data Warehouse のエディション (サービス レベル)、サービス目標 (価格レベル)、およびエラスティック プール名 (存在する場合) を返します。 Azure SQL Database サーバーの master データベースにログオンしている場合は、すべてのデータベースの情報が返されます。 Azure SQL Data Warehouse の場合は、master データベースに接続する必要があります。|
|[sys.database_usage (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Azure SQL Database サーバー上のデータベースの数、種類、および期間を一覧表示します。|

> [!TIP]
> Microsoft Windows で SQL Server Management Studio を使用する方法に関するクイック スタート チュートリアルについては、「[Azure SQL Database: SQL Server Management Studio を使って接続とデータの照会を行う](sql-database-connect-query-ssms.md)」を参照してください。 macOS、Linux、Windows で Visual Studio Code を使用する方法に関するクイック スタート チュートリアルについては、「[Azure SQL Database: Visual Studio Code を使って接続とデータの照会を行う](sql-database-connect-query-vscode.md)」を参照してください。

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>REST API を利用して Azure SQL のサーバー、データベース、ファイアウォールを管理する

REST API を利用して Azure SQL のサーバー、データベース、ファイアウォールを作成し、管理する方法については、「[Azure SQL Database REST API](/rest/api/sql/)」を参照してください。

## <a name="next-steps"></a>次のステップ

- SQL エラスティック プールを利用したプーリング データベースについては、「[エラスティック プール](sql-database-elastic-pool.md)」を参照してください。
- Azure SQL Database のサービスについては、「[SQL Database とは](sql-database-technical-overview.md)」を参照してください。
- SQL Server データベースを Azure に移行する方法については、「[Azure SQL Database に移行](sql-database-cloud-migrate.md)」を参照してください。
- サポートされている機能については、[機能](sql-database-features.md)に関する記事をご覧ください。

