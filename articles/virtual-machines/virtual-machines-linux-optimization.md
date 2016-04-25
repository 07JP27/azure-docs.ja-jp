<properties
	pageTitle="Azure での Linux VM の最適化 | Microsoft Azure"
	description="Azure で最適なパフォーマンスが得られるように Linux VM が設定されていることを確認するための、最適化に関するいくつかのヒントを説明します。"
	keywords="linux 仮想マシン,仮想マシン linux,ubuntu 仮想マシン" 
	services="virtual-machines-linux"
	documentationCenter=""
	authors="rickstercdn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager" />

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/07/2016"
	ms.author="rclaus"/>

# Azure での Linux VM の最適化

コマンド ラインやポータルを使用すると、Linux 仮想マシン (VM) を簡単に作成できます。このチュートリアルでは、Microsoft Azure Platform でのパフォーマンスが最適化されるように Linux 仮想マシンがセットアップされていることを確認する方法を説明します。このトピックでは Ubuntu Server VM を使用しますが、[テンプレートとして独自のイメージ](virtual-machines-linux-create-upload-generic.md)を使用して Linux 仮想マシンを作成することもできます。

## 前提条件

このトピックでは、実際にご利用の Azure サブスクリプション ([無料試用版のサインアップ](https://azure.microsoft.com/pricing/free-trial/)) が既にあること、[Azure CLI をインストール済みであること](../xplat-cli-install.md)、VM を Azure サブスクリプションにプロビジョニング済みであることを前提としています。Azure での操作を開始する前に、サブスクリプションに対する認証を行う必要があります。Azure CLI を使用してこれを行うには、「`azure login`」と入力して対話型プロセスを開始するだけです。

## Azure OS ディスク

Azure に Linux VM を作成すると、その VM には 2 つのディスクが関連付けられています。/dev/sda は OS ディスクを表し、/dev/sdb は一時ディスクを表します。メインの OS ディスク (/dev/sda) は、VM の高速起動用に最適化されており、ワークロードでは優れたパフォーマンスを発揮しないため、オペレーティング システム以外の用途には使用しないでください。データ用の永続的で最適化されたストレージにするために、1 つ以上のディスクを VM に接続することができます。

## サイズとパフォーマンスの目標に向けたディスクの追加 

選択した VM サイズに基づいて、A シリーズのマシンでは最大 16 個、D シリーズのマシンでは 32 個、G シリーズのマシンでは 64 個のディスクを接続できます (ディスクのサイズはそれぞれ、最大 1 TB)。スペースと IOPS の要件に従って、必要に応じてさらにディスクを追加することをお勧めします。各ディスクのパフォーマンス目標は、Standard Storage の場合は 500 IOPS、Premium Storage の場合は最大 5,000 IOPS です。Premium Storage のディスクの詳細については、「[Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](../storage/storage-premium-storage.md)」をご覧ください。

キャッシュ設定が "ReadOnly" または "None" に設定されている Premium Storage ディスクで最高レベルの IOPS を実現するには、Linux でファイル システムをマウントするときに "バリア" を無効にする必要があります。Premium Storage ディスクでこれらのキャッシュ設定を使用する場合は、ディスクへの書き込みの耐久性が保証されるため、バリアは必要ありません。

- **reiserFS**: バリアを無効にするには、マウント オプション "barrier=none" を使用します (バリアを有効にするには "barrier=flush" を使用します)。
- **ext3/ext4**: バリアを無効にするには、マウント オプション "barrier=0" を使用します (バリアを有効にするには "barrier=1" を使用します)。
- **XFS**: バリアを無効にするには、マウント オプション "nobarrier" を使用します (バリアを有効にするには "barrier" を使用します)。

## ストレージ アカウントに関する考慮事項

Azure に Linux VM を作成する場合は、近接性を確保してネットワーク待ち時間を最小限に抑えるために、VM と同じリージョンに存在するストレージ アカウントからディスクを接続するようにしてください。各 Standard Storage アカウントには、最大 20,000 IOPS および 500 TB のサイズ容量が適用されます。これは、OS ディスクと作成するすべてのデータ ディスクの両方を含む、約 40 個の使用頻度の高いディスクとなります。Premium Storage アカウントの場合、最大 IOPS に関する制限はありませんが、32 TB のサイズ制限があります。

IOPS が非常に高いワークロードを処理していて、ディスクに Standard Storage を選択した場合は、ディスクを複数のストレージ アカウントに分割して、Standard Storage アカウントの上限である 20,000 IOPS に達しないようにすることが必要になる可能性があります。VM には、ストレージ アカウントやストレージ アカウントの種類が異なるディスクを組み合わせて使用し、最適な構成を実現することができます。

## VM の一時ドライブ

既定では、新しい VM の作成時に、Azure から OS ディスク (/dev/sda) と一時ディスク (/dev/sdb) が提供されます。ユーザーが追加するその他のディスクはすべて、/dev/sdc、/dev/sdd、/dev/sde のように表示されます。一時ディスク (/dev/sdb) 上のすべてのデータは持続性がないため、VM のサイズ変更、再デプロイ、メンテナンスなどの特定のイベントによって VM が再起動された場合に失われる可能性があります。一時ディスクのサイズと種類は、デプロイ時に選択した VM サイズに関連付けられています。プレミアム サイズの VM (DS、G、DS\_V2 シリーズ) のいずれかの場合、ローカル SSD が一時ドライブに使用され、さらに最大 48,000 IOPS のパフォーマンスが実現します。

## Linux のスワップ ファイル

Azure Marketplace からデプロイされた VM イメージでは、VM Linux エージェントが、VM がさまざまな Azure サービスと対話できるようにする OS と統合されています。Azure Marketplace から標準イメージをデプロイしたことを想定すると、Linux スワップ ファイルの設定を正しく構成するには、次の操作を行う必要があります。

**/etc/waagent.conf** ファイル内で 2 つのエントリを探して変更します。これらのエントリは、専用のスワップ ファイルの存在と、そのスワップ ファイルのサイズを制御します。変更しようとしているパラメーターは `ResourceDisk.EnableSwap=N` と `ResourceDisk.SwapSizeMB=0` です

これを次のように変更する必要があります。

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={ニーズに合わせたサイズ (MB)} 

この変更を行った後は、変更を反映するために waagent を再起動するか、Linux VM を再起動する必要があります。`free` コマンドを使用して空き領域を表示すると、変更が実装され、スワップ ファイルが作成されたことがわかります。次の例では、waagent.conf ファイルを変更した結果として、512 MB のスワップ ファイルが作成されています。

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## Premium Storage の I/O スケジューリング アルゴリズム

2\.6.18 Linux カーネルでは、既定の I/O スケジューリング アルゴリズムが Deadline から CFQ (Completely fair queuing アルゴリズム) に変更されました。ランダム アクセス I/O パターンの場合、CFQ と Deadline のパフォーマンスの違いはごくわずかです。ディスク I/O パターンの大部分がシーケンシャルである SSD ベースのディスクの場合、NOOP または Deadline アルゴリズムに戻すと I/O パフォーマンスが向上する可能性があります。

### 現在の I/O スケジューラの表示

次のコマンドを使用します。

	admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

現在のスケジューラを示す次の出力が表示されます。

	noop [deadline] cfq

###I/O スケジューリング アルゴリズムの現在のデバイス (/dev/sda) を変更します。

次のコマンドを使用します。

	azureuser@mylinuxvm:~$ sudo su -
	root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
	root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
	root@mylinuxvm:~# update-grub

>[AZURE.NOTE] これを /dev/sda だけに設定するのは役に立ちません。I/O パターンの大部分がシーケンシャル I/O であるすべてのデータ ディスクで設定する必要があります。

次の出力では、grub.cfg が正常に再構築され、既定のスケジューラが NOOP に更新されたことが参照できます。

	Generating grub configuration file ...
	Found linux image: /boot/vmlinuz-3.13.0-34-generic
	Found initrd image: /boot/initrd.img-3.13.0-34-generic
	Found linux image: /boot/vmlinuz-3.13.0-32-generic
	Found initrd image: /boot/initrd.img-3.13.0-32-generic
	Found memtest86+ image: /memtest86+.elf
	Found memtest86+ image: /memtest86+.bin
	done

Redhat 配布ファミリでは、次のコマンドのみが必要です。

	echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## ソフトウェア RAID の使用による IOPS の向上

単一のディスクで実現できる以上の IOPS を必要とするワークロードの場合、複数のディスクから成るソフトウェア RAID 構成を使用する必要があります。Azure は既にローカルのファブリック層でディスクの回復性を実現しているため、RAID 0 のストライピング構成を使用することで最高レベルのパフォーマンスが実現されます。ドライブのパーティション分割、フォーマット、マウントを実行する前に、Azure 環境で新しいディスクをプロビジョニングして作成し、それらを Linux VM に接続する必要があります。Azure の Linux VM でのソフトウェア RAID セットアップの構成の詳細については、「**[Linux でのソフトウェア RAID の構成](virtual-machines-linux-configure-raid.md)**」をご覧ください。


## 次のステップ

最適化に関するあらゆる話題と同様に、それぞれの変更を加える前と後でテストを行って、その変更が与える影響を測定する必要があります。最適化は段階的なプロセスであり、お使いの環境内のコンピューターごとに結果は異なります。ある構成に効果があっても、それが他の構成に効果があるとは限りません。

関連リソースへの便利なリンクは次のとおりです。

- [Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](../storage/storage-premium-storage.md)
- [Azure Linux エージェント ユーザー ガイド](virtual-machines-linux-agent-user-guide.md)
- [Azure Linux VM 上での MySQL のパフォーマンスを最適化する](virtual-machines-linux-classic-optimize-mysql.md)
- [Linux でのソフトウェア RAID の構成](virtual-machines-linux-configure-raid.md)

<!---HONumber=AcomDC_0413_2016-->