---
title: PIM を管理する他の管理者にアクセス権を付与する - Azure | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) を管理するためのアクセス権を他の管理に付与する方法について説明します。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d7d064d9be3b180985c343b8dffc20f274fae048
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163128"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>PIM を管理する他の管理者にアクセス権を付与する

組織の Azure AD Privileged Identity Management (PIM) を有効にするグローバル管理者は、自動的にロールの割り当てを取得し、PIM にアクセスします。 ただし、他のグローバル管理者も含めて、既定で書き込みアクセス権を得るユーザーは他にはいません。 その他のグローバル管理者、セキュリティ管理者、およびセキュリティ リーダーは、PIM に対する読み取り専用アクセス権を持ちます。 PIM へのアクセス権を付与するには、最初のユーザーが他のユーザーに **特権ロール管理者** ロールを割り当てます。

> [!NOTE]
> PIM を管理するには、Azure MFA が必要です。 Microsoft アカウントは Azure MFA に登録できないため、Microsoft アカウントでサインインしたユーザーは PIM にアクセスできません。

1 人のユーザーがロックアウトされたり、アカウントが削除されたりしたときのために、特権ロール管理者ロールを割り当てられたユーザーが常に少なくとも 2 人いるようにしてください。

## <a name="grant-access-to-manage-pim"></a>PIM を管理するアクセス権を付与する

1. [Azure Portal](https://portal.azure.com/) にサインインします。

1. **[Azure AD Privileged Identity Management]** を開きます。

1. **[Azure AD ディレクトリ ロール]** をクリックします。

1. **[ロール]** をクリックします。

    ![PIM Azure AD ディレクトリ ロール - ロール](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. **[特権ロール管理者]** ロールをクリックして、メンバー ページを開きます。

    ![特権ロール管理者 - メンバー](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. **[メンバーの追加]** をクリックして [管理されるメンバーの追加] ウィンドウを開きます。

1. **[メンバーの選択]** をクリックして、[メンバーの選択] ウィンドウを開きます。

    ![特権ロール管理者 - メンバーの選択](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. メンバーを選択し、**[選択]** をクリックします。

1. **[OK]** をクリックして、メンバーを **特権ロール管理者**ロールの対象にします。

    新しいロールを PIM の他のユーザーに割り当てると、そのユーザーは、ロールをアクティブ化する**対象**として自動的に構成されます。

1. メンバーを固定するには、特権ロール管理者メンバーの一覧でユーザーをクリックします。

1. **[詳細]** をクリックし、**[永続化]** をクリックして、割り当てを永続化します。

    ![特権ロール管理者 - 永続化](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. [PIM の使用を開始](pim-getting-started.md)するためのリンクをユーザーに送信します。

## <a name="remove-access-to-manage-pim"></a>PIM を管理するアクセス権を削除する

特権ロール管理者ロールからユーザーを削除する際は、割り当てられているユーザーの人数が削除後も少なくとも 2 人は存在するように、必ず事前に確認してください。

1. [Azure Portal](https://portal.azure.com/) にサインインします。

1. **[Azure AD Privileged Identity Management]** を開きます。

1. **[Azure AD ディレクトリ ロール]** をクリックします。

1. **[ロール]** をクリックします。

1. **[特権ロール管理者]** ロールをクリックして、メンバー ページを開きます。

1. 削除するユーザーの横にチェックマークを追加した後、**[メンバーの削除]** をクリックします。

    ![特権ロール管理者 - メンバーの削除](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. ロールからメンバーを削除するかどうかを確認するメッセージが表示されたら、**[はい]** をクリックします。

## <a name="next-steps"></a>次の手順

- [PIM の使用を開始する](pim-getting-started.md)
