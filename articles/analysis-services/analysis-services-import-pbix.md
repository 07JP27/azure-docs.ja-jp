---
title: Power BI Desktop ファイルを Azure Analysis Services にインポートする | Microsoft Docs
description: Azure Portal を使って Power BI Desktop ファイル (pbix) をインポートする方法を説明します。
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 47223f22c797d892bc7cbdc0086439ee9cae9fcb
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54187724"
---
# <a name="import-a-power-bi-desktop-file"></a>Power BI Desktop ファイルをインポートする

Power BI Desktop ファイル (pbix) のデータモデルを Azure Analysis Services にインポートできます。 モデルのメタデータ、キャッシュ データ、およびデータ ソース接続がインポートされます。 レポートや視覚エフェクトはインポートされません。 Power BI Desktop からインポートされたデータ モデルは、1400 および 1465 互換性レベルです。

> [!IMPORTANT]
> このフィーチャーは非推奨です。 今後の更新プログラムで削除されるか、大幅に変更される可能性があります。 今後の更新プログラムとの互換性を維持するために、新規および既存のプロジェクトでこの機能の使用を中止することをお勧めします。 高度なモデルの開発とテストを実行する場合は、Visual Studio (SSDT) と SQL Server Management Studio (SSMS) を使用することが最善の方法です。

## <a name="restrictions"></a>制限  

- Power BI Desktop July 2018 Update (2.60.5169.3201) 以降でデータ モデルが作成されている場合、プレビュー機能が有効になっていないことを確認します。 Azure Analysis Services では、プレビュー機能はまだサポートされていません。 インポート中に次のエラー メッセージが表示される場合、pbix ファイルで、Azure Analysis Services ではまだサポートされていないプレビュー機能が有効になっています。

    ![互換性レベルの警告](./media/analysis-services-import-pbix/aas-import-pbix-cl-warning.png)   
- pbix ファイルからインポートするには、サーバー管理者のアクセス許可が必要です。
- pbix モデルは、**Azure SQL Database** と **Azure SQL Data Warehouse** のデータソースのみに接続できます。
- pbix モデルでは、ライブ接続や DirectQuery 接続は使用できません。 


## <a name="to-import-from-pbix"></a>pbix からインポートするには

1. サーバーの **[概要]** > **[Web デザイナー]** で、**[開く]** をクリックします。

    ![Azure ポータルでモデルを作成する](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. **[Web デザイナー]** > **[モデル]** を選択し、**[+ 追加]** をクリックします。

    ![Azure ポータルでモデルを作成する](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. **[新しいモデル]** で、モデルの名前を入力し、Power BI Desktop ファイルを選びます。

    ![Azure ポータルの [新しいモデル] ダイアログ](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. **[インポート]** で、ファイルを探して選びます。

     ![Azure ポータルの [接続] ダイアログ](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="change-credentials"></a>資格情報の変更

pbix ファイルからデータ モデルをインポートする場合、データソースに接続するために使用される資格情報には ServiceAccount が既定で設定されます。 pbix からモデルをインポートした後、次のメソッドを使用して資格情報を変更できます。

- 資格情報を編集するには、2018 年 7 月 (バージョン 17.8.1) 以降のバージョンの SSMS を使用します。 これは最も簡単な方法です。
- [DataSources オブジェクト](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl)に対して TMSL の [Alter コマンド](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl)を使用して、接続文字列プロパティを変更します。 
- Visual Studio でモデルを開いてデータソース接続用の資格情報を編集した後、モデルを再デプロイします。

SSMS を使用して資格情報を変更するには 

1. SSMS で、[データベース] > **[接続]** を展開します。 
2. データベース接続を右クリックし、**[資格情報の更新]** をクリックします。 

    ![資格情報を更新する](./media/analysis-services-import-pbix/aas-import-pbix-creds.png)

3. [資格情報] ダイアログ ボックスで、資格情報の種類を選択し、資格情報を入力します。 SQL 認証の場合は、データベースを選択します。 組織アカウント (OAuth) の場合は、Microsoft アカウントを選択します。
    ![資格情報を編集する](./media/analysis-services-import-pbix/aas-import-pbix-edit-creds.png)

Power BI Desktop の 2018 年 7 月のバージョンには、データソースのアクセス許可を変更するための新しい機能が含まれています。 **[ホーム]** タブで、**[クエリの編集]**  > **[データソースの設定]** をクリックします。 データソースの接続を選択し、**[アクセス許可の編集]** をクリックします。


## <a name="see-also"></a>関連項目

[Azure Portal でモデルを作成する](analysis-services-create-model-portal.md)   
[Azure Analysis Services に接続する](analysis-services-connect.md)  
