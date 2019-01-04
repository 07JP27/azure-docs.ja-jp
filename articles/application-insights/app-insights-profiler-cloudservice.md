---
title: Application Insights でライブ Azure クラウド サービスをプロファイリングする | Microsoft Docs
description: Cloud Services に対して Application Insights Profiler を有効にする
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 28e21180fde50d19154830694cd4959795ae9d5c
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789383"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Application Insights でライブ Azure クラウド サービスをプロファイリングする

Application Insights Profiler を次のサービスにデプロイすることもできます。
* [Azure App Service](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric アプリケーション](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler は、Windows Azure 診断 (WAD) 拡張機能と共にインストールされています。 必要な作業は、プロファイラーをインストールし、Application Insights リソースにプロファイルを送信するように WAD を構成することだけです。

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Azure クラウド サービスに対してプロファイラーを有効にする
1. [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) 以降を使用していることを確認します。  *ServiceConfiguration.\*.cscfg* ファイルの `osFamily` 値が "5" 以上になっていることを確認すれば十分です。
1. [Application Insights SDK をクラウド サービス](../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json)に追加します。
1. Application Insights で要求を追跡します。

    ASP.Net Web ロールの場合、Application Insights で要求を自動的に追跡できます。

    Worker ロールの場合、[要求を追跡するコードを追加します](app-insights-profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)。

    

1. Windows Azure Diagnostics (WAD) 拡張機能を構成し、プロファイラーを有効にします。



    1. アプリケーション ロールで、[Azure 診断](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics)の *diagnostics.wadcfgx* ファイルを探します。  

       ![診断構成ファイルの場所](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

        ファイルが見つからない場合は、「[Azure クラウド サービスと仮想マシンに対する診断を設定する](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them)」で、Azure Cloud Services プロジェクトで診断拡張機能を有効にする方法を確認してください。

    1. `WadCfg` の子要素として次の `SinksConfig` セクションを追加します。  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    >   **注:** diagnostics.wadcfgx* ファイルに `ApplicationInsights` タイプの別のシンクが含まれている場合、次の 3 つのインストルメンテーション キーがすべて一致する必要があります。  
    >  * アプリケーションによって使用されるキー。  
    >  * `ApplicationInsights` シンクによって使用されるキー。  
    >  * `ApplicationInsightsProfiler` シンクによって使用されるキー。  
    >
    > `ApplicationInsights` シンクで実際に使用されているインストルメンテーション キーの値は、*ServiceConfiguration.\*.cscfg* ファイルで確認できます。  
    > Visual Studio 15.5 Azure SDK リリース以降は、アプリケーションと `ApplicationInsightsProfiler` シンクで使用されるインストルメンテーション キーのみが相互に一致する必要があります。
1. 新しい診断構成でサービスをデプロイすると、ご利用のサービスで Application Insights Profiler が実行されるように構成されます。
 
## <a name="next-steps"></a>次の手順

- アプリケーションへのトラフィックを生成します (たとえば、[可用性テスト](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)を起動します)。 その後、Application Insights インスタンスへのトレースの送信が開始されるまで 10 ～ 15 分待機します。
- Azure ポータルで [Profiler トレース](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json)を表示します。
- Profiler の問題をトラブルシューティングするための情報を [Profilerのトラブルシューティング](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)に関するセクションで探します。