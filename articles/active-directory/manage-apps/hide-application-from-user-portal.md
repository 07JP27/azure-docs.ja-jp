---
title: Azure Active Directory でユーザーのエクスペリエンスからアプリケーションを非表示にする | Microsoft Docs
description: Azure Active Directory アクセス パネルまたは Office 365 ランチャーでユーザーのエクスペリエンスからアプリケーションを非表示にする方法
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: barbkess
ms.reviewer: kasimpso
ms.openlocfilehash: d5443f5b33f85d7a4b1e69cecc6bcdf68859abf4
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474908"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>Azure Active Directory のエンドユーザーに対してアプリケーションを非表示にする

エンドユーザーの MyApps パネルや Office 365 ランチャーにアプリケーションが表示されないようにする方法について説明します。 アプリケーションを非表示にしても、アプリケーションに対するユーザーのアクセス許可は維持されます。 

## <a name="prerequisites"></a>前提条件

MyApps パネルと Office 365 ランチャーにアプリケーションが表示されないようにするには、アプリケーション管理者特権が必要です。

すべての Office 365 アプリケーションを非表示にするは、グローバル管理者特権が必要です。


## <a name="hide-an-application-from-the-end-user"></a>エンドユーザーに対してアプリケーションを非表示にする
MyApps パネルと Office 365 アプリケーション ランチャーにアプリケーションが表示されないようにするには、次の手順を使用します。

1.  ディレクトリのグローバル管理者として [Azure portal](https://portal.azure.com) にサインインします。
2.  **[Azure Active Directory]** を選択します。
3.  **[エンタープライズ アプリケーション]** を選択します。 **[Enterprise applications - All applications (エンタープライズ アプリケーション - すべてのアプリケーション)]** ブレードが開きます。
4.  **[アプリケーションの種類]** で、**[エンタープライズ アプリケーション]** を選択します (まだ選択されていない場合)。
5.  非表示にするアプリケーションを検索し、アプリケーションをクリックします。  アプリケーションの概要が表示されます。
6.  **[プロパティ]** をクリックします。 
7.  **[ユーザーに表示しますか?]** という質問に対し、**[いいえ]** をクリックします。
8.  **[Save]** をクリックします。


## <a name="hide-office-365-applications-from-the-myapps-panel"></a>MyApps パネルに Office 365 アプリケーションが表示されないようにする

MyApps パネルに一切の Office 365 アプリケーションが表示されないようにするには、次の手順を使用します。 なお、アプリケーションは Office 365 ポータルには表示されます。

1.  ディレクトリのグローバル管理者として [Azure portal](https://portal.azure.com) にサインインします。
2.  **[Azure Active Directory]** を選択します。
3.  **[ユーザー設定]** を選択します。
4.  **[エンタープライズ アプリケーション]** で、**[エンド ユーザーがアプリケーションを起動して表示する方法を管理する]** をクリックします。
5.  **[ユーザーは Office 365 ポータルでのみ Office 365 アプリを表示できる]** で、**[はい]** をクリックします。
6.  **[Save]** をクリックします。


## <a name="next-steps"></a>次の手順
* [自分のグループをすべて表示する](../fundamentals/active-directory-groups-view-azure-portal.md)
* [エンタープライズ アプリケーションにユーザーまたはグループを割り当てる](assign-user-or-group-access-portal.md)
* [エンタープライズ アプリケーションからユーザーまたはグループの割り当てを削除する](remove-user-or-group-access-portal.md)
* [エンタープライズ アプリケーションの名前またはロゴを変更する](change-name-or-logo-portal.md)

