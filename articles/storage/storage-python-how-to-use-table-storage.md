---
title: "Python で Azure Table Storage を使用する方法 | Microsoft Docs"
description: "NoSQL データ ストアである Azure Table Storage を使用して構造化データをクラウドに格納します。"
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: marsma
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: c310a52182bbc3cf44ed4dc6a04e97aa59200a64
ms.contentlocale: ja-jp
ms.lasthandoff: 05/12/2017


---
# Python で Table Storage を使用する方法
<a id="how-to-use-table-storage-in-python" class="xliff"></a>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

このガイドでは、[Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python) を使用して、Python で Azure Table Storage の一般的なシナリオを実行する方法を説明します。 紹介するシナリオは、テーブルの作成と削除、およびエンティティの挿入とクエリ実行などです。

このチュートリアルでシナリオに従って作業する際に、[Azure Storage SDK for Python API のリファレンス](https://azure-storage.readthedocs.io/en/latest/index.html)を参照できます。

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Microsoft Azure Storage SDK for Python をインストールする
<a id="install-the-azure-storage-sdk-for-python" class="xliff"></a>

ストレージ アカウントを作成したら、次の手順は [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python) をインストールすることです。 SDK のインストールの詳細については、GitHub の Storage SDK for Python リポジトリ内の [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) を参照してください。

## テーブルを作成する
<a id="create-a-table" class="xliff"></a>

Python で Azure テーブル サービスを使用するには、[TableService][py_TableService] モジュールをインポートする必要があります。 テーブル エンティティを操作するので、[Entity][py_Entity] クラスも必要です。 次のコードを Python ファイルの先頭付近に追加して、両方をインポートします。

```python
from azure.storage.table import TableService, Entity
```

ストレージ アカウント名とアカウント キーを渡して、[TableService][py_TableService] オブジェクトを作成します。 `myaccount` と `mykey` を自分のアカウント名とキーで置き換え、[create_table][py_create_table] を呼び出して、Azure Storage にテーブルを作成します。

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## エンティティをテーブルに追加する
<a id="add-an-entity-to-a-table" class="xliff"></a>

エンティティを追加するには、最初にエンティティを表すオブジェクトを作成してから、そのオブジェクトを [TableService][py_TableService].[insert_entity][py_insert_entity] メソッドに渡します。 エンティティ オブジェクトには、ディクショナリまたは [Entity][py_Entity] 型のオブジェクトが可能で、エンティティのプロパティの名前と値を定義します。 エンティティに定義するその他のプロパティの他に、すべてのエンティティに必須の [PartitionKey と RowKey](#partitionkey-and-rowkey) のプロパティを含める必要があります。

この例では、エンティティを表すディクショナリ オブジェクトを作成し、それを [insert_entity][py_insert_entity] メソッドに渡してテーブルに追加します。

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

この例では、[Entity][py_Entity] オブジェクトを作成し、それを [insert_entity][py_insert_entity] メソッドに渡してテーブルに追加します。

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### PartitionKey と RowKey
<a id="partitionkey-and-rowkey" class="xliff"></a>

すべてのエンティティに **PartitionKey** と **RowKey** を指定する必要があります。 これらは共にエンティティのプライマリ キーを形成するため、エンティティの一意の識別子です。 これらのプロパティのみがインデックス付けされているため、これらの値を使用すると、他のエンティティのプロパティでクエリを実行するよりもずっと高速にクエリを実行できます。

Table service は **PartitionKey** を使用してインテリジェントにテーブル エンティティをストレージ ノード全体に分散させます。 **PartitionKey** が同じエンティティは同じノードに格納されます。 **RowKey** は、エンティティが属するパーティション内のエンティティの一意の ID です。

## エンティティを更新する
<a id="update-an-entity" class="xliff"></a>

エンティティのプロパティの値をすべて更新するには、[update_entity][py_update_entity] メソッドを呼び出します。 この例は、既存のエンティティを更新されたバージョンに置き換える方法を示しています。

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

更新されるエンティティが存在しない場合、更新操作は失敗します。 エンティティが存在するかどうかに関わらず、エンティティを格納する場合は、[insert_or_replace_entity][py_insert_or_replace_entity] を使用します。 次の例では、最初の呼び出しで既存のエンティティを置き換えます。 2 番目の呼び出しでは、指定された PartitionKey と RowKey を持つエンティティがテーブル内に存在しないため、新しいエンティティが挿入されます。

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> [update_entity][py_update_entity] メソッドは、既存のエンティティのすべてのプロパティと値を置き換えます。既存のエンティティからプロパティを削除するのにも使用できます。 [merge_entity][py_merge_entity] メソッドを使用すると、エンティティを完全に置き換えることなく、既存のエンティティを新しい、または変更されたプロパティ値で置き換えることができます。

## 複数のエンティティを変更する
<a id="modify-multiple-entities" class="xliff"></a>

Table service による要求のアトミック処理を保証するため、複数の操作をまとめてバッチで送信できます。 まず、[TableBatch][py_TableBatch] クラスを使用して、1 つのバッチに複数の操作を追加します。 次に、[TableService][py_TableService].[commit_batch][py_commit_batch] を呼び出して、操作をアトミック操作で送信します。 バッチで変更されるすべてのエンティティが同じパーティション内にある必要があります。

この例では、2 つのエンティティをバッチでまとめて追加します。

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

バッチは、コンテキスト マネージャーの構文でも使用できます。

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## エンティティを照会する
<a id="query-for-an-entity" class="xliff"></a>

テーブル内のエンティティに対してクエリを実行するには、その PartitionKey と RowKey を [TableService][py_TableService].[get_entity][py_get_entity] メソッドに渡します。

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## エンティティのセットを照会する
<a id="query-a-set-of-entities" class="xliff"></a>

**filter** パラメーターにフィルター文字列を指定することで、一連のエンティティに対してクエリを実行できます。 この例では、PartitionKey にフィルターを適用して、Seattle 内のすべてのタスクを検索します。

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## エンティティ プロパティのサブセットを照会する
<a id="query-a-subset-of-entity-properties" class="xliff"></a>

クエリの各エンティティに返すプロパティを制限することもできます。 *プロジェクション*と呼ばれるこの方法では、帯域幅の使用が削減され、クエリのパフォーマンスが向上します。特に、大量のエンティティや結果セットがある場合に役立ちます。 **select** パラメーターを使用して、クライアントに返すプロパティの名前を渡します。

次のコードのクエリは、テーブル内のエンティティの説明だけを返します。

> [!NOTE]
> 次のスニペットは、Azure Storage に対してのみ機能します。 ストレージ エミュレーターではサポートされません。

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## エンティティを削除する
<a id="delete-an-entity" class="xliff"></a>

PartitionKey と RowKey を [delete_entity][py_delete_entity] メソッドに渡すことで、エンティティを削除できます。

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## テーブルを削除する
<a id="delete-a-table" class="xliff"></a>

不要になったテーブルまたはテーブル内のエンティティがある場合、[delete_table][py_delete_table] メソッドを呼び出して、Azure Storage からテーブルを完全に削除します。

```python
table_service.delete_table('tasktable')
```

## 次のステップ
<a id="next-steps" class="xliff"></a>

* [Azure Storage SDK for Python API のリファレンス](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python)
* [Python デベロッパー センター](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure ストレージ エクスプローラー](../vs-azure-tools-storage-manage-with-storage-explorer.md): Windows、macOS、および Linux で Azure Storage のデータを視覚的に操作するための無料のクロス プラットフォーム アプリケーション

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch

