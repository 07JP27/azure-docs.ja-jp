<properties
   pageTitle="モノのインターネットのセキュリティの概要 | Microsoft Azure"
   description=" Azure IoT (モノのインターネット) サービスには、さまざまな機能が用意されています。この記事では、Azure の IoT ソリューションをセキュリティで保護する方法について説明します。"
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# モノのインターネットのセキュリティの概要

Azure IoT (モノのインターネット) サービスには、さまざまな機能が用意されています。このエンタープライズ クラスのサービスを使用すると、次の操作を実行できます。

- デバイスからデータを収集する
- インモーション データ ストリームを分析する
- 大規模なデータ セットを格納して照会する
- リアルタイム データと履歴データを表示する
- バックオフィス システムと統合する

これらの機能を提供するため、Azure IoT Suite では、複数の Azure サービスとカスタム拡張機能が構成済みソリューションとしてパッケージ化されています。構成済みソリューションは、一般的な IoT ソリューション パターンの基本実装です。このソリューションを使用すると、IoT ソリューションの提供に必要な時間を短縮できます。IoT ソフトウェア開発キットを使用し、独自の要件を満たすためにこのソリューションをカスタマイズして拡張することができます。新しい IoT ソリューションを開発する場合は、例やテンプレートとしてこのソリューションを使用することもできます。

Azure IoT Suite は、IoT のニーズに応える強力なソリューションです。ただし、IoT ソリューションは、最初からセキュリティを念頭に置いて設計することが最も重要です。IoT デバイスは非常に多いため、セキュリティ上の問題がが発生すると、すぐに広範囲にわたって重大な影響を及ぼすことがあります。

IoT ソリューションをセキュリティ保護する方法について、以下に示します。

## セキュリティのアーキテクチャ

システムを設計する際は、そのシステムに対する脅威のリスクを把握し、設計と構築時にそれに応じた適切な防御措置を講じることが重要となります。特に重要なことは、最初からセキュリティを考慮に入れて製品を設計することです。攻撃者がどのような方法でシステムに侵入しうるかを把握しておけば、適切な軽減策を初期段階から講じることができるためです。

IoT のセキュリティ アーキテクチャについて詳しくは、「[モノのインターネット (IoT) のセキュリティ アーキテクチャ](../iot-suite/iot-security-architecture.md)」を参照してください。

この記事では、次のトピックについて説明します。

- [セキュリティの第一歩は脅威モデル](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [IoT のセキュリティ](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Azure IoT リファレンス アーキテクチャの脅威のモデル化](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## 徹底的なセキュリティ

IoT は、世界各地の企業に固有のセキュリティ、プライバシー、およびコンプライアンスの課題をもたらします。ソフトウェアとその実装方法に関する問題が発生した場合、従来のサイバー テクノロジとは異なり、IoT ではサイバーおよび物理世界が融合すると何が起こるかが懸念されます。IoT ソリューションを保護するには、デバイスの安全なプロビジョニング、デバイスとクラウド間の安全な接続、処理中および保管中のクラウドでの安全なデータ保護を確実に行う必要があります。ただし、そのような機能には、リソースが限られたデバイス、デプロイの地理的分散、およびソリューション内の多数のデバイスという問題が伴います。

これらの領域のセキュリティに対処する方法について詳しくは、「[最初からモノのインターネットをセキュリティで保護する](../iot-suite/securing-iot-ground-up.md)」を参照してください。

この記事では、次のトピックについて説明します。

- [最初からインフラストラクチャをセキュリティで保護する](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure - 企業の IoT インフラストラクチャのセキュリティ保護](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## ベスト プラクティス

IoT インフラストラクチャを保護するには、緻密なセキュリティ戦略が必要です。クラウド内のデータのセキュリティ保護から、パブリック インターネット経由での転送中におけるデータ整合性の保護、およびデバイスを安全にプロビジョニングする機能の提供に至るまで、各レイヤーは、インフラストラクチャ全体でより高度なセキュリティ確保を実現します。

モノのインターネットのセキュリティのベスト プラクティスについて詳しくは、「[モノのインターネット (IoT) のセキュリティのベスト プラクティス](../iot-suite/iot-security-best-practices.md)」を参照してください。

この記事では、次のトピックについて説明します。

- [IoT ハードウェアの製造元/インテグレーター](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [IoT ソリューション開発者](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [IoT ソリューションのデプロイ担当者](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [IoT ソリューションのオペレーター](../iot-suite/iot-security-best-practices.md#iot-solution-operator)

<!---HONumber=AcomDC_0810_2016-->