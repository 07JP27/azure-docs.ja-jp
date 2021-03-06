---
title: MMLSpark Machine Learning を使用する方法 | Microsoft Docs
description: Azure Machine Learning で MMLSpark ライブラリを使用する方法について説明します。
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 6167e10219791466ca275ff02cc051e3227634e8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980043"
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning Library for Apache Spark を使用する方法

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

## <a name="introduction"></a>はじめに

[Microsoft Machine Learning Library for Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) は、大規模なデータセット向けのスケーラブルな Machine Learning モデルを簡単に作成するためのツールを提供します。 これは SparkML パイプラインと [Microsoft Cognitive Toolkit ](https://github.com/Microsoft/CNTK) および [OpenCV](http://www.opencv.org/) の統合を含んでいるため、次のことが可能になります。 
 * イメージ データのイングレスと前処理
 * 事前トレーニング済みの深層学習モデルを使用した、イメージおよびテキストの特徴の生成
 * 暗黙的な特徴の生成を使用した、分類および回帰モデルのトレーニングとスコア付け

## <a name="prerequisites"></a>前提条件

このハウツー ガイドの手順を実行するには、以下のことを行う必要があります。
- [Azure Machine Learning Workbench のインストール](quickstart-installation.md)
- [Azure HDInsight Spark クラスターの設定](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Docker コンテナーで実験を実行する

Azure Machine Learning Workbench は、ローカルまたはリモート VM の Docker コンテナーで実験を実行するときに MMLSpark を使用するように構成されています。 この機能により、PySpark モデルをクラスター上でスケールで実行する前に、モデルのデバッグと実験を簡単に行うことができます。 

サンプルの使用を開始するには、新しいプロジェクトを作成し、ギャラリーの [MMLSpark on Adult Census]\(成人の国勢調査での MMLSpark)\ サンプルを選択します。 プロジェクト ダッシュボードで、計算コンテキストとして [Docker] を選択し、[実行] をクリックします。 Azure Machine Learning Workbench が PySpark プログラムを実行するための Docker コンテナーをビルドし、プログラムを実行します。

実行が完了したら、Azure Machine Learning Workbench の実行履歴ビューで結果を確認できます。

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark クラスターに MMLSpark をインストールする

この手順および次の手順を実行するには、最初に[Azure HDInsight Spark クラスターを作成する](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)必要があります。

既定では、実験を実行すると、Azure Machine Learning Workbench によってクラスターに MMLSpark パッケージがインストールされます。 プロジェクト フォルダー内の _aml_config/spark_dependencies.yml_ という名前のファイルを編集すると、この動作をコントロールして他の Spark パッケージをインストールできます。

```
# Spark configuration properties.
configuration:
  "spark.app.name": "Azure ML Experiment"
  "spark.yarn.maxAppAttempts": 1

repositories:
  - "https://mmlspark.azureedge.net/maven"
packages:
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.9.9"
```

[スクリプト アクション](https://github.com/Azure/mmlspark#hdinsight)を使用して、HDInsight Spark クラスターに直接 MMLSpark をインストールすることもできます。

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Azure HDInsight Spark クラスターを計算ターゲットとして設定する

Azure Machine Learning Workbench の [ファイル] メニューで [コマンド プロンプトを開く] をクリックして、CLI ウィンドウを開きます。

CLI ウィンドウで、次のコマンドを実行します。

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

これで、クラスターをプロジェクトの計算ターゲットとして使用できるようになりました。

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Azure HDInsight Spark クラスターで実験を実行する

[MMLSpark on Adult Census]\(成人の国勢調査での MMLSpark)\ サンプルのプロジェクト ダッシュボードに戻りましょう。 クラスターを計算ターゲットとして選択し、[実行] をクリックします。

Azure Machine Learning Workbench が、Spark ジョブをクラスターに送信します。 実行履歴ビューで進行状況を監視し、結果を確認することができます。

## <a name="next-steps"></a>次の手順
MMLSpark ライブラリの詳細と例については、[MMLSpark の GitHub リポジトリ](https://github.com/Azure/mmlspark)に関するページをご覧ください。

*Apache®、Apache Spark、および Spark® は、Apache Software Foundation の米国およびその他の国における登録商標です。*
