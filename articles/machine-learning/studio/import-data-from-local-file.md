---
title:ファイルからデータをインポートする titleSuffix: Azure Machine Learning Studio description:ハード ドライブのトレーニング データ ファイルを Azure Machine Learning Studio にアップロードする方法について説明します。 これにより、ワークスペースにデータセット モジュールが作成されます。
services: machine-learning ms.service: machine-learning ms.component: studio ms.topic: article

author: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date:03/20/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-azure-machine-learning-studio"></a>ハード ドライブ上のファイルのトレーニング データを Azure Machine Learning Studio にインポートする

ハード ドライブからデータ ファイルをアップロードして、Azure Machine Learning Studio でトレーニング データとして使用する方法について説明します。 データ ファイルをインポートすると、ワークスペースでデータセット モジュールが用意されます。

## <a name="steps-to-import-data-from-a-local-file"></a>ローカル ファイルからのデータのインポート手順
ローカル ハード ドライブからデータをインポートするには、次の手順を実行します。

1. Machine Learning Studio ウィンドウの下部で、 **[+新規]** をクリックします。
2. **[データセット]** と **[FROM LOCAL FILE]** を選択します。
3. **[Upload a new dataset]** ダイアログ ボックスで、アップロードするファイルを参照します。
4. 名前を入力し、データ型を指定したら、必要に応じて説明を入力します。 データの特徴を記録しておくと、後でデータを使用する際に参照できるため、説明を入力しておくことをお勧めします。
5. チェックボックス **[This is the new version of an existing dataset]** をオンにしておくことにより、新しいデータで既存のデータセットを更新できます。 このチェックボックスをクリックし、既存のデータセット名を入力します。

![新しいデータセットのアップロード](./media/import-data-from-local-file/upload-dataset.png)

アップロードする際に、ファイルがアップロードされていることを示すメッセージが表示されます。 アップロード時間は、データのサイズと、サービスへの接続速度に依存します。 ファイルのアップロードに時間がかかる場合、待機している間に Machine Learning Studio で他の作業も実行できます。 ただし、ブラウザーを閉じるとデータのアップロードに失敗します。

## <a name="dataset-module-is-ready-for-use"></a>データセット モジュールの使用準備完了
データがアップロードされると、データセット モジュールに格納され、ワークスペース内のすべての実験で使用できるようになります。

実験を編集するときは、モジュール パレットの **[保存されたデータセット]** 一覧の **[マイ データセット]** 一覧に、作成済みのデータセットが表示されます。 データセットを別の分析と機械学習に使用する場合は、データセットを実験キャンバスにドラッグ アンド ドロップします。
