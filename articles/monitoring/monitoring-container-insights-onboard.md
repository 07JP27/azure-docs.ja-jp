---
title: コンテナーに Azure Monitor をオンボードする方法 | Microsoft Docs
description: この記事では、コンテナーがどのように動作し、動作状況に関してどのような問題が特定されているかを把握できるように、Azure Monitor をコンテナーにオンボードして構成する方法について説明します。
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: magoedte
ms.openlocfilehash: df145ebe6276c911ef3064e3f8ff7a23a2faa870
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423036"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>コンテナーに Azure Monitor をオンボードする方法
この記事では、Kubernetes 環境にデプロイされ、[Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) 上でホストされているワークロードのパフォーマンスを監視するために、コンテナーに対して Azure Monitor を設定する方法について説明します。

## <a name="prerequisites"></a>前提条件 
始める前に、必ず以下のものを用意してください。

- 新規または既存の AKS クラスター。
- microsoft/oms:ciprod04202018 バージョン以上の、Linux 向けのコンテナー化された Log Analytics エージェント。 バージョン番号は、*mmddyyyy* のような日付の形式で表されます。 エージェントは、この機能のオンボード時に自動的にインストールされます。 
- Log Analytics ワークスペース。 新しい AKS クラスターの監視を有効にするときにワークスペースを作成すること、またはオンボード エクスペリエンスを使用して AKS クラスター サブスクリプションの既定のリソース グループに既定のワークスペースを作成することができます。 自分でワークスペースを作成する場合は、[Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)、[PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json)、[Azure portal](../log-analytics/log-analytics-quick-create-workspace.md) のいずれかを使用して作成できます。
- コンテナーの監視を有効にするための Log Analytics 共同作成者ロール。 Log Analytics ワークスペースへのアクセスを制御する方法の詳細については、「[ワークスペースを管理する](../log-analytics/log-analytics-manage-access.md)」を参照してください。

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>コンポーネント 

パフォーマンスの監視機能では、クラスター内のすべてのノードからパフォーマンス データおよびイベント データを収集する、Linux 向けのコンテナー化された Log Analytics エージェントを利用します。 コンテナーの監視を有効にすると、エージェントは自動的に展開され、指定した Log Analytics ワークスペースに登録されます。 

>[!NOTE] 
>AKS クラスターが既にデプロイされている場合は、この記事の後半で説明されているように、Azure CLI または提供されている Azure Resource Manager テンプレートを使用して、監視を有効にします。 `kubectl` を使用してアップグレード、エージェントを削除、再展開、または展開することはできません。 
>

