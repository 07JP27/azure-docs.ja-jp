---
title: インクルード ファイル
description: インクルード ファイル
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/22/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 0cfa0fdb51969c92e767adfa86a0065d11da56e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237744"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) は、[Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/overview.md) を使用した、SMB (Server Message Block) 上の ID ベースの認証をサポートします。 ドメインに参加している Windows 仮想マシン (VM) は、[Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md) の資格情報を使用して Azure ファイル共有にアクセスできます。 

Azure AD は、[ロールベースのアクセス制御 (RBAC)](../articles/role-based-access-control/overview.md) を使用して、ユーザー、グループ、サービス プリンシパルなどの ID を認証します。 Azure Files へのアクセスに使用されるアクセス許可の共通セットを含むカスタムの RBAC ロールを定義することができます。 カスタムの RBAC ロールを Azure AD ID に割り当てると、その ID には、それらのアクセス許可に従って Azure ファイル共有へのアクセス権が与えられます。

プレビューの一環として、Azure Files は、ファイル共有内のすべてのファイルとディレクトリに対する [NTFS DACL](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) の保持、継承、および適用もサポートしています。 ファイル共有から Azure Files に (または逆方向に) データをコピーする場合、NTFS DACL が保持されるように指定することができます。 このように、Azure Files を使用してバックアップ シナリオを実装し、オンプレミスのファイル共有とクラウドのファイル共有の間で NTFS DACLS を保持することができます。 

> [!NOTE]
> - SMB 経由の Azure AD 認証は、プレビュー リリースの Linux VM ではサポートされていません。 サポートされているのは Windows Server VM のみです。
> - SMB 経由の Azure AD 認証は、Azure Files にアクセスするオンプレミスのマシンではサポートされません。
> - Azure AD 認証は、2018 年 9 月 24 日よりも後に作成されたストレージ アカウントにのみ使用できます。
> - SMB と永続的 NTFS ACL 経由の Azure AD 認証は、Azure File Sync サービスによって管理されている Azure ファイル共有でサポートされていません。 
