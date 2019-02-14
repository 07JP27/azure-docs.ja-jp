---
title: チュートリアル:Azure Active Directory と Fidelity NetBenefits の統合 | Microsoft Docs
description: Azure Active Directory と Fidelity NetBenefits の間のシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf7de6d0416c60f33f57fd83768fd23b0a09e0f1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184875"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>チュートリアル:Azure Active Directory と Fidelity NetBenefits の統合

このチュートリアルでは、Fidelity NetBenefits と Azure Active Directory (Azure AD) を統合する方法について説明します。

Fidelity NetBenefits と Azure AD の統合には、次の利点があります。

- Fidelity NetBenefits にアクセスするユーザーを Azure AD 上でコントロールできます。
- ユーザーが自分の Azure AD アカウントで自動的に Fidelity NetBenefits にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Fidelity NetBenefits と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Fidelity NetBenefits でのシングル サインオンが有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[1 か月の評価版を入手できます](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。
このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの Fidelity NetBenefits の追加
2. Azure AD シングル サインオンの構成とテスト

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>ギャラリーからの Fidelity NetBenefits の追加

Azure AD への Fidelity NetBenefits の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Fidelity NetBenefits を追加する必要があります。

**ギャラリーから Fidelity NetBenefits を追加するには、次の手順に従います。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure Active Directory のボタン][1]

2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[エンタープライズ アプリケーション] ブレード][2]

3. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[新しいアプリケーション] ボタン][3]

