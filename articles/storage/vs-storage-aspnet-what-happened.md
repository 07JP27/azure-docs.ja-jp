---
title: ASP.NET プロジェクトの変更点 | Microsoft Docs
description: Visual Studio 接続済みサービスを使用して ASP.NET プロジェクトに Azure Storage を追加した後の変更点について説明します。
services: storage
documentationcenter: ''
author: TomArcher
manager: douge
editor: ''

ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: tarcher

---
# ASP.NET プロジェクトの変更点 (Visual Studio Azure Storage 接続済みサービス)
## リファレンスの追加
Visual Studio プロジェクトに Azure Storage の NuGet パッケージが追加されました。このパッケージは、次の .NET 参照を追加します。

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## Azure Storage の接続文字列の追加
選択されたストレージ アカウントの接続文字列とキーを使用して、プロジェクトの web.config ファイル内に要素が作成されました。

詳細については、「[ASP.NET](http://www.asp.net)」を参照してください。

<!---HONumber=AcomDC_0817_2016-->