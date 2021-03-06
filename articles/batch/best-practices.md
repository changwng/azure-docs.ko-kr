---
title: 모범 사례-Azure Batch
description: Azure Batch 솔루션을 개발 하는 데 유용한 모범 사례 및 유용한 팁을 알아보세요.
author: laurenhughes
ms.author: lahugh
ms.date: 11/22/2019
ms.service: batch
ms.topic: article
manager: gwallace
ms.openlocfilehash: 19c5b6acaeddb915af49cf62a884da0678075f15
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74535666"
---
# <a name="azure-batch-best-practices"></a>Azure Batch 모범 사례

이 문서에서는 Azure Batch 서비스를 효과적이 고 효율적으로 사용 하기 위한 모범 사례 모음을 설명 합니다. 이러한 모범 사례는 batch와 Batch 고객의 경험에서 얻은 것입니다. 을 개발 하 고 Batch를 사용 하는 동안 디자인 문제, 잠재적 성능 문제 및 패턴을 방지 하려면이 문서를 이해 하는 것이 중요 합니다.

이 문서에서는 다음에 대해 알아봅니다.

> [!div class="checklist"]
> - 모범 사례
> - 모범 사례를 사용 해야 하는 이유
> - 모범 사례를 따르지 않을 경우 발생 하는 상황
> - 모범 사례를 따르는 방법

## <a name="pools"></a>풀

Batch 풀은 Batch 서비스에서 작업을 실행 하기 위한 계산 리소스입니다. 다음 섹션에서는 Batch 풀을 사용할 때 따라야 하는 주요 모범 사례에 대 한 지침을 제공 합니다.

### <a name="pool-configuration-and-naming"></a>풀 구성 및 이름 지정

- **풀 할당 모드**  
    Batch 계정을 만들 때 **batch 서비스** 또는 **사용자 구독의**두 가지 풀 할당 모드 중에서 선택할 수 있습니다. 대부분의 경우 기본 Batch 서비스 모드를 사용 해야 합니다 .이 모드에서는 풀이 일괄 처리 관리 되는 구독에서 내부적으로 할당 됩니다. 대체 사용자 구독 모드인 경우 Batch VM 및 기타 리소스는 풀이 만들어질 때 구독에서 직접 만들어집니다. 사용자 구독 계정은 주로 중요 하지만 시나리오의 작은 하위 집합을 사용 하도록 설정 하는 데 사용 됩니다. 사용자 구독 모드에 [대 한 추가 구성](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode)에서 사용자 구독 모드에 대해 자세히 알아볼 수 있습니다.

- **풀 매핑 작업을 결정할 때 작업 및 태스크 실행 시간을 고려 합니다.**  
    주로 단기 실행 작업으로 구성 된 작업 및 예상 되는 총 작업 수가 작은 경우 작업의 예상 되는 전체 실행 시간이 길지 않게 하려면 각 작업에 대해 새 풀을 할당 하지 마십시오. 노드의 할당 시간은 작업의 실행 시간을 감소 합니다.

- **풀에는 계산 노드가 두 개 이상 있어야 합니다.**  
    개별 노드는 항상 사용할 수 있는 것은 아닙니다. 일반적이 지 않은 하드웨어 오류, 운영 체제 업데이트 및 다른 문제에 대 한 호스트는 개별 노드가 오프 라인 상태가 될 수 있습니다. 일괄 처리 작업에 결정적이 고 보장 된 진행률이 필요한 경우 여러 노드가 있는 풀을 할당 해야 합니다.

- **리소스 이름을 다시 사용 하지 마십시오.**  
    일괄 처리 리소스 (작업, 풀 등)는 자주 발생 하 고 시간이 지남에 따라 이동 합니다. 예를 들어 월요일에 풀을 만들고 화요일에 삭제 한 다음 목요일에 다른 풀을 만들 수 있습니다. 만든 새 리소스 마다 이전에 사용 하지 않은 고유한 이름을 지정 해야 합니다. GUID (전체 리소스 이름 또는 그 일부로)를 사용 하거나 리소스 이름에서 리소스를 만든 시간을 포함 하 여이 작업을 수행할 수 있습니다. Batch는 실제 리소스 ID가 사람이 인식할 수 없는 것 이더라도 리소스에 사람이 읽을 수 있는 이름을 지정 하는 데 사용할 수 있는 [DisplayName](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.jobspecification.displayname?view=azure-dotnet)을 지원 합니다. 고유 이름을 사용 하면 로그 및 메트릭에 어떤 특정 리소스를 사용 했는지 쉽게 구분할 수 있습니다. 또한 리소스에 대 한 지원 사례를 파일에 포함 해야 하는 경우 모호성을 제거 합니다.