4. 検索ボックスに「**Fidelity NetBenefits**」と入力し、結果ウィンドウで **[Fidelity NetBenefits]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![結果リストの Fidelity NetBenefits](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト

このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、Fidelity NetBenefits で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Fidelity NetBenefits ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Fidelity NetBenefits の関連ユーザーの間で、リンク関係が確立されている必要があります。

Fidelity NetBenefits で、**Azure AD ユーザー**を使用して**ユーザー** マッピングを実行し、リンク関係を確立します。

Fidelity NetBenefits で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configure-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[Fidelity NetBenefits のテスト ユーザーの作成](#create-a-fidelity-netbenefits-test-user)** - Fidelity NetBenefits で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - Britta Simon が Azure AD シングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#test-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にして、Fidelity NetBenefits アプリケーションでシングル サインオンを構成します。

**Fidelity NetBenefits で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure Portal の **Fidelity NetBenefits** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![シングル サインオン構成のリンク][4]

2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。

    ![[シングル サインオン] ダイアログ ボックス](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_samlbase.png)

3. **[Fidelity NetBenefits のドメインと URL]** セクションで、次の手順を実行します。

    ![[Fidelity NetBenefits のドメインと URL] のシングル サインオン情報](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_url.png)

    a. **[識別子]** ボックスに次の URL を入力します。

    テスト環境: `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    運用環境: `urn:sp:fidelity:geninbndnbparts20`

    b. **[応答 URL]** テキストボックスに、実装時に Fidelity から提供された URL を入力するか、担当の Fidelity Client Service Manager に問い合わせてください。

4. Fidelity NetBenefits アプリケーションは、特定の形式で構成された SAML アサーションを受け入れます。 **[ユーザー識別子]** は **user.userprincipalname** にマップされています。 これは **employeeid**、または**ユーザー識別子**として組織に適用できる他の要求をマップできます。 次のスクリーンショットは一例にすぎません。

    ![Fidelity NetBenefits 属性](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_attribute.png)

    >[!Note]
    >Fidelity NetBenefits では、静的および動的なフェデレーションがサポートされます。 静的の場合は、SAML ベースのジャスト イン タイムのユーザー プロビジョニングが使用されません。一方、動的の場合は、ジャスト イン タイムのユーザー プロビジョニングがサポートされます。 JIT ベースのプロビジョニングを使用する場合、お客様は、ユーザーの生年月日など、要求をいくつか Azure AD で追加する必要があります。こうした詳細は、**Fidelity Client Service Manager** から提供されます。インスタンスに対して、この動的なフェデレーションを有効にする必要があります。

5. **[SAML 署名証明書]** セクションで、**[Metadata XML (メタデータ XML)]** をクリックし、コンピューターにメタデータ ファイルを保存します。

    ![証明書のダウンロードのリンク](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_certificate.png)

6. **[保存]** ボタンをクリックします。

    ![[シングル サインオンの構成] の [保存] ボタン](./media/fidelitynetbenefits-tutorial/tutorial_general_400.png)

7. **[Fidelity NetBenefits 構成]** セクションで、**[Fidelity NetBenefits の構成]** をクリックして、**[サインオンの構成]** ウィンドウを開きます。 **[クイック リファレンス]** セクションから、**SAML エンティティ ID と SAML シングル サインオン サービス URL** をコピーします。

    ![Fidelity NetBenefits 属性](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_configure.png)

8. **Fidelity NetBenefits** 側でシングル サインオンを構成するには、ダウンロードした**メタデータ XML**、**SAML シングル サインオン サービス URL**、**SAML エンティティ ID** を、**担当の Fidelity Client Service Manager** に送る必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

   ![Azure AD のテスト ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. Azure Portal の左側のウィンドウで、**Azure Active Directory** のボタンをクリックします。

    ![Azure Active Directory のボタン](./media/fidelitynetbenefits-tutorial/create_aaduser_01.png)

2. ユーザーの一覧を表示するには、**[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックします。

    ![[ユーザーとグループ] と [すべてのユーザー] リンク](./media/fidelitynetbenefits-tutorial/create_aaduser_02.png)

3. **[ユーザー]** ダイアログ ボックスを開くには、**[すべてのユーザー]** ダイアログ ボックスの上部にある **[追加]** をクリックしてきます。

    ![[追加] ボタン](./media/fidelitynetbenefits-tutorial/create_aaduser_03.png)

4. **[ユーザー]** ダイアログ ボックスで、次の手順に従います。

    ![[ユーザー] ダイアログ ボックス](./media/fidelitynetbenefits-tutorial/create_aaduser_04.png)

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに、ユーザーである Britta Simon の電子メール アドレスを入力します。

    c. **[パスワードを表示]** チェック ボックスをオンにし、**[パスワード]** ボックスに表示された値を書き留めます。

    d.[Tableau Server return URL]: Tableau Server ユーザーがアクセスする URL。 **Create** をクリックしてください。
  
### <a name="create-a-fidelity-netbenefits-test-user"></a>Fidelity NetBenefits テスト ユーザーの作成

このセクションでは、Fidelity NetBenefits で Britta Simon というユーザーを作成します。 静的なフェデレーションを作成している場合、Fidelity NetBenefits プラットフォームでユーザーを作成するには、担当の **Fidelity Client Service Manager** と一緒に作業してください。 こうしたユーザーは、シングル サインオンを使用する前に作成し、アクティブにする必要があります。

動的なフェデレーションについては、ジャスト イン タイムプロビジョニングを使用してユーザーが作成されます。 JIT ベースのプロビジョニングを使用する場合、お客様は、ユーザーの生年月日など、要求をいくつか Azure AD で追加する必要があります。こうした詳細は、**Fidelity Client Service Manager** から提供されます。インスタンスに対して、この動的なフェデレーションを有効にする必要があります。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に Fidelity NetBenefits へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザー ロールを割り当てる][200]

**Fidelity NetBenefits に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201]

2. アプリケーションの一覧で **[Fidelity NetBenefits]** を選択します。

    ![アプリケーションの一覧の Fidelity NetBenefits のリンク](./media/fidelitynetbenefits-tutorial/tutorial_fidelitynetbenefits_app.png)  

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![[ユーザーとグループ] リンク][202]

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![[割り当ての追加] ウィンドウ][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。

### <a name="test-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで [Fidelity NetBenefits] タイルをクリックすると、自動的に Fidelity NetBenefits アプリケーションにサインオンします。
アクセス パネルの詳細については、[アクセス パネルの概要](../user-help/active-directory-saas-access-panel-introduction.md)に関するページを参照してください。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fidelitynetbenefits-tutorial/tutorial_general_01.png
[2]: ./media/fidelitynetbenefits-tutorial/tutorial_general_02.png
[3]: ./media/fidelitynetbenefits-tutorial/tutorial_general_03.png
[4]: ./media/fidelitynetbenefits-tutorial/tutorial_general_04.png

[100]: ./media/fidelitynetbenefits-tutorial/tutorial_general_100.png

[200]: ./media/fidelitynetbenefits-tutorial/tutorial_general_200.png
[201]: ./media/fidelitynetbenefits-tutorial/tutorial_general_201.png
[202]: ./media/fidelitynetbenefits-tutorial/tutorial_general_202.png
[203]: ./media/fidelitynetbenefits-tutorial/tutorial_general_203.png
