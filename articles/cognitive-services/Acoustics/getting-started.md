---
title: Project Acoustics スタート ガイド
titlesuffix: Azure Cognitive Services
description: このクイックスタート ガイドでは、Unity プロジェクトにプラグインを統合し、シーンをベイクし、音響を音源に適用する方法について説明します。
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 0aad231b4a57c2d9927a553a43394681282b5494
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155954"
---
# <a name="getting-started-with-project-acoustics"></a>Project Acoustics スタート ガイド
このクイックスタート ガイドでは、Unity プロジェクトにプラグインを統合し、シーンをベイクし、音響を音源に適用する方法について説明します。 このクイックスタート用に、まず [Azure Batch アカウント](create-azure-account.md)を作成する必要があります。 このガイドでは、Unity に関して一定の知識があることを前提にしています。

## <a name="download-the-plugin"></a>プラグインのダウンロード
[こちら](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)から登録して Designer Preview に参加します。

## <a name="supported-platforms-for-quickstart"></a>クイックスタートでサポートされているプラットフォーム
* [Unity 2018.2 以上](http://www.unity3d.com)
  * プロジェクトのスクリプト ランタイム バージョンを **.NET 4.x 相当**に設定する必要があります 
  * Windows ベースの Unity エディターが必要です

## <a name="import-the-plugin"></a>プラグインのインポート
音響 UnityPackage をプロジェクトにインポートします。 
* Unity で、**[Assets] > [Import Package] > [Custom Package...]** を選択します。

![パッケージのインポート](media/ImportPackage.png)  

* **MicrosoftAcoustics.unitypackage** を選択します

プラグインを既存のプロジェクトにインポートする場合、C# コンパイラへのオプションを指定する **mcs.rsp** ファイルがプロジェクト ルートに既に存在している可能性があります。 このファイルの内容を、Project Acoustics プラグインに付属する mcs.rsp ファイルにマージする必要があります。

## <a name="enable-the-plugin"></a>プラグインの有効化
音響ツールキットのベイク部分には、.NET 4.x スクリプト ランタイム バージョンが必要です。 パッケージ インポートによって Unity Player の設定が更新されます。 この設定を有効にするには、Unity を再起動します。

![Player 設定](media/PlayerSettings.png)

![.NET 4.5](media/Net45.png)

## <a name="create-a-navigation-mesh"></a>ナビゲーション メッシュの作成
標準の [Unity ワークフロー](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html)を使用して、プロジェクトのナビゲーション メッシュを作成します。 独自のナビゲーション メッシュの使用方法については、[bake UI チュートリアル](bake-ui-walkthrough.md)を参照してください。

## <a name="mark-static-meshes-for-acoustics"></a>静的メッシュを音響用にマークする
Unity で **[Window] > [Acoustics]** を選択して音響ウィンドウを表示します。 このウィンドウはインスペクターの隣にドッキングできます。

![[Acoustics] ウィンドウを開く](media/WindowAcoustics.png)

Unity の階層ウィンドウで、選択されている項目をすべて選択解除します。 音響の **[Object]** タブで、[Acoustics Geometry] チェック ボックスをオンにして、シーン内のすべてのメッシュと地形に音響ジオメトリのマークを付けます。

**[Materials]** タブで、シーンで使用されているマテリアルに音響マテリアルを割り当てます。 **[Default]** マテリアルの吸音はコンクリート相当です。 独自のマテリアル プロパティの指定について、詳しくは[デザイン プロセスに関するページ](design-process.md)を参照してください。

![[Materials] タブ](media/MaterialsTab.png)

## <a name="preview-the-probes"></a>プローブのプレビュー
**[Probes]** タブで、**[Calculate]** をクリックします。 シーンのサイズによっては、この計算に数分かかる場合があります。 計算が完了すると、浮かんだ球がシーン ビューに表示され、音響シミュレーションの位置 ("プローブ ポイント" と呼ばれます) にマークが付きます。 シーン ウィンドウでオブジェクトに十分に近づくと、シーンのボクセル化も確認できます。 緑色のボクセルは、ジオメトリとしてマークを付けたオブジェクトにぴったり合っているはずです。 プローブ ポイントとボクセルの表示は、シーン ビューの右上にある [Gizmos] メニューで切り替えることができます。

![GizmosPreview](media/BakePreviewWithGizmos.png)

## <a name="bake-the-scene"></a>シーンのベイク
**[Bake]** タブで、Azure 資格情報を入力して **[Bake]** をクリックします。 Azure Batch アカウントを持っていない場合は、[推奨するアカウント設定に関するこちらのチュートリアル](create-azure-account.md)を参照してください。
ベイクが終了すると、プロジェクトの **Assets/AcousticsData** ファイルにデータ ファイルが自動的にダウンロードされます。

## <a name="set-up-audio-runtime-dsp"></a>オーディオ ランタイム DSP のセットアップ
Unity の spatializer (立体化) フレームワークで音響用のオブジェクト ランタイム DSP を埋め込み、それを HRTF ベースの立体化と統合します。 音響処理を有効化するには、**Microsoft Acoustics** の立体化に切り替えます。それには、**[Edit] > [Project Settings] > [Audio]** を選択し、**[Microsoft Acoustics]** をプロジェクト用の **[Spatializer Plugin]** として選択します。 また、**[DSP Buffer Size]** は必ず [Best Performance] に設定します。

![プロジェクト設定](media/ProjectSettings.png)  

![Project Acoustics 立体化](media/ChooseSpatializer.png)

オーディオ ミキサーを開きます (**[Window] > [Audio Mixer]**)。 少なくとも 1 つのミキサーと 1 つのグループがあることを確認します。 ない場合、**[Mixers]** の右側にある "+" ボタンをクリックします。 エフェクト セクションのチャンネル ストリップの下部で右クリックし、**[Microsoft Acoustics Mixer]** エフェクトを追加します。 同時にサポートされる Project Acoustics ミキサーは 1 つだけですので注意してください。

![オーディオ ミキサー](media/AudioMixer.png)

## <a name="set-up-the-acoustics-lookup-table"></a>音響ルックアップ テーブルの設定
プロジェクト パネルから **[Microsoft Acoustics]** プレハブをドラッグし、シーンにドロップします。

![Acoustics プレハブ](media/AcousticsPrefab.png)

**ProjectAcoustics** ゲーム オブジェクトをクリックして、そのインスペクター パネルに移動します。 ベイク結果の場所 (**Assets/AcousticsData** 内の .ACE ファイル) を指定します。それには、このファイルを Acoustics Manager スクリプトにドラッグ アンド ドロップするか、またはテキスト ボックスの隣にある丸ボタンをクリックします。

![Acoustics Manager](media/AcousticsManager.png)  

## <a name="apply-acoustics-to-sound-sources"></a>音源に音響を適用する
オーディオ ソースを作成します。 AudioSource のインスペクター パネルの下部にある **[Spatialize]** チェック ボックスをオンにします。 **[Spatial Blend]** がフル 3D に設定されていることを確認します。  

![オーディオ ソース](media/AudioSource.png)

## <a name="apply-post-bake-design"></a>ベイク後デザインの適用
スクリプト **AcousticsAdjust** をシーン内の音源にアタッチして、追加のソース デザイン パラメーターを有効化できます。それには、**[Add Component]** をクリックし、**[Scripts] > [Acoustics Adjust]** を選択します。

![AcousticsAdjust](media/AcousticsAdjust.png)

**Microsoft Acoustics Mixer** にもパラメーターがあります。 ベイク後デザインの詳細については、[デザイン パラメーター](design-process.md)を参照してください。

## <a name="next-steps"></a>次の手順
* [サンプル シーン](sample-walkthrough.md)を試す
* すべての[ベイク機能](bake-ui-walkthrough.md)について学ぶ
* より詳細な[デザイン パラメーター](design-process.md)を確認する