- **풀 유지 관리 및 실패 시 연속성**  
    작업에서 풀을 동적으로 사용 하도록 하는 것이 가장 좋습니다. 작업에서 모든 항목에 대해 동일한 풀을 사용 하는 경우 풀에 문제가 발생 하는 경우 작업이 실행 되지 않을 가능성이 있습니다. 이는 시간이 중요 한 워크 로드에서 특히 중요 합니다. 이 문제를 해결 하려면 각 작업을 예약할 때 동적으로 풀을 선택 하거나 만들거나, 비정상 풀을 무시할 수 있도록 풀 이름을 재정의 하는 방법을 사용할 수 있습니다.

- **풀 유지 관리 및 실패 시 비즈니스 연속성**  
    내부 오류, 용량 제약 조건 등의 필요한 크기에 따라 풀이 커지는 것을 방해할 수 있는 여러 가지 원인이 있습니다. 따라서 필요한 경우 다른 풀에서 작업 대상을 변경할 수 있습니다. 즉, 다른 VM 크기에서 일괄 처리를 통해이 [작업](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update?view=azure-dotnet)을 지원할 수 있습니다. 고정 풀 ID는 절대로 삭제 되지 않으며 변경 되지 않는다는 가정 하에 사용 하지 않도록 합니다.

### <a name="pool-lifetime-and-billing"></a>풀 수명 및 청구

풀 수명은 풀 구성에 적용 되는 할당 및 옵션 방법에 따라 달라질 수 있습니다. 풀에는 임의 수명 및 임의 시점에 풀의 계산 노드 수가 달라질 수 있습니다. 풀의 계산 노드를 명시적으로 관리 하거나 서비스에서 제공 하는 기능 (자동 크기 조정 또는 자동 풀)을 통해 관리 하는 것은 사용자의 책임입니다.

