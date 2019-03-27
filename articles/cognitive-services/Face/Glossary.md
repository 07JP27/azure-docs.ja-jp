---
title: 用語集 - Face API サービス
titleSuffix: Azure Cognitive Services
description: 用語集では、Face API サービスの使用中に目にする可能性のある用語について説明します。
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: ad3ea9b0cde16d5c9645e7eacee9ba7f9a08ed2d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859167"
---
# <a name="glossary"></a>用語集

## <a name="a"></a>A

#### <a name="attributes"></a>属性

属性は検出結果から省略可能です ([年齢](#Age-Attribute)、[性別](#Gender-Attribute)、[頭部姿勢](#Head-Pose-Attribute)、[髭](#Facial-Hair-Attribute)、[笑顔](#Smile-Attribute)など)。
これらはクエリ パラメーター returnFaceAttributes を指定することによって検出 API から取得できます。 属性は[顔 ID](#Face-ID) と[四角形](#Face-Rectangle)に加えて、選択した[顔](#Face)に関する追加情報を提供します。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="age-attribute"></a>年齢 (属性)

年齢は、特定の顔の年齢を表す[属性](#Attributes)の 1 つです。 年齢属性は検出結果から省略可能であり、returnFaceAttributes パラメーターを指定することによって検出要求で制御できます。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

## <a name="b"></a>b

## <a name="c"></a>C

#### <a name="candidate"></a>候補

候補は本質的には[識別](#Identification)結果 (たとえば、検出において特定された人物と信頼度レベル) です。 候補は [PersonID](#Person-ID) と[信頼度](#Confidence)で表され、その人物が高い信頼度レベルで特定されたことを示します。

詳しくは、ガイド「[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)」(顔 - 識別) をご覧ください。

#### <a name="confidence"></a>信頼度

信頼度は、[顔](#Face)や[人物](#Person)の類似性を数値化したもので、[特定](#Identification)や[検証](#Verification)において、検索、識別、検証された結果の類似性を示します。

詳細については、「[Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)」(顔 - 類似検索)、「[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)」(顔 - 識別)、「[Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)」(顔 - 検証) の各ガイドを参照してください。

## <a name="d"></a>D

#### <a name="detectionface-detection"></a>検出/顔検出

顔検出とは、イメージ内の顔を特定するアクションのことです。 ユーザーは要求にイメージをアップロードするか、イメージの URL を指定できます。 検出された顔は[顔 ID](#Face-ID) とともに返されます。顔 ID は Face API の一意の ID を示します。 四角形は、イメージ内の顔の位置 (ピクセル単位) のほか、各顔の省略可能な[属性](#Attributes) ([年齢](#Age-Attribute)、[性別](#Gender-Attribute)、[頭部姿勢](#Head-Pose-Attribute)、[髭](#Facial-Hair-Attribute)、[笑顔](#Smile-Attribute)など) を示します。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

## <a name="e"></a>E

## <a name="f"></a>F

#### <a name="face"></a>Face

顔とは、検出された顔に関連する、Face API から派生した結果を表す統一用語のことです。 最終的に、顔は統一 ID ([顔 ID](#Face-ID))、イメージ内の指定されたリージョン ([顔四角形](#Face-Rectangle))、および顔に関連する追加の属性 ([年齢](#Age-Attribute)、[性別](#Gender-Attribute)、ランドマーク、[頭部姿勢](#Head-Pose-Attribute)など) で表されます。 さらに、顔は検出から返される場合があります。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="face-api"></a>Face API

Face API は、顔検出および認証の最も高度なアルゴリズムを提供するクラウド ベースの API です。 Face API の主な機能は、属性による顔検出と顔の[認識](#Recognition)の 2 つのカテゴリに分類できます。

詳細については、[Face API の概要](./Overview.md)に関するページ、「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出)、「[Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)」(顔 - 類似検索)、「[Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)」(顔 - グループ)、「[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)」(顔 - 識別)、「[Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)」(顔 - 検証) の各ガイドを参照してください。

#### <a name="face-attributesfacial-attributes"></a>顔の属性

詳しくは、「[属性](#Attributes)」をご覧ください。

#### <a name="face-id"></a>顔 ID

顔 ID は検出結果から派生します。ここでは、文字列が [Face API](#Face-API) で[顔](#Face)を表します。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="face-landmarksfacial-landmarks"></a>顔のランドマーク

ランドマークは、顔のセマンティック ポイント (目、鼻、口など) である検出結果から省略可能です (次の図に示します)。 ランドマークは、ブール値 returnFaceLandmarks によって検出要求で制御できます。 returnFaceLandmarks が true に設定されると、返される顔にはランドマーク属性があります。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="face-rectangle"></a>顔四角形

顔四角形は検出結果から派生し、イメージ内の矩形 (左、上、幅、高さ) をピクセル単位で表します。 [顔](#Face)の左上隅 (左、上 ) の、幅と高さの隣は、x 軸 と y 軸でそれぞれ顔のサイズを示します。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="facial-hair-attribute"></a>髭 (属性)

髭は、対象の顔の髭の長さを表すのに使用される[属性](#Attributes)の 1 つです。 髭属性は検出結果から省略可能であり、returnFaceAttributes によって検出要求で制御できます。 returnFaceAttributes に 'facialHair' が含まれていると、返される顔には髭属性が指定されます。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="facelist"></a>FaceList

FaceList は [PersistedFace](#PersistedFace) のコレクションで、[類似検索](#Find-Similar)のユニットです。 FaceList には、[FaceList ID](#FaceList-ID) のほか、名前やユーザー データなどのその他の属性が付属します。

詳細については、「[FaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)」(FaceList - 作成)、「[FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c)」(FaceList - 取得) の各ガイドを参照してください。

#### <a name="facelist-id"></a>FaceList ID

FaceList ID は、[FaceList](#FaceList) の識別子として使用される、ユーザー指定の ID です。 FaceList ID は、サブスクリプション内で一意である必要があります。

詳細については、「[FaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)」(FaceList - 作成)、「[FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c)」(FaceList - 取得) の各ガイドを参照してください。

#### <a name="find-similar"></a>類似検索

この API は、顔のコレクションに基づいて類似の顔の検索/クエリに使用されます。 顔や顔のコレクションのクエリは、要求内で[顔 ID](#Face-ID) または [FceList ID](#FaceList-ID)/[LargeFaceList ID](#LargeFaceList-ID) で表されます。 返される結果は、[顔 ID](#Face-ID) または PersistedFace IDで表される、検索された類似の顔になります。

詳細については、「[Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)」(顔 - 類似検索)、「[LargeFaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)」(LargeFaceList - 作成)、「[FaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)」(FaceList - 作成) の各ガイドを参照してください。

## <a name="g"></a>G

#### <a name="gender-attribute"></a>性別 (属性)

性別は、対象の顔の性別を表すのに使用される[属性](#Attributes)の 1 つです。 性別属性は検出結果から省略可能であり、returnFaceAttributes によって検出要求で制御できます。 returnFaceAttributes に 'gender' が含まれていると、返される顔には性別属性が指定されます。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="grouping"></a>グループ化

顔のグループ化とは、顔の類似性に基づいて顔のコレクションをグループ化することです。 顔のコレクションは、その要求内で顔 ID のコレクションによって示されます。 グループ化の結果、類似の顔が一緒の[グループ](#Groups)にグループ化され、他のどの顔にも類似していない顔は、不揃いのグループにマージされます。 グループ化の結果には、最大で 1 つの[不揃いのグループ](#Messy-Group)が存在します。

詳しくは、ガイド「[Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)」(顔 - グループ) をご覧ください。

#### <a name="groups"></a>グループ

グループは、[グループ化](#Grouping)の結果から派生します。 各グループには類似の顔が含まれ、顔は[顔 ID](#Face-ID) によって示されます。

詳しくは、ガイド「[Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)」(顔 - グループ) をご覧ください。

## <a name="h"></a>H

#### <a name="head-pose-attribute"></a>頭部姿勢 (属性)

頭部姿勢は、次の図に示すように、ロール、ピッチ、ヨー角に基づいて、3D 空間における顔の向きを表す[属性](#Attributes)の 1 つです。 ロールとヨー角の値の範囲は、[-180, 180] と [-90, 90] です。 最新バージョンでは、検出から返されるピッチの値は常に 0 です。 頭部姿勢属性は検出結果から省略可能であり、returnFaceAttributes パラメーターによって検出要求で制御できます。 returnFaceAttributes パラメーターに 'headPose' が含まれていると、返される顔には頭部姿勢属性が指定されています。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="identification"></a>識別

識別とは、LargePersonGroup/PersonGroup から 1 つ以上の顔を識別することです。
[PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup) は、[人物](#Person)のコレクションです。
顔と LargePersonGroup/PersonGroup は、その要求内でそれぞれ[顔 ID ](#Face-ID)と [LargePersonGroup ID](#LargePersonGroup-ID)/[PersonGroup ID](#PersonGroup-ID) によって表されます。
識別された結果は、信頼度とともに[人物](#Person)で表される[候補](#Candidate)です。
入力内の複数の顔は別個のものと見なされ、各顔にはそれぞれ独自の識別された結果を持ちます。

> [!NOTE]
> LargePersonGroup/PersonGroup は、識別の前に正常にトレーニングが行われている必要があります。 LargePersonGroup/PersonGroup がトレーニングを受けていない場合や、トレーニングの[状態](#Status-Train)が 'succeeded' と表示されていない (つまり、'running'、'failed'、または 'timeout' と表示されている) 場合、要求の応答は 400 になります。
> 

詳細については、「[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)」(顔 - 識別)、「[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)」(LargePersonGroup の人物 - 作成)、「[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)」(LargePersonGroup - 作成)、「[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)」(LargePersonGroup - トレーニング)、「[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)」(PersonGroup の人物 - 作成)、「[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)」(PersonGroup - 作成)、「[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)」(PersonGroup - トレーニング) の各ガイドを参照してください。

#### <a name="isidentical"></a>IsIdentical

IsIdentical は[検証](#Verification)結果のブール値フィールドで、2 つの顔が同じ人物のものであるかどうかを示します。

詳しくは、ガイド「[Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)」(顔 - 検証) をご覧ください。

## <a name="j"></a>J

## <a name="k"></a>K

## <a name="l"></a>L

#### <a name="landmarks"></a>ランドマーク

「顔のランドマーク」を参照してください。

#### <a name="largefacelist"></a>LargeFaceList

LargeFaceList は [PersistedFace](#PersistedFace) のコレクションで、[類似検索](#Find-Similar)のユニットです。 LargeFaceList には、[LargeFaceList ID](#LargeFaceList-ID) のほか、名前やユーザー データなどのその他の属性が付属します。

詳細については、「[LargeFaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)」(LargeFaceList - 作成)、「[LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce)」(LargeFaceList - 取得)、「[LargeFaceList - List Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6)」(FaceList - 顔のリスト) の各ガイドを参照してください。

#### <a name="largefacelist-id"></a>LargeFaceList ID

LargeFaceList ID は、[LargeFaceList](#LargeFaceList) の識別子として使用される、ユーザー指定の ID です。 LargeFaceList ID は、サブスクリプション内で一意である必要があります。

詳細については、「[LargeFaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)」(LargeFaceList - 作成)、「[LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce)」(LargeFaceList - 取得) の各ガイドを参照してください。

#### <a name="largepersongroup"></a>LargePersonGroup

LargePersonGroup は[人物](#Person)のコレクションで、[識別](#Identification)のユニットです。 LargePersonGroup には、[LargePersonGroup ID](#LargePersonGroup-ID) のほか、名前やユーザー データなどのその他の属性が付属します。

詳細については、「[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)」(LargePersonGroup - 作成)、「[LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e)」(LargePersonGroup - 取得)、「[LargePersonGroup Person - List](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1)」(LargePersonGroup の人物 - リスト) の各ガイドを参照してください。

#### <a name="largepersongroup-id"></a>LargePersonGroup ID

LargePersonGroup ID は、[LargePersonGroup](#LargePersonGroup) の識別子として使用される、ユーザー指定の文字列です。 LargePersonGroup ID は、サブスクリプション内で一意である必要があります。

詳細については、「[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)」(LargePersonGroup - 作成)、「[LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e)」(LargePersonGroup - 取得) の各ガイドを参照してください。

## <a name="m"></a>M

#### <a name="messy-group"></a>不揃いのグループ

不揃いのグループは[グループ化](#Grouping)の結果から派生し、他のどの顔とも類似していない顔が含まれます。 不揃いのグループ内の顔はそれぞれ[顔 ID](#Face-ID) で示されます。

詳しくは、ガイド「[Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)」(顔 - グループ) をご覧ください。

## <a name="n"></a>N

#### <a name="name-person"></a>名前 (人物)

名前は、[人物](#Person)をわかりやすく説明する文字列です。 [人物 ID](#Person-ID) とは異なり、人物の名前はグループ内で重複してかまいません。

詳細については、「[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)」(LargePersonGroup の人物 - 作成)、「[LargePersonGroup Person - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0)」(LargePersonGroup の人物 - 取得)、「[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)」(PersonGroup の人物 - 作成)、「[PersonGroup Person - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f)」(PersonGroup の人物 - 取得) の各ガイドを参照してください。

#### <a name="name-largepersongrouppersongroup"></a>名前 (LargePersonGroup/PersonGroup)

名前は、[LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup) をわかりやすく説明する文字列です。 [LargePersonGroup ID](#LargePersonGroup-ID)/[PersonGroup ID](#PersonGroup-ID) とは異なり、LargePersonGroup/PersonGroup の名前はサブスクリプション内で重複してかまいません。

詳細については、「[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)」(LargePersonGroup - 作成)、「[LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e)」(LargePersonGroup - 取得)、「[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)」(PersonGroup - 作成)、「[PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246)」(PersonGroup - 取得) の各ガイドを参照してください。

## <a name="o"></a>O

## <a name="p"></a>P

#### <a name="persistedface"></a>PersistedFace

PersistedFace は、Face API のデータ構造です。 PersistedFace には、PersistedFace ID のほか、名前やユーザー データなどのその他の属性が付属します。

詳細については、「[LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)」(LargeFaceList - 顔の追加)、「[FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)」(FaceList - 顔の追加)、「[LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)」(LargePersonGroup の人物 - 顔の追加)、「[PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)」(PersonGroup の人物 - 顔の追加) の各ガイドを参照してください。

#### <a name="person-id"></a>人物 ID

人物 ID は、[PersistedFace](#PersistedFace) が正常に作成されると生成されます。 [Face API](#Face-API) でこの顔を表す文字列が作成されます。

詳細については、「[LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)」(LargeFaceList - 顔の追加)、「[FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)」(FaceList - 顔の追加)、「[LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)」(LargePersonGroup の人物 - 顔の追加)、「[PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)」(PersonGroup の人物 - 顔の追加) の各ガイドを参照してください。

#### <a name="person"></a>Person

人物は、Face API で管理されるデータ構造です。 人物には、[人物 ID](#Person-ID) のほか、名前、[PersistedFace](#PersistedFace) のコレクション、ユーザー データなどのその他の属性が付属します。

詳細については、「[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)」(LargePersonGroup の人物 - 作成)、「[LargePersonGroup Person - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0)」(LargePersonGroup の人物 - 取得)、「[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)」(PersonGroup の人物 - 作成)、「[PersonGroup Person - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f)」(PersonGroup の人物 - 取得) の各ガイドを参照してください。

#### <a name="person-id"></a>人物 ID

人物 ID は、[人物](#Person)が正常に作成されると生成されます。 [Face API](#Face-API) でこの人物を表す文字列が作成されます。

詳細については、「[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)」(LargePersonGroup の人物 - 作成)、「[LargePersonGroup Person - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0)」(LargePersonGroup の人物 - 取得)、「[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)」(PersonGroup の人物 - 作成)、「[PersonGroup Person - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f)」(PersonGroup の人物 - 取得) の各ガイドを参照してください。

#### <a name="persongroup"></a>PersonGroup

PersonGroup は[人物](#Person)のコレクションで、[識別](#Identification)のユニットです。 PersonGroup には、[PersonGroup ID](#PersonGroup-ID) のほか、名前やユーザー データなどのその他の属性が付属します。

詳細については、「[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)」(PersonGroup - 作成)、「[PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246)」(PersonGroup - 取得)、「[PersonGroup Person - List](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241)」(PersonGroup の人物 - リスト) の各ガイドを参照してください。

#### <a name="persongroup-id"></a>PersonGroup ID

PersonGroup ID は、[PersonGroup](#PersonGroup) の識別子として使用される、ユーザー指定の文字列です。 グループ ID は、サブスクリプション内で一意である必要があります。

詳細については、「[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)」(PersonGroup - 作成)、「[PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246)」(PersonGroup - 取得) の各ガイドを参照してください。

#### <a name="pose-attribute"></a>姿勢 (属性)

「[頭部姿勢](#Head-Pose-Attribute)」をご覧ください。

## <a name="q"></a>Q

## <a name="r"></a>R

#### <a name="recognition"></a>認識

認識は、[類似検索](#Find-Similar)、[グループ化](#Grouping)、[識別](#Identification)、[2 つの顔が同じであるかどうかの検証](#Verification)など、顔認識テクノロジにおいて一般的なアプリケーション領域です。

詳細については、「[Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)」(顔 - 類似検索)、「[Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)」(顔 - グループ)、「[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)」(顔 - 識別)、「[Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)」(顔 - 検証) の各ガイドを参照してください。

#### <a name="rectangle-face"></a>四角形 (顔)

「[顔四角形](#Face-Rectangle)」をご覧ください。

## <a name="s"></a>S

#### <a name="similar-face-searching"></a>類似した顔の検索

「[類似検索](#Find-Similar)」をご覧ください。

#### <a name="smile-attribute"></a>笑顔 (属性)

笑顔は、対象の顔の笑顔を表すのに使用される[属性](#Attributes)の 1 つです。 笑顔属性は検出結果から省略可能であり、returnFaceAttributes によって検出要求で制御できます。 returnFaceAttributes に 'smile' が含まれていると、返される顔には笑顔属性が指定されます。

詳しくは、ガイド「[Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)」(顔 - 検出) をご覧ください。

#### <a name="snapshot"></a>スナップショット

スナップショットは、特定の顔データ型のための一時的なリモート ストレージです。 これは、あるサブスクリプションから別のサブスクリプションにデータをコピーするための一種のクリップボードとして機能します。 最初に、ユーザーはソース サブスクリプション内のデータのスナップショットを "取得" し、次にそれをターゲット サブスクリプション内の新しいデータ オブジェクトに "適用" します。 

詳細については、[顔の移行ガイド](./face-api-how-to-topics/how-to-migrate-face-data.md)のほか、「[Snapshot - Take](https://docs.microsoft.com/rest/api/cognitiveservices/face/snapshot/take)」(スナップショット - 作成) および「[Snapshot - Apply](https://docs.microsoft.com/rest/api/cognitiveservices/face/snapshot/apply)」(スナップショット - 適用) のリファレンス ドキュメント (REST) を参照してください。

#### <a name="status-train"></a>状態 (トレーニング)

状態は、'notstarted'、'running'、'succeeded'、'failed' などの、LargeFaceList/LargePersonGroups/PersonGroups をトレーニングするための手順を記述するために使用される文字列です。

詳しくは、「[LargeFaceList - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1)」(LargeFaceList - トレーニング)、「[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)」(LargePersonGroup - トレーニング)、「[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)」(PersonGroup - トレーニング) の各ガイドをご覧ください。

#### <a name="subscription-key"></a>Subscription key

サブスクリプション キーは、Face API を呼び出すために、クエリ文字列パラメーターとして指定する必要がある文字列です。 サブスクリプション キーは、Microsoft Cognitive Services ポータルにサインインした後に、[個人用サブスクリプション] ページで確認できます。 各サブスクリプションには、主キー 1 つと 2 次キー 1 つの 2 つのキーが関連付けられています。 両方とも API を同じように呼び出すことができます。 サブスクリプション キーは安全に保管する必要があります。また、サブスクリプション キーは [個人用サブスクリプション] ページでいつでも再生成できます。

## <a name="t"></a>T

#### <a name="train-largefacelistlargepersongrouppersongroup"></a>トレーニング (LargeFaceList/LargePersonGroup/PersonGroup)

この API は、[Find Similar](#Find-Similar)/[Identification](#Identification) のパフォーマンスを確保するために、[LargeFaceList](#LargeFaceList)/[LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup) を事前処理するために使用されます。 トレーニングを行わない、または[トレーニングの状態](#Status-Train)が成功と表示されていない場合、この PersonGroup の識別は失敗します。

詳細については、「[LargeFaceList - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1)」(LargeFaceList - トレーニング)、「[LargePersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)」(LargePersonGroup - トレーニング)、「[PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)」(PersonGroup - トレーニング)、「[Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)」(顔 - 識別) の各ガイドを参照してください。

## <a name="u"></a>U

#### <a name="userdatauser-data"></a>UserData/ユーザー データ

ユーザー データは、[人物](#Person)や [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup) に関連付けられている追加情報です。 ユーザー データはユーザーが設定することで、データの使用、理解、記憶がより簡単になります。

詳細については、「[LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)」(LargePersonGroup - 作成)、「[LargePersonGroup - Update](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f)」(LargePersonGroup - 更新)、「[LargePersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)」(LargePersonGroup の人物 - 作成)、「[LargePersonGroup Person - Update](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41)」(LargePersonGroup の人物 - 更新)、「[PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)」(PersonGroup - 作成)、「[PersonGroup - Update](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a)」(PersonGroup - 更新)、「[PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)」(PersonGroup の人物 - 作成)、「[PersonGroup Person - Update](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242)」(PersonGroup の人物 - 更新) の各ガイドを参照してください。

## <a name="v"></a>V

#### <a name="verification"></a>確認

この API は、2 つの顔が同じであるかどうかを検証します。 両方の顔は、その要求内で顔 ID として表されます。 検証された結果には、true の場合は同じであることを示すブール値フィールド (isIdentical) と、信頼のレベルを示す数値フィールド ([confidence](#Confidence)) が含まれています。

詳しくは、ガイド「[Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)」(顔 - 検証) をご覧ください。

## <a name="w"></a>W

## <a name="x"></a>X

## <a name="y"></a>Y

## <a name="z"></a>Z
