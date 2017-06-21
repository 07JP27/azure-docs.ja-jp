---
title: "Azure Database for MySQL サーバーの復元方法 | Microsoft Docs"
description: "この記事では、Azure Portal を使用して Azure Database for MySQL サーバーを復元する方法について説明します。"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0603119da20e74b423072ce6afdb8c9f20830383
ms.contentlocale: ja-jp
ms.lasthandoff: 05/10/2017

---

# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Azure Portal を使用して Azure Database for MySQL サーバーのバックアップと復元を行う方法

## <a name="backup-happens-automatically"></a>自動バックアップ
Azure Database for MySQL を使用するとき、このデータベース サービスは 5 分ごとに自動でサービスのバックアップを行います。 

このバックアップは、Basic レベルでは 7 日間、Standard レベルでは 35 日間使用できます。 詳細については、[Azure Database for MySQL サービス レベル](concepts-service-tiers.md)に関するページをご覧ください。

自動バックアップ機能を使用して、過去の特定の時点までサーバーとそのサーバーのすべてのデータベースを新しいサーバーに復元できます。

## <a name="restore-in-the-azure-portal"></a>Azure Portal で復元する
Azure Database for MySQL では、過去の特定の時点までサーバーのコピーを新しいサーバーに復元できます。 この新しいサーバーを使用して、データを回復できます。 

たとえば、本日正午にテーブルが誤って削除された場合、正午前の時点まで復元し、削除されたテーブルを新しいサーバーのコピーから取得できます。

次の手順では、サンプルのサーバーを特定の時点まで復元します。

1. [Azure Portal](https://portal.azure.com/) にサインインします。

2. Azure Database for MySQL サーバーを検索します。 左側のウィンドウで、**[All resources (すべてのリソース)]** を選択し、一覧からサーバーを選択します。

3.  サーバーの概要ブレードの上部で、ツールバーの **[復元]** をクリックします。 [復元] ブレードが開きます。
![[復元] ボタンをクリックする](./media/howto-restore-server-portal/click-restore-button.png)

4. [復元] フォームに必要な情報を入力します。

- **[復元ポイント (UTC)]**: 復元する特定の時点の日付と時刻を選択します。 指定した時刻は UTC 形式であるため、現地時刻を UTC に変換する必要があります。
- **[新しいサーバーに復元]**: 既存サーバーの復元先である新しいサーバーの名前を入力します。
- **[場所]**: リージョンは自動でソース サーバーのリージョンが選択され、変更できません。
- **[価格レベル]**: 価格レベルはソース サーバーと同じ価格レベルが自動で選択されるため、ここでは変更できません。 
![PITR による復元](./media/howto-restore-server-portal/pitr-restore.png)

5. **[OK]** をクリックして、特定の時点までサーバーを復元します。 

6. 復元が完了したら、作成した新しいサーバーを検索して、データベースが想定どおりに復元されていることを確認します。

## <a name="next-steps"></a>次のステップ
- [Azure Database for MySQL の接続ライブラリ](concepts-connection-libraries.md)
