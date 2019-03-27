---
title: REST API を使用して Azure リソースの拒否割り当てを一覧表示する - Azure | Microsoft Docs
description: Azure リソースのロールベースのアクセス制御 (RBAC) と REST API を使用してユーザー、グループ、アプリケーションの拒否割り当てを一覧表示する方法について説明します。
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 29b8e0953109238b724cc8df9f456706f71a041e
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341625"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>REST API を使用して Azure リソースの拒否割り当てを一覧表示する

現在、拒否割り当ては**読み取り専用**であり、Azure によってのみ設定されます。 独自の拒否割り当てを作成することはできませんが、有効なアクセス許可に影響を与える可能性があるので、拒否割り当てを一覧表示することはできます。 この記事では、RBAC と REST API を使用して拒否割り当てを一覧表示する方法について説明します。

## <a name="list-a-single-deny-assignment"></a>1 つの拒否割り当てを一覧表示する

1. 次の要求から開始します。

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI の *{scope}* を、拒否割り当てを一覧表示するスコープに変更します。

    | Scope (スコープ) | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | サブスクリプション |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | リソース グループ |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | リソース |

1. *{deny-assignment-id}* を、取得する拒否割り当て識別子に置き換えます。

## <a name="list-multiple-deny-assignments"></a>複数の拒否割り当てを一覧表示する

1. 次のいずれかの要求から開始します。

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    省略可能なパラメーターを使用します。

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. URI の *{scope}* を、拒否割り当てを一覧表示するスコープに変更します。

    | Scope (スコープ) | type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | サブスクリプション |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | リソース グループ |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | リソース |

1. *{filter}* には、拒否割り当て一覧をフィルター処理するために適用する条件を指定します。

    | filter | 説明 |
    | --- | --- |
    | (フィルターなし) | 指定されたスコープおよびその上下の拒否割り当てをすべて一覧表示します。 |
    | `$filter=atScope()` | 指定されたスコープ以上の拒否割り当てのみを一覧表示します。 サブスコープの拒否割り当ては含まれません。 |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | 指定された名前の拒否割り当てを一覧表示します。 |

## <a name="list-deny-assignments-at-the-root-scope-"></a>指定されたルート スコープ (/) の拒否割り当てを一覧表示する

1. 「[Azure Active Directory で全体管理者のアクセス権を昇格する](elevate-access-global-admin.md)」の説明に従って、アクセス権を昇格します。

1. 次の要求を使用します。

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. *{filter}* には、拒否割り当て一覧をフィルター処理するために適用する条件を指定します。 フィルターが必要です。

    | filter | 説明 |
    | --- | --- |
    | `$filter=atScope()` | 指定されたルート スコープの拒否割り当てのみを一覧表示します。 サブスコープの拒否割り当ては含まれません。 |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | 指定された名前の拒否割り当てを一覧表示します。 |

1. 昇格されたアクセス権を削除します。

## <a name="next-steps"></a>次の手順

- [Azure リソースの拒否割り当ての概要](deny-assignments.md)
- [Azure Active Directory の全体管理者のアクセス権を昇格する](elevate-access-global-admin.md)
- [Azure REST API リファレンス](/rest/api/azure/)