## <a name="sign-in-to-the-azure-portal"></a>Azure ポータルにサインインします。
[Azure Portal](https://portal.azure.com) にサインインします。 

## <a name="enable-monitoring-for-a-new-cluster"></a>新しいクラスターの監視を有効にする
デプロイ時に、Azure portal または Azure CLI で新しい AKS クラスターの監視を有効にすることができます。 ポータルから有効にする場合は、クイック スタートの記事「[Azure Kubernetes Service (AKS) クラスターのデプロイ](../aks/kubernetes-walkthrough-portal.md)」の手順に従ってください。 **[監視]** ページの **[監視を有効にする]** オプションで **[はい]** を選択して、既存の Log Analytics ワークスペースを選ぶか、新しく作成します。 

Azure CLI で作成した新しい AKS クラスターの監視を有効にするには、クイック スタート記事の「[AKS クラスターの作成](../aks/kubernetes-walkthrough.md#create-aks-cluster)」セクションの手順に従ってください。  

>[!NOTE]
>Azure CLI を使用する場合は、まず、ローカルに CLI をインストールして使用する必要があります。 Azure CLI バージョン 2.0.43 以降を実行する必要があります。 ご利用のバージョンを識別するには、`az --version` を実行します。 Azure CLI をインストールまたはアップグレードする必要がある場合は、[Azure CLI のインストール](https://docs.microsoft.com/cli/azure/install-azure-cli)に関するページを参照してください。 
>

監視を有効にし、すべての構成タスクが正常に完了すると、次の 2 つの方法のいずれかを使用して、クラスターのパフォーマンスを監視することができます。

* 左側のウィンドウで **[正常性]** を選択し、AKS クラスターで直接監視します。
* 選択したクラスターの AKS クラスター ページで、**[Monitor Container insights]\(コンテナーの分析情報の監視\)** タイルを選択します。 Azure Monitor の左側のウィンドウにある **[正常性]** を選択します。 

  ![AKS でコンテナーに Azure Monitor を選択するためのオプション](./media/monitoring-container-insights-onboard/kubernetes-select-monitoring-01.png)

監視を有効にした後、クラスターの正常性メトリックが表示されるまで、約 15 分かかる場合があります。 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>既存のマネージド クラスターに対する監視を有効にする
既にデプロイされている AKS クラスターの監視を有効にするには、Azure CLI、ポータル、または提供されている Azure Resource Manager テンプレートで PowerShell コマンドレット `New-AzureRmResourceGroupDeployment` を使用します。 

### <a name="enable-monitoring-using-azure-cli"></a>Azure CLI を使用して監視を有効にする
Azure CLI を使用して AKS クラスターの監視を有効にするには、次の手順のようにします。 この例では、ワークスペースを事前に作成したり、既存のワークスペースを指定したりする必要はありません。 このコマンドでは、リージョンにワークスペースがまだ存在しない場合、AKS クラスター サブスクリプションの既定のリソース グループに既定のワークスペースが作成されるので、プロセスが簡単になります。  作成される既定のワークスペースは、*DefaultWorkspace-<GUID>-<Region>* のような形式になります。  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

出力は次のようになります。

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-from-azure-monitor"></a>Azure Monitor からの監視を有効にする
Azure portal で Azure Monitor からの AKS クラスターの監視を有効にするには、次のようにします。

1. Azure portal で、**[モニター]** を選択します。 
2. 一覧から **[コンテナー (プレビュー)]** を選択します。
3. **[モニター - コンテナー (プレビュー)]** ページで、**[Non-monitored clusters]\(監視対象外のクラスター\)** を選択します。
4. 監視対象外のクラスターの一覧でコンテナーを検索し、**[有効にする]** をクリックします。   
5. **[Onboarding to Container Health and Logs]\(コンテナー正常性およびログのオンボード\)** ページにクラスターと同じサブスクリプションの既存の Log Analytics ワークスペースが存在する場合は、ドロップダウン リストから選択します。  
    このリストでは、サブスクリプションで AKS コンテナーがデプロイされている既定のワークスペースと場所が事前に選択されています。 

    ![AKS コンテナーの分析情報の監視を有効にする](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >クラスターからの監視データを格納するための新しい Log Analytics ワークスペースを作成する場合は、[Log Analytics ワークスペースの作成](../log-analytics/log-analytics-quick-create-workspace.md)に関する記事の手順に従います。 必ず、AKS コンテナーがデプロイされるのと同じサブスクリプションでワークスペースを作成してください。 
 
監視を有効にした後、クラスターの正常性メトリックが表示されるまで、約 15 分かかる場合があります。 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>ポータルで AKS クラスターからの監視を有効にする
Azure Portal で AKS コンテナーの監視を有効にするには、次のようにします。

1. Azure Portal で **[すべてのサービス]** を選択します。 
2. リソースの一覧で、「**Containers**」と入力を開始します。  
    入力内容に基づいて、一覧がフィルター処理されます。 
3. **[Kubernetes サービス]** を選択します。  

    ![Kubernetes サービスのリンク](./media/monitoring-container-insights-onboard/portal-search-containers-01.png)

4. コンテナーの一覧で、コンテナーを選択します。
5. コンテナーの概要ページで、**[Monitor コンテナーの正常性]** を選択します。  
6. そのクラスターと同じサブスクリプションに既存の Log Analytics ワークスペースがある場合は、**[コンテナー正常性およびログのオンボード]** ページのドロップダウン リストでそれを選択します。  
    このリストでは、サブスクリプションで AKS コンテナーがデプロイされている既定のワークスペースと場所が事前に選択されています。 

    ![AKS コンテナーの正常性の監視を有効にする](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >クラスターからの監視データを格納するための新しい Log Analytics ワークスペースを作成する場合は、[Log Analytics ワークスペースの作成](../log-analytics/log-analytics-quick-create-workspace.md)に関する記事の手順に従います。 必ず、AKS コンテナーがデプロイされるのと同じサブスクリプションでワークスペースを作成してください。 
 
監視を有効にした後、クラスターのオペレーショナル データが表示されるまで、約 15 分かかる場合があります。 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Azure Resource Manager テンプレートを使用して監視を有効にする
この方法には、2 つの JSON テンプレートが含まれます。 1 つのテンプレートでは監視を有効にする構成が指定され、もう 1 つのテンプレートには、次を指定するために構成するパラメーター値が含まれています。

* AKS コンテナー リソース ID。 
* クラスターがデプロイされるリソース グループ。
* ワークスペースを作成する Log Analytics ワークスペースとリージョン。 

Log Analytics ワークスペースは、手動で作成する必要があります。 ワークスペースを作成するには、[Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)、[PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json)、[Azure portal](../log-analytics/log-analytics-quick-create-workspace.md) のいずれかを使用して設定できます。

テンプレートを使用するリソースのデプロイの概念について馴染みがない場合は、以下を参照してください。
* [Resource Manager テンプレートと Azure PowerShell を使用したリソースのデプロイ](../azure-resource-manager/resource-group-template-deploy.md)
* [Resource Manager テンプレートと Azure CLI を使用したリソースのデプロイ](../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI を使用する場合は、まず、ローカルに CLI をインストールして使用する必要があります。 Azure CLI バージョン 2.0.27 以降を実行する必要があります。 ご利用のバージョンを識別するには、`az --version` を実行します。 Azure CLI をインストールまたはアップグレードする必要がある場合は、[Azure CLI のインストール](https://docs.microsoft.com/cli/azure/install-azure-cli)に関するページを参照してください。 

#### <a name="create-and-execute-a-template"></a>テンプレートを作成して実行する

1. 以下の JSON 構文をコピーして、ファイルに貼り付けます。

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. このファイルを **existingClusterOnboarding.json** としてローカル フォルダーに保存します。
3. 以下の JSON 構文をファイルに貼り付けます。

    ```json
    {
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. **aksResourceId** および **aksResourceLocation** の値を、AKS クラスターの **[AKS Overview]\(AKS 概要\)** ページの値を使用して編集します。 **workspaceResourceId** の値は、ワークスペース名を含む Log Analytics ワークスペースの完全なリソース ID です。 また、**workspaceRegion** にワークスペースの場所を指定します。 
5. このファイルを **existingClusterParam.json** としてローカル フォルダーに保存します。
6. これでこのテンプレートをデプロイする準備が整いました。 

    * テンプレートが含まれているフォルダーで、次の PowerShell コマンドを使用します。

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ClusterResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        設定の変更が完了するまで数分かかります。 完了すると、次のような結果を含むメッセージが表示されます。

        ```powershell
        provisioningState       : Succeeded
        ```

    * Azure CLI を使用して次のコマンドを実行するには、次の手順を実行します。
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        設定の変更が完了するまで数分かかります。 完了すると、次のような結果を含むメッセージが表示されます。

        ```azurecli
        provisioningState       : Succeeded
        ```
監視を有効にした後、クラスターの正常性メトリックが表示されるまで、約 15 分かかる場合があります。 

## <a name="verify-agent-and-solution-deployment"></a>エージェントとソリューションのデプロイを確認する
バージョン *06072018* 以降のエージェントでは、エージェントとソリューションの両方が正常にデプロイされていることを確認することができます。 これより前のバージョンのエージェントでは、エージェントのデプロイしか確認できません。

### <a name="agent-version-06072018-or-later"></a>エージェント バージョン 06072018 以降
エージェントが正常にデプロイされていることを確認するには、次のコマンドを実行します。 

```
kubectl get ds omsagent --namespace=kube-system
```

出力は次のようになり、適切にデプロイされたことが示されます。

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

ソリューションのデプロイを確認するには、次のコマンドを実行します。

```
kubectl get deployment omsagent-rs -n=kube-system
```

出力は次のようになり、適切にデプロイされたことが示されます。

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>06072018 より前のバージョンのエージェント

*06072018* より前にリリースされたバージョンの Log Analytics エージェントが適切にデプロイされていることを確認するには、次のコマンドを実行します。  

```
kubectl get ds omsagent --namespace=kube-system
```

出力は次のようになり、適切にデプロイされたことが示されます。  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>CLI で構成を表示する
ソリューションが有効かどうか、Log Analytics ワークスペースのリソース ID、クラスターに関するサマリー詳細などの詳細を取得するには、`aks show` コマンドを使用します。  

```azurecli
az aks show -g <resoourceGroupofAKSCluster> -n <nameofAksCluster>
```

数分してコマンドが完了すると、ソリューションに関する情報が JSON 形式で表示されます。  コマンドの結果では監視アドオン プロファイルが表示され、次の出力例のようになります。

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```
## <a name="next-steps"></a>次の手順

* ソリューションのオンボードを試みた際に問題が発生した場合は、[トラブルシューティング ガイド](monitoring-container-insights-troubleshoot.md)を確認してください。

* AKS クラスター ノードとポッドの両方について正常性メトリックを取得するための監視が有効になったので、これらの正常性メトリックを Azure portal で利用できます。 コンテナー用 Azure Monitor を使用する方法については、[Azure Kubernetes Service の正常性の表示](monitoring-container-insights-analyze.md)に関するページをご覧ください。