- **풀을 새로 유지 합니다.**  
    최신 노드 에이전트 업데이트 및 버그 수정을 얻으려면 몇 개월 마다 풀 크기를 0으로 조정 해야 합니다. 풀은 다시 생성 되거나 0 개 계산 노드로 크기가 조정 되는 경우를 제외 하 고 노드 에이전트 업데이트를 수신 하지 않습니다. 풀을 다시 만들거나 크기를 조정 하기 전에 [노드](#nodes) 섹션에 설명 된 대로 디버깅 목적으로 노드 에이전트 로그를 다운로드 하는 것이 좋습니다.

- **풀 다시 만들기**  
    이와 유사한 정보를 통해 풀을 삭제 하 고 다시 만드는 것은 바람직하지 않습니다. 대신 새 풀을 만들어 새 풀을 가리키도록 기존 작업을 업데이트 합니다. 모든 작업이 새 풀로 이동 된 후에는 이전 풀을 삭제 합니다.

- **풀 효율성 및 청구**  
    일괄 처리 자체에는 추가 요금이 발생 하지 않지만 사용 된 계산 리소스에 대 한 요금은 부과 됩니다. 상태에 관계 없이 풀의 모든 계산 노드에 대해 요금이 청구 됩니다. 여기에는 저장소 및 네트워킹 비용과 같이 노드가 실행 되는 데 필요한 모든 요금이 포함 됩니다. 모범 사례에 대 한 자세한 내용은 [Azure Batch 비용 분석 및 예산](budget.md)을 참조 하세요.

### <a name="pool-allocation-failures"></a>풀 할당 실패

풀 할당 오류는 첫 번째 할당 중 또는 후속 크기 조정 중에 언제 든 지 발생할 수 있습니다. 이는 일괄 처리에서 사용 하는 다른 Azure 서비스의 오류 또는 지역에서 일시적인 용량 고갈로 인 한 것일 수 있습니다. 코어 할당량은 보장은 아니지만 한도입니다.

### <a name="unplanned-downtime"></a>계획 되지 않은 가동 중지 시간

Batch 풀에서 Azure의 가동 중지 시간 이벤트를 경험할 수 있습니다. 이는 일괄 처리에 대 한 시나리오 또는 워크플로를 계획 하 고 개발할 때 염두에 두어야 합니다.

노드가 실패 하는 경우 Batch는 자동으로 이러한 계산 노드의 복구를 자동으로 시도 합니다. 이는 복구 된 노드에서 실행 중인 모든 태스크를 다시 예약 하는 작업을 트리거할 수 있습니다. 중단 되는 작업에 대해 자세히 알아보려면 다시 [시도 디자인](#designing-for-retries-and-re-execution) 을 참조 하세요.

- **Azure 지역 종속성**  
    시간이 중요 하거나 프로덕션 워크 로드가 있는 경우 단일 Azure 지역에 종속 되지 않는 것이 좋습니다. 드문 경우 지만 전체 지역에 영향을 줄 수 있는 문제가 있습니다. 예를 들어 특정 시간에 처리를 시작 해야 하는 경우 *시작 시간 전에*주 지역에서 풀을 확장 하는 것이 좋습니다. 풀 크기 조정이 실패 하면 백업 지역 (또는 지역)에서 풀을 확장 하는 것으로 대체할 수 있습니다. 다른 지역에 있는 여러 계정에 걸친 풀은 다른 풀에서 문제가 발생 하는 경우 쉽게 액세스할 수 있는 준비 된 백업을 제공 합니다. 자세한 내용은 [고가용성을 위한 응용 프로그램 디자인](high-availability-disaster-recovery.md)을 참조 하세요.

## <a name="jobs"></a>교육

작업은 수백, 수천 또는 수백만 개의 작업을 포함 하도록 디자인 된 컨테이너입니다.

- **작업에 많은 태스크 배치**  
    작업을 사용 하 여 단일 태스크를 실행 하는 것은 비효율적입니다. 예를 들어 각각 10 개의 작업을 포함 하는 100 작업을 만드는 대신 1000 작업을 포함 하는 단일 작업을 사용 하는 것이 더 효율적입니다. 각각 단일 작업이 있는 1000 작업을 실행 하는 것이 가장 효율적이 고 가장 느리고 비용이 많이 드는 방법입니다.  

    수천 개의 활성 작업이 필요한 Batch 솔루션을 설계 하지 마십시오. 태스크에 대 한 할당량이 없으므로 가능한 한 적은 작업으로 작업 [및 작업 일정 할당량](batch-quota-limit.md#resource-quotas)을 효율적으로 실행할 수 있습니다.

- **작업 수명**  
    일괄 처리 작업은 시스템에서 삭제 될 때까지 무기한 수명이 있습니다. 작업의 상태는 일정 예약에 더 많은 작업을 허용할 수 있는지 여부를 지정 합니다. 명시적으로 종료 하지 않는 한 작업은 자동으로 완료 됨 상태로 전환 되지 않습니다. [OnAllTasksComplete](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete?view=azure-dotnet) 속성 또는 [maxWallClockTime](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)를 통해 자동으로 트리거될 수 있습니다.

기본 [활성 작업 및 작업 일정 할당량이](batch-quota-limit.md#resource-quotas)있습니다. 완료 됨 상태의 작업 및 작업 일정은이 할당량으로 계산 되지 않습니다.

## <a name="tasks"></a>작업

작업은 작업을 구성 하는 개별 작업 단위입니다. 태스크는 사용자가 제출 하 고 일괄 처리를 통해 계산 노드에 예약 됩니다. 작업을 만들고 실행할 때 고려해 야 할 몇 가지 디자인이 있습니다. 다음 섹션에서는 일반적인 시나리오와 문제를 처리 하 고 효율적으로 수행할 수 있도록 작업을 디자인 하는 방법을 설명 합니다.

- **태스크의 일부로 작업 데이터를 저장 합니다.**  
    계산 노드는 기본적으로 삭제 됩니다. 자동 풀 및 자동 크기 조정과 같이 일괄 처리에는 노드를 쉽게 숨길 수 있도록 하는 다양 한 기능이 있습니다. 노드가 풀을 벗어나면 (크기 조정 또는 풀 삭제로 인해) 해당 노드의 모든 파일도 삭제 됩니다. 이로 인해 작업이 완료 되기 전에는 작업이 실행 되는 노드 및 영구 저장소에 대 한 출력을 이동 하는 것이 좋습니다. 마찬가지로 태스크가 실패 하는 경우에도 지 속성 저장소에 대 한 오류를 진단 하는 데 필요한 로그를 이동 해야 합니다. Batch에는 [Outputfiles](batch-task-output-files.md)및 다양 한 공유 파일 시스템을 통해 데이터를 업로드 하는 Azure Storage 통합 지원이 있습니다. 또는 작업에서 직접 업로드를 수행할 수 있습니다.

### <a name="task-lifetime"></a>작업 수명

- **작업이 완료 되 면 삭제 합니다.**  
    태스크가 더 이상 필요 하지 않은 경우 삭제 하거나 [보존](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime?view=azure-dotnet) 작업 제약 조건을 설정 합니다. `retentionTime` 설정 된 경우 Batch는 `retentionTime` 만료 될 때 작업에서 사용 하는 디스크 공간을 자동으로 정리 합니다.  

    작업을 삭제 하면 두 가지 작업이 수행 됩니다. 이를 통해 작업에 대 한 작업의 빌드를 수행 하지 않아도 됩니다. 즉, 완료 된 작업을 통해 필터링 해야 하기 때문에 관심 있는 작업을 쿼리/찾을 수 있습니다. 또한 노드의 해당 태스크 데이터를 정리 합니다 (`retentionTime` 이미 적중 되지 않은 경우). 이렇게 하면 노드가 작업 데이터를 채우지 않고 디스크 공간이 부족해 지 게 됩니다.

### <a name="task-submission"></a>작업 제출

- **컬렉션에 많은 수의 작업을 제출 합니다.**  
    작업은 개별 기준 또는 컬렉션에 제출할 수 있습니다. 오버 헤드 및 제출 시간을 줄이기 위해 작업을 대량으로 전송 하는 경우 한 번에 최대 100의 [컬렉션](https://docs.microsoft.com/rest/api/batchservice/task/addcollection) 에 작업을 제출 합니다.

### <a name="task-execution"></a>작업 실행

- **노드당 최대 태스크 선택**  
    Batch는 노드에 대 한 과도 한 구독 작업을 지원 합니다 (노드에 코어 수가 더 많은 작업 실행). 작업이 풀의 노드에 "적합" 한지 확인 합니다. 예를 들어 두 개의 작업을 각각 25%의 CPU 사용량을 사용 하는 풀에서 하나의 노드 (`maxTasksPerNode = 8`)로 예약 하려는 경우 성능이 저하 될 수 있습니다.

### <a name="designing-for-retries-and-re-execution"></a>다시 시도 및 다시 실행을 위한 디자인

일괄 처리로 태스크를 자동으로 다시 시도할 수 있습니다. 사용자가 제어 하 고 내부를 다시 시도 하는 두 가지 유형이 있습니다. 사용자가 제어 하는 재시도는 작업의 [maxTaskRetryCount](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount?view=azure-dotnet)에 의해 지정 됩니다. 작업에 지정 된 프로그램이 0이 아닌 종료 코드로 종료 되 면 작업은 `maxTaskRetryCount`값으로 다시 시도 됩니다.

드문 경우 지만 작업을 실행 하는 동안 노드의 내부 상태나 실패를 업데이트할 수 없는 등 계산 노드의 오류로 인해 내부적으로 작업을 다시 시도할 수 있습니다. 가능한 경우 동일한 계산 노드에서 작업을 다시 시도 하 고, 작업을 포기 하기 전 내부 제한까지 다시 시도 하 고, 일괄 처리를 통해 다른 계산 노드에 잠재적으로 다시 예약할 작업을 지연 시킵니다.

- **내구성이 있는 작업 빌드**  
    작업은 오류를 견딜 수 있도록 설계 되 고 재시도를 수용 해야 합니다. 이는 장기 실행 작업에서 특히 중요 합니다. 이렇게 하려면 작업이 두 번 이상 실행 되는 경우에도 동일한 단일 결과를 생성 하도록 합니다. 이를 달성 하는 한 가지 방법은 작업을 "목표 검색"으로 만드는 것입니다. 또 다른 방법은 작업이 idempotent 지 확인 하는 것입니다. 작업은 실행 횟수에 관계 없이 동일한 결과를 가집니다.

    일반적인 예는 계산 노드에 파일을 복사 하는 작업입니다. 간단한 방법은 실행 될 때마다 지정 된 모든 파일을 복사 하는 작업입니다 .이 작업은 비효율적 이며 문제를 견딜 수 있도록 빌드되지 않습니다. 대신 파일이 계산 노드에 있는지 확인 하는 작업을 만듭니다. 이미 있는 파일을 다시 복사 하지 않는 작업입니다. 이러한 방식으로 작업이 중단 된 경우 중단 된 위치를 선택 합니다.

- **낮은 우선 순위 노드**  
    전용 또는 우선 순위가 낮은 노드에서 작업을 실행할 때는 디자인의 차이점이 없습니다. 낮은 우선 순위의 노드에서 실행 되거나 전용 노드의 오류로 인해 중단 된 작업을 선점 하는지 여부에 상관 없이 두 경우 모두 오류를 견딜 수 있도록 작업을 디자인 하 여 완화 됩니다.

- **작업 실행 시간**  
    실행 시간이 짧은 작업을 피합니다. 1 ~ 2 초 동안만 실행 되는 작업은 적절 하지 않습니다. 개별 작업에서 많은 양의 작업을 수행 해야 합니다 (최소 10 초, 최대 시간 또는 일 이동). 각 태스크가 1 분 이상 실행 되는 경우 전체 계산 시간의 일부로 예약 오버 헤드가 줄어듭니다.

## <a name="nodes"></a>노드

- **시작 작업은 idempotent 여야 합니다.**  
    다른 작업과 마찬가지로 노드 시작 태스크는 노드가 부팅 될 때마다 다시 실행 되므로 idempotent 되어야 합니다. Idempotent 작업은 여러 번 실행할 때 일관 된 결과를 생성 하는 작업입니다.

- **운영 체제 서비스 인터페이스를 통해 장기 실행 서비스를 관리 합니다.**  
    노드의 일괄 처리 에이전트와 함께 다른 에이전트를 실행 해야 하는 경우도 있습니다. 예를 들어 노드에서 데이터를 수집 하 고 보고 합니다. 이러한 에이전트는 Windows 서비스 또는 Linux `systemd` 서비스와 같은 OS 서비스로 배포 하는 것이 좋습니다.  

    이러한 서비스를 실행 하는 경우 노드의 일괄 처리 관리 디렉터리에 있는 파일에 대 한 파일 잠금을 사용 하지 않아야 합니다. 그렇지 않으면 일괄 처리는 파일 잠금으로 인해 이러한 디렉터리를 삭제할 수 없기 때문입니다. 예를 들어 시작 작업에 Windows 서비스를 설치 하는 경우 시작 태스크 작업 디렉터리에서 직접 서비스를 시작 하는 대신 파일을 다른 위치에 복사 합니다. 파일이 있는 경우 복사본을 건너뜁니다. 해당 위치에서 서비스를 설치 합니다. 일괄 처리가 시작 작업을 다시 실행 하는 경우 시작 태스크 작업 디렉터리를 삭제 하 고 다시 만듭니다. 이는 서비스가 시작 태스크 작업 디렉터리가 아닌 다른 디렉터리에서 파일 잠금을 보유 하 고 있기 때문에 작동 합니다.

- **Windows에서 디렉터리 연결점을 만들지 마십시오.**  
    디렉터리 하드 링크 라고도 하는 디렉터리 연결은 작업 및 작업 정리 중에 처리 하기 어렵습니다. 하드 링크 대신 symlink (소프트 링크)를 사용 합니다.

- **문제가 있는 경우 Batch 에이전트 로그 수집**  
    노드에서 실행 중인 노드 또는 태스크의 동작과 관련 된 문제가 발생 하는 경우 해당 노드의 할당을 취소 하기 전에 일괄 처리 에이전트 로그를 수집 하는 것이 좋습니다. Batch 에이전트 로그는 Batch 서비스 로그 업로드 API를 사용 하 여 수집할 수 있습니다. 이러한 로그는 Microsoft에 대 한 지원 티켓의 일부로 제공 될 수 있으며 문제 해결 및 해결에 도움이 됩니다.

## <a name="security"></a>보안

### <a name="security-isolation"></a>보안 격리

격리를 위해 시나리오에서 서로 다른 작업을 격리 해야 하는 경우 별도의 풀에 포함 하 여 이러한 작업을 격리 해야 합니다. 풀은 Batch의 보안 격리 경계 이며 기본적으로 두 개의 풀이 표시 되지 않거나 서로 통신할 수 없습니다. 격리 수단으로 별도의 Batch 계정을 사용 하지 마십시오.
