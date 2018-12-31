---
title: 最小限のダウンタイムでの Azure Database for PostgreSQL への移行
description: この記事では、Azure Database Migration Service を使用して、PostgreSQL データベースを Azure Database for PostgreSQL に最小限のダウンタイムで移行する方法について説明します。
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 0c8c3443a19c26dade9699560e883969d3c074df
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53010843"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>最小限のダウンタイムでの Azure Database for PostgreSQL への移行
[Azure Database Migration Service](https://aka.ms/get-dms) (DMS) に新たに導入された**継続的同期機能**を使用すると、最小限のダウンタイムで PostgreSQL を Azure Database for PostgreSQL に移行できます。 この機能で、アプリケーションによって発生するダウンタイムの長さが短くなります。

## <a name="overview"></a>概要
Azure DMS では、オンプレミスから Azure Database for PostgreSQL への初期読み込みを実行してから、アプリケーションは実行中のままで、新しいトランザクションを Azure に継続的に同期します。 データがターゲットの Azure 側に追いついた後、アプリケーションを短時間停止し (最小限のダウンタイム)、データの最後のバッチがターゲットに追いつくまで (アプリケーションを停止してから、アプリケーションが事実上新しいトラフィックを受け取ることができなくなるまで) 待ってから、Azure を指すように接続文字列を更新します。 完了すると、アプリケーションは Azure 上で動作します。

![Azure Database Migration Service での継続的同期](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>次の手順
- [Microsoft Azure でのアプリケーションの最新化](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102)のビデオをご覧ください。PostgreSQL アプリを Azure Database for PostgreSQL に移行する方法のデモが含まれています。
- 「[DMS を使用して PostgreSQL をオンラインで Azure Database for PostgreSQL に移行する](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online)」のチュートリアルを参照してください。
