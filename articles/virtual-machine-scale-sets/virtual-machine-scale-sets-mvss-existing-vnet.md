---
title: "Azure 仮想マシン スケール セット: 既存の仮想ネットワーク テンプレート | Microsoft Docs"
description: "既存の仮想ネットワークを使用するスケール セット テンプレートを作成する方法を説明します。"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/06/2017
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: ddb3e1789e49d138e744c2238679236134b69324
ms.lasthandoff: 03/09/2017


---

# <a name="about-this-article"></a>この記事の内容

この記事では、[実行可能な最小のスケール セット テンプレート](./virtual-machine-scale-sets-mvss-start.md)を変更して、新しい仮想ネットワークを作成するのではなく、既存の仮想ネットワークにデプロイする方法について説明します。

## <a name="modifying-the-minimum-viable-scale-set-to-deploy-into-an-existing-virtual-network"></a>実行可能な最小のスケール セットを変更して既存の仮想ネットワークにデプロイする

実行可能な最小のスケール セット テンプレートは[こちら](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json)で確認できます。また、スケール セットを既存の仮想ネットワークにデプロイするためのテンプレートは[こちら](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json)で確認できます。 このテンプレートを作成する際に使用する diff (`git diff master minimum-viable-scale-set`) を項目ごとに確認しましょう。

まず、`subnetId` パラメーターを追加します。 この文字列はスケール セット構成に渡されます。これにより、スケール セットは仮想マシンをデプロイする作成済みのサブネットを特定できるようになります。 この文字列は、`/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>` の形式で指定する必要があります。 たとえば、名前が `myvnet`、サブネットが `mysubnet`、リソース グループが `myrg`、サブスクリプションが `00000000-0000-0000-0000-000000000000` の既存の仮想ネットワークにスケール セットをデプロイする場合、subnetId は `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet` になります。

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

次に、`resources` 配列から仮想ネットワーク リソースを削除します。ここでは既存の仮想ネットワークを使用し、新しい仮想ネットワークをデプロイする必要はないためです。

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

テンプレートをデプロイする前に仮想ネットワークが既に存在するので、スケール セットの dependsOn 句を仮想ネットワークに指定する必要はありません。 したがって、これらの行を削除します。

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

最後に、ユーザーが設定した `subnetId` パラメーターを渡します (`resourceId` を使用して同じデプロイの vnet の ID は取得しません。これは、実行可能な最小のスケール セット テンプレートで実行されます)。

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>次のステップ

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

