---
title: 'チュートリアル: Azure Machine Learning で画像分類モデルをトレーニングする'
description: Python Jupyter Notebook で scikit-learn 画像分類モデルをトレーニングする方法について説明します。 このチュートリアルは、2 部構成のシリーズのパート 1 です。
author: hning86
ms.author: haining
ms.topic: conceptual
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: bed4abcce3019607715416b5194a2ddecc89b76a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966613"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning"></a>チュートリアル #1: Azure Machine Learning で画像分類モデルをトレーニングする

このチュートリアルでは、機械学習モデルのトレーニングをローカルに行ったり、リモートのコンピューティング リソース上で行ったりします。 Python Jupyter Notebook 内の Azure Machine Learning サービスに関するトレーニングとデプロイのワークフローを使用します。  それからノートブックをテンプレートとして使用し、独自のデータで独自の機械学習モデルをトレーニングできます。 このチュートリアルは、**2 部構成のチュートリアル シリーズのパート 1 です**。  

このチュートリアルでは、Azure Machine Learning で [MNIST](http://yann.lecun.com/exdb/mnist/) データセットや [scikit-learn](http://scikit-learn.org) を使用して、単純なロジスティック回帰をトレーニングします。  MNIST は、70,000 ものグレースケールのイメージから成る、人気のあるデータセットです。 各イメージは、0 から 9 までの数値を表す 28x28 ピクセルの手書き数字です。 多クラス分類子を作成して、特定のイメージが表す数字を識別することが目標です。 

以下の項目について説明します。

> [!div class="checklist"]
> * 開発環境を設定する
> * データにアクセスして検査する
> * 人気のある scikit-learn 機械学習ライブラリを使って、単純なロジスティック回帰モデルをローカルでトレーニングする 
> * リモート クラスター上で複数のモデルをトレーニングする
> * トレーニングの結果をレビューし、最適なモデルを登録する

その後[このチュートリアルのパート 2](tutorial-deploy-models-with-aml.md) で、モデルを選択してデプロイする方法を学習します。 

Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。

## <a name="get-the-notebook"></a>ノートブックを入手する

便利なように、このチュートリアルは Jupyter Notebook として提供されています。 `tutorials/01.train-models.ipynb` ノートブックを実行するには、以下のいずれかの方式を使用します。

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="set-up-your-development-environment"></a>開発環境を設定する

Python Notebook で、開発作業に関するすべての設定を行うことができます。  設定には次のものが含まれます。

* Python パッケージをインポートする
* ワークスペースに接続し、ローカル コンピューターとリモート リソースの間で通信できるようにする
* 実験を作成して、すべての実行を追跡する
* リモート コンピューティング ターゲットを作成して、トレーニングに使用する

### <a name="import-packages"></a>パッケージのインポート

このセッションで必要な Python パッケージをインポートします。 さらに、Azure Machine Learning の SDK のバージョンを表示します。

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>ワークスペースへの接続

既存のワークスペースからワークスペース オブジェクトを作成します。 `Workspace.from_config()` は、**config.json** ファイルを読み取り、詳細情報を `ws` という名前のオブジェクト内に読み込みます。

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>実験の作成

実験を作成して、ワークスペース内のすべての実行を追跡します。  

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-remote-compute-target"></a>リモート コンピューティング ターゲットの作成

Azure Batch AI は、Azure 仮想マシン (GPU をサポートする VM を含む) のクラスター上で、データ サイエンティストが機械学習モデルをトレーニングすることを可能にする管理されたサービスです。  このチュートリアルでは、トレーニング環境として Azure Batch AI クラスターを作成します。 このコードは、クラスターがまだワークスペース内にない場合にクラスターを作成します。 

 **クラスターの作成には約 5 分かかります。** ワークスペース内にクラスターが既存の場合は、このコードはそのクラスターを使用し、作成プロセスをスキップします。


```python
from azureml.core.compute import ComputeTarget, BatchAiCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
batchai_cluster_name = "traincluster"

try:
    # look for the existing cluster by name
    compute_target = ComputeTarget(workspace=ws, name=batchai_cluster_name)
    if compute_target is BatchAiCompute:
        print('found compute target {}, just use it.'.format(batchai_cluster_name))
    else:
        print('{} exists but it is not a Batch AI cluster. Please choose a different name.'.format(batchai_cluster_name))
except ComputeTargetException:
    print('creating a new compute target...')
    compute_config = BatchAiCompute.provisioning_configuration(vm_size="STANDARD_D2_V2", # small CPU-based VM
                                                                #vm_priority='lowpriority', # optional
                                                                autoscale_enabled=True,
                                                                cluster_min_nodes=0, 
                                                                cluster_max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, compute_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it uses the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
    # Use the 'status' property to get a detailed status for the current cluster. 
    print(compute_target.status.serialize())
```

これで、クラウドでモデルをトレーニングするために必要なパッケージとコンピューティング リソースが揃いました。 

## <a name="explore-data"></a>データを調査する

モデルのトレーニングの前に、トレーニングに使用するデータを解釈する必要があります。  また、クラウドのトレーニング環境で利用できるように、このデータをクラウド内にコピーする必要もあります。  このセクションでは、次の方法について説明します。

* MNIST データセットのダウンロード
* 複数のサンプル イメージの表示
* クラウドへのデータのアップロード

### <a name="download-the-mnist-dataset"></a>MNIST データセットのダウンロード

MNIST データセットをダウンロードし、ファイルを `data` ディレクトリ内にローカルに保存します。  トレーニング用とテスト用にイメージとラベルがダウンロードされます。  


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>複数のサンプル イメージの表示

圧縮されたファイルを `numpy` 配列内に読み込みます。 それから `matplotlib` を使用して、ラベルがあるデータセットから 30 個のランダムなイメージをプロットします。



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

ランダムなイメージのサンプルは、以下のように表示されます。

![ランダムなイメージのサンプル](./media/tutorial-train-models-with-aml/digits.png)

これで、これらのイメージの概観が頭に浮かんだり、予測結果を想定したりできるようになりました。

### <a name="upload-data-to-the-cloud"></a>クラウドへのデータのアップロード

次に、ローカル コンピューターからクラウド内にデータをアップロードして、データをリモートから利用できるようにします。これにより、リモート トレーニングでデータを利用できるようになります。 データストアは、データをアップロード/ダウンロードしたり、リモート コンピューティング ターゲットから対話したりするために、ワークスペースに関連付けられる便利なコンストラクトです。 

MNIST ファイルは、データストアのルートにある `mnist` という名前のディレクトリ内にアップロードされます。

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
これで、モデルのトレーニングを開始するために必要なものがすべて揃いました。 

## <a name="train-a-model-locally"></a>ローカルでモデルをトレーニングする

scikit-learn からの単純なロジスティック回帰モデルをローカルでトレーニングします。

コンピューターの構成に応じて、**ローカルでのトレーニングには 1、2 分かかります**。

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

次に、テスト セットを使用して予測を作成し、精度を計算します。 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

ローカルのモデルの精度は次のように表示されます。

`0.9202`

わずか 2、3 行のコードで、精度は 92% です。

## <a name="train-on-a-remote-cluster"></a>リモート クラスターでのトレーニング

次に、別の正則化率を使用してモデルを構築し、この単純なモデルを展開できます。 今回は、リモート リソース上でモデルをトレーニングします。  

このタスクでは、以前に設定したリモート トレーニング クラスターにジョブを送信します。  ジョブを送信するには、次のようにします。
* ディレクトリを作成する
* トレーニング スクリプトを作成する
* 推定を作成する
* ジョブを送信する 

### <a name="create-a-directory"></a>ディレクトリを作成する

必要なコードをコンピューターからリモート リソースに配信するためのディレクトリを作成します。

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>トレーニング スクリプトを作成する

ジョブをクラスターに送信するには、まずトレーニング スクリプトを作成します。 次のコードを実行して、作成したばかりのディレクトリ内に `train.py` と呼ばれるトレーニング スクリプトを作成します。 このトレーニングでは正則化率をトレーニング アルゴリズムに追加するので、ローカル バージョンとは多少異なるモデルになります。

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_submitted_run()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

以下の、スクリプトがデータを取得してモデルを保存する方法に注目してください。

+ トレーニング スクリプトが引数を読み取り、データが含まれるディレクトリを検出します。  後でジョブを送信する際に、次のように、引数にデータストアを指定します。`parser.add_argument('--data-folder', type = str, dest = 'data_folder', help = 'data directory mounting point')`

    
+ トレーニング スクリプトが、outputs という名前のディレクトリ内にモデルを保存します。 <br/>
`joblib.dump(value = clf, filename = 'outputs/sklearn_mnist_model.pkl')`<br/>
このディレクトリ内に書き込まれたものはすべてワークスペース内に自動的にアップロードされます。 チュートリアルの後半で、このディレクトリからモデルにアクセスします。

データセットを正しく読み込むために、`utils.py` ファイルがトレーニング スクリプトから参照されます。  このスクリプトをスクリプト フォルダーにコピーして、リモート リソース上でトレーニング スクリプトと共にアクセスできるようにします。


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>推定を作成する

実行の送信には、推定オブジェクトが使用されます。  推定を作成するには、以下のコードを実行し、次のものを定義します。

* 推定オブジェクトの名前 `est`
* スクリプトが含まれるディレクトリ。 このディレクトリ内のすべてのファイルは、実行のためにクラスター ノード内にアップロードされます。 
* コンピューティング ターゲット。  ここでは、作成した Batch AI クラスターを使用します
* トレーニング スクリプト名 train.py
* トレーニング スクリプトからの必須パラメーター 
* トレーニングに必要な Python パッケージ

このチュートリアルでは、このターゲットは Batch AI クラスターです。 プロジェクト ディレクトリ内のすべてのファイルは、実行のためにクラスター ノード内にアップロードされます。 データストア (`ds.as_mount()`) を使用するために data_folder が設定されます。

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>ジョブをクラスターに送信する

推定オブジェクトを送信して、実験を実行します。

```python
run = exp.submit(config=est)
run
```

呼び出しは非同期なので、ジョブが開始されると即時に**実行**状態が返されます。

## <a name="monitor-a-remote-run"></a>リモート実行を監視する

初回の実行は合計で**約 10 分**かかります。 しかし、その後の実行の場合、スクリプトの依存関係が変わらなければ、同じイメージが再利用されるので、コンテナーの起動時間が大幅に短縮されます。

待っている間に次のことが行われます。

- **イメージの作成**: 推定で指定した Python 環境と一致する Docker イメージが作成されます。 このイメージがワークスペースにアップロードされます。 イメージの作成とアップロードには**約 5 分**かかります。 

  その後の実行のためにコンテナーがキャッシュに入れられるので、この段階は Python 環境ごとに 1 回行われます。  イメージの作成中に、ログが実行履歴にストリーミングされます。 これらのログを使用して、イメージの作成の進行状況を監視できます。

- **拡大縮小**: 現在使用できるものより多くのノードがリモート クラスターで必要な場合、自動的にノードがさらに追加されます。 通常、拡大縮小には**約 5 分**かかります。

- **実行**: この段階では、必要なスクリプトとファイルがコンピューティング ターゲットに送信され、データ ストアがマウント/コピーされてから、entry_script が実行されます。 ジョブの実行中に、stdout と ./logs ディレクトリが実行履歴にストリーミングされます。 これらのログを使用して、実行の進行状況を監視できます。

- **後処理**: この実行の ./outputs ディレクトリがワークスペース内の実行履歴に上書きコピーされ、これらの結果にアクセスできるようになります。


複数の方法で、実行中のジョブの進行状況を確認できます。 このチュートリアルでは、`wait_for_completion` メソッドに加えて Jupyter ウィジェットを使用します。 

### <a name="jupyter-widget"></a>Jupyter ウィジェット

Jupyter ウィジェットを使用して、実行の進行状況を監視します。  実行の送信と同様に、このウィジェットも非同期です。また、ジョブが完了するまで 10 秒から 15 秒ごとにライブ更新を提供します。


```python
from azureml.train.widgets import RunDetails
RunDetails(run).show()
```

トレーニングの終わりに表示される、このウィジェットの静止画スナップショットを次に示します。

![ノートブックのウィジェット](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>完了時にログの結果を取得する

モデルのトレーニングと監視は、バック グラウンドで行われます。 モデルのトレーニングが完了するまで待ってから、さらにコードを実行します。 `wait_for_completion` を使用して、モデルのトレーニングが完了する時点を表示します。 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>実行結果を表示する

これで、リモート クラスター上のモデルのトレーニングが完了しました。  次のようにして、モデルの精度を取得します。

```python
print(run.get_metrics())
```
トレーニング中に正則化率が追加されたために、リモート モデルの精度がローカル モデルよりもわずかに高いことが出力に示されます。  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

デプロイのチュートリアルで、このモデルについてさらに詳しく探索できます。

## <a name="register-model"></a>モデルの登録

トレーニング クリプトの最後のステップで、`outputs/sklearn_mnist_model.pkl` ファイルを、ジョブの実行場所のクラスターの VM 内の `outputs` という名前のディレクトリ内に書き込みました。 `outputs` は特別なディレクトリで、このディレクトリ内のすべてのコンテンツがワークスペースに自動的にアップロードされます。  このコンテンツは、ワークスペースの実験内の実行レコード内に表示されます。 したがって、モデル ファイルはワークスペースでも使用できるようになっています。

この実行に関連付けられているファイルを表示できます。

```python
print(run.get_file_names())
```

ワークスペース内でモデルを登録して、後で自分や他のコラボレーターがこのモデルのクエリ、検査、デプロイを行えるようにします。

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>リソースのクリーンアップ

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Azure の管理対象コンピューティング クラスターだけを削除することもできます。 しかし、自動スケーリングがオンになっており、クラスターの最小値が 0 なので、この特定のリソースが使用中でないときの追加のコンピューティングの料金はかかりません。


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>次の手順

この Azure Machine Learning のチュートリアルでは、Python を使用して次の作業を行いました。

> [!div class="checklist"]
> * 開発環境を設定する
> * データにアクセスして検査する
> * 人気のある scikit-learn 機械学習ライブラリを使って、単純なロジスティック回帰モデルをローカルでトレーニングする
> * リモート クラスター上で複数のモデルをトレーニングする
> * トレーニングの詳細情報をレビューし、最適なモデルを登録する

チュートリアル シリーズの次のパートの説明に従って、この登録済みのモデルをデプロイする準備ができました。

> [!div class="nextstepaction"]
> [チュートリアル 2 - モデルをデプロイする](tutorial-deploy-models-with-aml.md)