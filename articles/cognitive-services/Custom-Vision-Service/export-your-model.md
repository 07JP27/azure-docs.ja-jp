---
title: モデルをモバイルにエクスポートする - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: モバイル アプリケーションの作成で利用する目的でモデルをエクスポートする方法について説明します。
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 080a4691869c4deaf86f0e27961e80b60d439f05
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857248"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>モバイル デバイスで使用するためにモデルをエクスポートする

Custom Vision Service では、分類子をエクスポートし、オフラインで実行できます。 エクスポートした分類子をアプリケーションに組み込み、デバイス上でローカル実行すると、リアルタイムの分類が可能になります。 

Custom Vision Service は次のエクスポートに対応しています。

* __Android__ 向け __Tensorflow__。
* __iOS11__ 向け __CoreML__。
* __Windows ML__ 向け __ONNX__。
* Windows または Linux __コンテナー__。 コンテナーには、Tensorflow モデルと、Custom Vision Service API を使用するためのサービス コードが含まれています。 

> [!IMPORTANT]
> Custom Vision Service は__コンパクト__ ドメインのみをエクスポートします。 コンパクト ドメインで生成されたモデルは、モバイル デバイスのリアルタイム分類の制約に合わせて最適化されています。 コンパクト ドメインでビルドされた分類子は、トレーニング データの量が同じ標準ドメインより、わずかに正確性に欠けることがあります。
>
> 分類子を改善する方法については、[分類子の改善](getting-started-improving-your-classifier.md)に関するドキュメントを参照してください。

## <a name="convert-to-a-compact-domain"></a>コンパクト ドメインに変換する

> [!NOTE]
> このセクションの手順は、既存の分類子がコンパクト ドメインに設定されていない場合にのみ適用されます。
 
既存の分類子のドメインは次の手順で変換します。

1. [[Custom Vision]](https://customvision.ai) ページから、__[ホーム]__ アイコンを選択し、プロジェクトを一覧表示します。 [https://customvision.ai/projects](https://customvision.ai/projects) を使用してプロジェクトを表示することもできます。

    ![ホーム アイコンとプロジェクト一覧の画像](./media/export-your-model/projects-list.png)

2. プロジェクトを選択し、ページ右上にある__歯車__アイコンを選択します。

    ![歯車アイコンの画像](./media/export-your-model/gear-icon.png)

3. __[ドメイン]__ セクションで、__コンパクト__ ドメインを選択します。 __[変更の保存]__ を選択して変更を保存します。

    ![ドメイン選択の画像](./media/export-your-model/domains.png)

4. ページの上部から、__[Train]\(トレーニングする\)__ を選択し、新しいドメインを使用して再トレーニングします。

## <a name="export-your-model"></a>モデルをエクスポートする

再トレーニング後にモデルをエクスポートするには、次の手順を使用します。

1. **[パフォーマンス]** タブに移動し、__[エクスポート]__ を選択します。 

    ![エクスポート アイコンの画像](./media/export-your-model/export.png)

    > [!TIP]
    > __[エクスポート]__ エントリがない場合、選択されているイテレーションでコンパクト ドメインが使用されていません。 このページの __[イテレーション]__ セクションを利用し、コンパクト ドメインを使用しているイテレーションを選択し、__[エクスポート]__ を選択してください。

2. エクスポート形式を選択し、__[エクスポート]__ を選択してモデルをダウンロードします。

## <a name="next-steps"></a>次の手順

エクスポートしたモデルをアプリケーションに統合します。 次のようなサンプル アプリケーションをご利用いただけます。

* [エクスポートした CoreML モデルを iOS アプリケーションで使用](https://go.microsoft.com/fwlink/?linkid=857726)し、Swift で画像をリアルタイム分類するためのサンプル
* [エクスポートした CoreML モデルと Xamarin を使用](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)し、画像をリアルタイム分類するためのサンプル iOS アプリケーション 
* [エクスポートした Tensorflow モデルを Android アプリケーションで使用](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)し、Swift で画像をリアルタイム分類するためのサンプル 
* [Windows で Tensorflow モデルを使用する](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-model-python)
* [エクスポートした ONNX モデルを Windows Machine Learning で使用する](https://azure.microsoft.com/resources/samples/cognitive-services-onnx-customvision-sample/)ためのサンプル
