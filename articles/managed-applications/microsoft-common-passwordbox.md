---
title: Azure の PasswordBox UI 要素 | Microsoft Docs
description: Azure Portal の Microsoft.Common.PasswordBox UI 要素について説明します。
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 944f59da680c3a058a3cd245cca48d903e44ab87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "64710930"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI 要素
パスワードの提供と確認に使用できるコントロールです。

## <a name="ui-sample"></a>UI サンプル
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>スキーマ
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a>解説
- この要素は `defaultValue` プロパティをサポートしていません。
- `constraints` の実装の詳細については、[Microsoft.Common.TextBox](microsoft-common-textbox.md) に関する記事を参照してください。
- `options.hideConfirmation` が **true** に設定されている場合、ユーザーのパスワードを確認するための 2 つ目のテキスト ボックスは表示されません。 既定値は **false** です。

## <a name="sample-output"></a>サンプル出力
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>次の手順
* UI 定義の作成の概要については、「[CreateUiDefinition の基本概念](create-uidefinition-overview.md)」を参照してください。
* UI 要素の共通プロパティの説明については、「[CreateUiDefinition の要素](create-uidefinition-elements.md)」を参照してください。
