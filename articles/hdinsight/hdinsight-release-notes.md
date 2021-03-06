---
title: Azure HDInsight 릴리스 정보
description: Azure HDInsight에 대한 최신 릴리스 정보입니다. Hadoop, Spark, R Server, Hive 등에 대 한 개발 팁 및 세부 정보를 확인 하세요.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 37b49b3fbe91d199b13f548e8aaf72a6a2f0f848
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895303"
---
# <a name="release-notes"></a>릴리스 정보

이 문서에서는 **가장 최근** Azure HDInsight 릴리스 업데이트에 대한 정보를 제공합니다. 이전 릴리스에 대한 자세한 내용은 [HDInsight 릴리스 정보 보관](hdinsight-release-notes-archive.md)을 참조하세요.

## <a name="summary"></a>요약

Azure HDInsight는 Azure에서 오픈 소스 분석을 위해 기업 고객 들 사이에서 가장 인기 있는 서비스 중 하나입니다.

## <a name="release-date-01092019"></a>릴리스 날짜: 01/09/2019

이 릴리스는 HDInsight 3.6 및 4.0에 모두 적용 됩니다. HDInsight 릴리스는 며칠 동안 모든 지역에서 사용할 수 있습니다. 릴리스 날짜는 첫 번째 지역 릴리스 날짜를 나타냅니다. 아래 변경 내용이 표시 되지 않으면 며칠 동안 해당 지역에서 릴리스가 라이브 될 때까지 기다려 주세요.

> [!IMPORTANT]  
> Linux는 HDInsight 버전 3.4 이상에서 사용되는 유일한 운영 체제입니다. 자세한 내용은 [HDInsight 버전 관리 문서](hdinsight-component-versioning.md)를 참조하세요.

## <a name="new-features"></a>새로운 기능
### <a name="tls-12-enforcement"></a>TLS 1.2 적용
TLS(전송 계층 보안) 및 SSL(Secure Sockets Layer)은 컴퓨터 네트워크를 통해 통신 보안을 제공하는 암호화 프로토콜입니다. [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)에 대해 자세히 알아보세요. HDInsight는 공용 HTTPs 끝점에서 TLS 1.2를 사용 하지만 TLS 1.1은 이전 버전과의 호환성을 위해 계속 지원 됩니다. 

이 릴리스에서 고객은 tls 1.2를 통해 모든 연결에 TLS 1.2을 옵트인 (opt in) 할 수 있습니다. 새 속성 **Minsupportedtlsversion** 은 클러스터 생성을 위해 Azure Resource Manager 템플릿을 통해 도입 됩니다. 속성이 설정 되지 않은 경우 클러스터는 여전히 1.0, 1.1 및 1.2을 지원 하 고 오늘날의 동작과 동일 하 게 지원 합니다. 고객은이 속성의 값을 "1.2"로 설정할 수 있습니다. 즉, 클러스터가 TLS 1.2 이상만 지원 합니다. 

### <a name="bring-your-own-key-for-disk-encryption"></a>디스크 암호화를 위한 고유 키 가져오기
HDInsight의 모든 관리 디스크는 Azure SSE(스토리지 서비스 암호화)로 보호됩니다. 이러한 디스크의 데이터는 기본적으로 Microsoft 관리 키에 의해 암호화 됩니다. 이 릴리스부터는 디스크 암호화에 대 한 Bring Your Own Key (BYOK) Azure Key Vault를 사용 하 여 관리할 수 있습니다. BYOK 암호화는 클러스터를 만드는 동안 추가 비용 없이 1 단계 구성입니다. Azure Key Vault를 사용 하 여 HDInsight를 관리 되는 id로 등록 하 고 클러스터를 만들 때 암호화 키를 추가 하기만 하면 됩니다. 

## <a name="deprecation"></a>사용 중단
이 릴리스에 대 한 결함 없습니다. 예정 된 결함을 준비 하려면 예정 된 [변경 내용](#upcoming-changes)을 참조 하세요.

## <a name="behavior-changes"></a>동작 변경
이 릴리스에 대 한 동작은 변경 되지 않습니다. 예정 된 변경 내용을 준비 하려면 예정 된 [변경 내용](#upcoming-changes)을 참조 하세요.

## <a name="upcoming-changes"></a>예정된 변경
이후 릴리스에서는 다음과 같은 변경이 수행 됩니다. 

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>헤드 노드에는 최소 4 코어 VM이 필요 합니다. 
최소 4 코어 VM은 HDInsight 클러스터의 고가용성 및 안정성을 보장 하기 위해 헤드 노드에 필요 합니다. 4 월 6 일 2020부터 고객은 새 HDInsight 클러스터에 대 한 헤드 노드로 4 코어 이상 VM만 선택할 수 있습니다. 기존 클러스터는 계속해서 예상대로 실행됩니다. 

### <a name="esp-spark-cluster-node-size-change"></a>ESP Spark 클러스터 노드 크기 변경 
이후 릴리스에서는 ESP Spark 클러스터에 허용 되는 최소 노드 크기가 Standard_D13_V2로 변경 됩니다. A 시리즈 Vm은 비교적 낮은 CPU와 메모리 용량 때문에 ESP 클러스터 문제를 일으킬 수 있습니다. A 시리즈 Vm은 새 ESP 클러스터를 만드는 데 사용 되지 않습니다.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure 가상 머신 확장 집합으로 이동
이제 HDInsight는 Azure virtual machines를 사용 하 여 클러스터를 프로 비전 합니다. 향후 릴리스에서 HDInsight는 Azure virtual machine scale sets를 대신 사용 합니다. Azure 가상 머신 확장 집합에 대 한 자세한 내용을 참조 하세요.

### <a name="hbase-20-to-21"></a>HBase 2.0 ~ 2.1
예정 된 HDInsight 4.0 릴리스에서 HBase 버전은 버전 2.0에서 2.1로 업그레이드 됩니다.

## <a name="bug-fixes"></a>버그 수정
HDInsight는 계속 해 서 클러스터 안정성과 성능을 향상 시킵니다. 

## <a name="component-version-change"></a>구성 요소 버전 변경
이 릴리스에 대 한 구성 요소 버전이 변경 되지 않았습니다. HDInsight 4.0 ad HDInsight 3.6의 최신 구성 요소 버전은 여기에서 찾을 수 있습니다.
