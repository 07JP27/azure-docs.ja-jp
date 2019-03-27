---
title: Log Analytics を使用して Azure HDInsight クラスターを監視する
description: Azure Log Analytics を使って HDInsight クラスターで実行中のジョブを監視する方法を説明します。
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: cd129ea68315223516ac1cd3e7577b5ee4bf92e5
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005116"
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Azure Log Analytics を使用して Azure HDInsight クラスターを監視する

Azure Log Analytics を有効にして HDInsight で Hadoop クラスターの操作を監視する方法と、HDInisght 監視ソリューション追加する方法を説明します。

[Log Analytics](../log-analytics/log-analytics-overview.md) は、クラウドおよびオンプレミス環境の可用性やパフォーマンスを維持するためにそれらの環境を監視する Azure Monitor のサービスです。 Log Analytics を使用すると、クラウドおよびオンプレミスの環境内にあるリソースによって生成されたデータや、他の監視ツールのデータを収集し、複数のソースにわたる分析を行えます。

Azure サブスクリプションをお持ちでない場合は、開始する前に[無料アカウントを作成](https://azure.microsoft.com/free/)してください。

## <a name="prerequisites"></a>前提条件

* **Log Analytics ワークスペース**。 ワークスペースは、独自のデータ リポジトリ、データ ソース、およびソリューションを備えた一意の Log Analytics 環境として考えることができます。 手順については、[Log Analytics ワークスペースの作成](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace)に関するページをご覧ください。

* **Azure HDInsight クラスター**。 現在、Log Analytics は次の HDInsight クラスター タイプで使用することができます。

  * Hadoop
  * hbase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  HDInsight クラスターの作成手順については、[Azure HDInsight の概要](hadoop/apache-hadoop-linux-tutorial-get-started.md)に関するページをご覧ください。

> [!NOTE]  
> パフォーマンス向上のため、HDInsight クラスターと Log Analytics ワークスペースの両方を同じリージョンに配置することをお勧めします。 Azure Log Analytics は、すべての Azure リージョンで使用できるわけではありません。

## <a name="enable-log-analytics-by-using-the-portal"></a>ポータルを使用して Log Analytics を有効にする

ここでは、ジョブやデバッグ ログなどを監視するために、Azure Log Analytics ワークスペースを使用するよう既存の HDInsight Hadoop クラスターを構成します。

1. [Azure Portal](https://portal.azure.com) にサインインします。

2. 左側のメニューから、**[すべてのサービス]** を選択します。

3. **[ANALYTICS]** で **[HDInsight クラスター]** を選択します。

4. 左側から、**[監視]** の下にある **[Operations Management Suite]** を選択します。

5. メイン ビューから、**[OMS 監視]** の下にある **[有効化]** を選択します。

6. **[ワークスペースを選択]** ドロップダウン リストから、既存の Log Analytics ワークスペースを選択します。

7. **[保存]** を選択します。

    ![HDInsight クラスターの監視の有効化](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "HDInsight クラスターの監視の有効化")

    設定を保存するまでしばらく時間がかかります。

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Azure PowerShell を使用して Log Analytics を有効にする

Azure PowerShell を使用して Log Analytics を有効にすることができます。 コマンドレットは次のとおりです。

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
      [-Name] <CLUSTER NAME>
      [-WorkspaceId] <LOG ANALYTICS WORKSPACE NAME>
      [-PrimaryKey] <LOG ANALYTICS WORKSPACE PRIMARY KEY>
      [-ResourceGroupName] <RESOURCE GROUIP NAME>
```

「[Enable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0)」(Enable-AzureRmHDInsightOperationsManagementSuite) をご覧ください。

無効にするコマンドレットは次のとおりです。

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
       [-Name] <CLUSTER NAME>
       [-ResourceGroupName] <RESOURCE GROUP NAME>
```

「[Disable-AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0)」(Disable-AzureRmHDInsightOperationsManagementSuite) をご覧ください。

## <a name="install-hdinsight-cluster-management-solutions"></a>HDInsight クラスター管理ソリューションをインストールする

HDInsight では、Azure Log Analytics に追加できるクラスター固有の管理ソリューションが提供されます。 [管理ソリューション](../log-analytics/log-analytics-add-solutions.md)を使用すると、Log Analytics に機能を追加して、追加のデータおよび分析ツールを提供できます。 これらのソリューションは、HDInsight クラスターから重要なパフォーマンス メトリックを収集し、メトリックを検索するツールを提供します。 また、これらのソリューションは、HDInsight でサポートされるほとんどのクラスターの種類に対する視覚化とダッシュボードも提供します。 このソリューションで収集したメトリックを使用して、独自の監視ルールおよびアラートを作成できます。

利用可能な HDInsight ソリューションは次のとおりです。

* HDInsight Hadoop Monitoring
* HDInsight HBase の監視
* HDInsight Interactive Query Monitoring
* HDInsight Kafka Monitoring
* HDInsight Spark Monitoring
* HDInsight Storm Monitoring

管理ソリューションをインストールする手順については、「[Azure の管理ソリューション](../azure-monitor/insights/solutions.md#install-a-monitoring-solution)」をご覧ください。 テスト用に HDInsight Hadoop Monotiring ソリューションをインストールします。 インストールが完了すると、**[概要]** の下に **HDInsightHadoop** タイルが表示されます。 **HDInsightHadoop** タイルを選択します。 HDInsightHadoop ソリューションは次のようになります。

![HDInsight 監視ソリューションのビュー](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

クラスターは新規のクラスターであるため、レポートにはアクティビティが表示されません。

## <a name="next-steps"></a>次の手順

* [Azure Log Analytics でクエリを実行して Azure HDInsight クラスターを監視する](hdinsight-hadoop-oms-log-analytics-use-queries.md)
