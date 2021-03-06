---
title: Azure IoT Hub 할당량 및 제한 이해 | Microsoft 문서
description: 개발자 가이드 - IoT Hub에 적용할 할당량 및 예상되는 제한 동작을 설명합니다.
author: robinsh
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: c17576bb8cd772742b5335000a2453ff34753779
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75457072"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>참조 - IoT Hub 할당량 및 제한

이 문서에서는 IoT Hub에 대 한 할당량을 설명 하 고, 제한이 작동 하는 방식을 이해 하는 데 도움이 되는 정보를 제공 합니다.

## <a name="quotas-and-throttling"></a>할당량 및 제한

각 Azure 구독은 IoT Hub 최대 50개와 무료 허브 최대 1개를 가질 수 있습니다.

각 IoT 허브는 특정 계층에서 특정한 단위 수로 프로비전됩니다. 계층과 단위 수는 보낼 수 있는 메시지의 최대 일일 할당량을 결정합니다. 일일 할당량을 계산하는 데 사용되는 메시지 크기는 무료 계층 허브의 경우 0.5KB이며 기타 모든 계층의 경우 4KB입니다. 자세한 내용은 [Azure IoT Hub 가격](https://azure.microsoft.com/pricing/details/iot-hub/)을 참조하세요.

또한 계층은 IoT Hub가 모든 작업에 강제로 적용하는 조정 제한을 결정합니다.

### <a name="iot-plug-and-play"></a>IoT 플러그 앤 플레이

공개 미리 보기 중 IoT 플러그 앤 플레이 장치는 인터페이스 마다 별도의 메시지를 전송 하 여 메시지 할당량에 대해 계산 되는 메시지 수를 늘릴 수 있습니다.

## <a name="operation-throttles"></a>작업 제한

작업 제한는 분 범위에서 적용 되 고 남용을 방지 하기 위해 사용 되는 rate 제한 사항입니다. 또한 [트래픽 셰이핑](#traffic-shaping)의 영향을 받습니다.

다음 표에서는 적용된 제한을 보여 줍니다. 값은 개별 허브라고 합니다.

| 제한 | 무료, B1 및 S1 | B2 및 S2 | B3 및 S3 | 
| -------- | ------- | ------- | ------- |
| [Id 레지스트리 작업](#identity-registry-operations-throttle) (만들기, 검색, 목록, 업데이트, 삭제) | 1.67/초/단위(100/분/단위) | 1.67/초/단위(100/분/단위) | 83.33/초/단위 (5000/분/단위) |
| [새 장치 연결](#device-connections-throttle) (이 제한은 총 연결 수가 아닌 _새 연결_의 비율에 적용 됨) | 100/초 또는 12/초/단위 이상 <br/> 예를 들어 두 개의 S1 단위는 2\*12=24 새 연결/초이지만 단위에는 100개 이상의 새 연결/초가 있습니다. S1 단위가 9개인 경우 단위 전체에 대한 값은 108 새 연결/초(9\*12)입니다. | 120 새 연결/초/단위 | 6000 새 연결/초/단위 |
| 디바이스-&gt;클라우드 보내기 | 100 전송 작업/초 또는 12 개의 전송 작업/초/단위 <br/> 예를 들어 두 개의 S1 단위는 2\*12 = 24/sec 이지만 사용자 단위에 대해 최소 100의 전송 작업/초가 있습니다. S1 단위 9 개를 사용 하는 경우 단위에서 108 send 작업/초 (9\*12)가 있습니다. | 120 전송 작업/초/단위 | 6000 전송 작업/초/단위 |
| 클라우드-디바이스 보내기<sup>1</sup> | 1.67 전송 작업/초/단위 (100 메시지/분/단위) | 1.67 전송 작업/초/단위 (100 전송 작업/분/단위) | 83.33 전송 작업/초/단위 (5000 전송 작업/분/단위) |
| 클라우드-디바이스 받기<sup>1</sup> <br/> (디바이스에서 HTTPS를 사용하는 경우에만)| 16.67 수신 작업/초/단위 (1000 수신 작업/분/단위) | 16.67 수신 작업/초/단위 (1000 수신 작업/분/단위) | 833.33 수신 작업/초/단위 (5만 수신 작업/분/단위) |
| 파일 업로드 | 1.67 파일 업로드 따르며/초/단위 (100/분/단위) | 1.67 파일 업로드 따르며/초/단위 (100/분/단위) | 83.33 파일 업로드 따르며/초/단위 (5000/분/단위) |
| 직접 메서드<sup>1</sup> | 160KB/sec/unit<sup>2</sup> | 480KB/sec/unit<sup>2</sup> | 24MB/sec/unit<sup>2</sup> | 
| 쿼리 | 20/분/단위 | 20/분/단위 | 1000/분/단위 |
| 쌍(디바이스 및 모듈) 읽기<sup>1</sup> | 100/초 | 100/초 또는 10/초/단위 이상 | 500/초/단위 |
| 쌍 업데이트(디바이스 및 모듈)<sup>1</sup> | 50/초 | 50/초 또는 5/초/단위 이상 | 250/초/단위 |
| 작업(Job) 작업<sup>1</sup> <br/> (만들기, 업데이트, 나열, 삭제) | 1.67/초/단위(100/분/단위) | 1.67/초/단위(100/분/단위) | 83.33/초/단위 (5000/분/단위) |
| 작업 디바이스 연산<sup>1</sup> <br/> (쌍 업데이트, 직접 메서드 호출) | 10/초 | 10/초 또는 1/초/단위 이상 | 50/초/단위 |
| 구성 및 에지 배포<sup>1</sup> <br/> (만들기, 업데이트, 나열, 삭제) | 0.33/초/단위(20/분/단위) | 0.33/초/단위(20/분/단위) | 0.33/초/단위(20/분/단위) |
| 장치 스트림 시작 율<sup>1</sup> | 새 스트림 5개/초 | 새 스트림 5개/초 | 새 스트림 5개/초 |
| 동시에 연결 된 장치 스트림의 최대 수<sup>1</sup> | 50 | 50 | 50 |
| 최대 장치 스트림 데이터 전송<sup>1</sup> (일일 집계 볼륨) | 300MB | 300MB | 300MB |

<sup>1</sup>이 기능은 IoT Hub의 기본 계층에서 사용할 수 없습니다. 자세한 내용은 [올바른 IoT Hub를 선택하는 방법](iot-hub-scaling.md)을 참조하세요. <br/><sup>2</sup> 제한 측정기 크기는 4kb입니다.

### <a name="throttling-details"></a>제한 세부 정보

* 측정기 크기는 제한 한도를 사용 하는 증가값을 결정 합니다. 직접 호출의 페이로드가 0에서 4kb 사이인 경우 4kb로 계산 됩니다. 160 k b/초/단위 제한에 도달 하기 전에 단위당 초당 최대 40 개의 호출을 만들 수 있습니다.

   마찬가지로 페이로드가 4kb에서 8kb 사이에 있는 경우 각 호출은 8kb에 대해 계정을 호출 하 고 최대 한도에 도달 하기 전에 단위당 초당 20 개의 호출을 만들 수 있습니다.

   마지막으로, 페이로드 크기가 156KB에서 160 KB 사이인 경우 허브 당 장치당 초당 1 개의 호출을 수행할 수 있으며, 160 k b/초/단위 제한에 도달 합니다.

*  계층 s 2에 대 한 *작업 장치 작업 (업데이트 쌍, 호출 직접 메서드)* 의 경우 작업을 사용 하 여 메서드를 호출 하는 경우에만 50/초/단위가 적용 됩니다. 직접 메서드를 직접 호출 하는 경우 원래 제한 제한인 24 m b/초/단위 (s 2의 경우)가 적용 됩니다.

*  **할당량** 은 *하루*에 허브에서 보낼 수 있는 집계 메시지 수입니다. [IoT Hub 가격 책정 페이지](https://azure.microsoft.com/pricing/details/iot-hub/)의 **총 메시지 수/일** 열에서 허브의 할당량 한도를 찾을 수 있습니다.

*  클라우드-장치 및 장치-클라우드 제한은 메시지를 보낼 수 있는 최대 *속도* 를 결정 합니다. 4 KB 청크와 관계 없이 메시지 수입니다. 각 메시지는 최대 256 KB가 될 수 있으며 [최대 메시지 크기는 최대](iot-hub-devguide-quotas-throttling.md#other-limits)입니다.

*  제한 한도를 초과 하거나 초과 하지 않도록 호출을 제한 하는 것이 좋습니다. 제한에 도달 하면 IoT Hub는 오류 코드 429로 응답 하 고 클라이언트는 백오프 했다가 다시 시도 해야 합니다. 이러한 한도는 허브 당 또는 허브/단위당 일부 경우에 해당 합니다. 자세한 내용은 [연결 및 신뢰할 수 있는 메시징/재시도 패턴 관리](iot-hub-reliability-features-in-sdks.md#retry-patterns)를 참조 하세요.

### <a name="traffic-shaping"></a>교통 셰이핑

버스트 트래픽을 수용 하기 위해 제한 된 시간에 대 한 제한 보다 높은 요청을 허용 IoT Hub 합니다. 이러한 요청 중 처음 몇 개는 즉시 처리 됩니다. 그러나 요청 수가 계속 해 서 제한을 위반 하는 경우 IoT Hub은 요청을 큐에 배치 하 고 제한 속도로 처리를 시작 합니다. 이러한 효과를 *트래픽 셰이핑*이라고 합니다. 또한이 큐의 크기는 제한 됩니다. 스로틀 위반이 계속 되 면 결국 큐가 채워지고 IoT Hub `429 ThrottlingException`를 사용 하 여 요청을 거부 하기 시작 합니다.

예를 들어 시뮬레이션 된 장치를 사용 하 여 200 초당 장치-클라우드 메시지를 S1 IoT Hub (100/sec D2C 전송)로 보냅니다. 첫 번째 1 ~ 2 개의 경우 메시지는 즉시 처리 됩니다. 그러나 장치가 스로틀 제한 보다 더 많은 메시지를 계속 보내기 때문에 IoT Hub는 초당 100 메시지만 처리 하 고 나머지는 큐에 배치 합니다. 대기 시간이 증가 하는 것을 시작 합니다. 결과적으로, 큐가 채워질 때 `429 ThrottlingException`를 가져오기 시작 하 고 [IoT Hub 메트릭의](iot-hub-metrics.md) "제한 오류 수"가 증가할 수 있습니다.

### <a name="identity-registry-operations-throttle"></a>Id 레지스트리 작업 제한

장치 id 레지스트리 작업은 장치 관리 및 프로 비전 시나리오에서 런타임 사용을 위한 것입니다. 많은 수의 디바이스 ID는 [가져오기 및 내보내기 작업](iot-hub-devguide-identity-registry.md#import-and-export-device-identities)을 통해 읽거나 업데이트할 수 있습니다.

### <a name="device-connections-throttle"></a>장치 연결 제한

*디바이스 연결* 제한은 IoT Hub에서 새 디바이스 연결을 설정할 수 있는 속도를 제어합니다. *디바이스 연결* 제한은 동시에 연결되는 디바이스의 최대 수를 제어하지 않습니다. *디바이스 연결* 속도 제한은 IoT Hub에 대해 프로비전되는 단위의 수에 따라 다릅니다.

예를 들어 S1 단위 하나를 구매하는 경우 초당 연결 제한은 100개입니다. 따라서 10만 장치를 연결 하려면 최소 1000 초 (약 16 분)가 걸립니다. 그러나 ID 레지스트리에 등록한 수만큼의 디바이스를 동시에 연결할 수 있습니다.

## <a name="other-limits"></a>기타 제한

IoT Hub에는 다른 작업 제한도 적용됩니다.

| 작업 | 제한 |
| --------- | ----- |
| 디바이스 | 단일 IoT hub에 등록할 수 있는 장치 및 모듈의 총 수는 100만로 표시 됩니다. 이 한도를 늘리는 유일한 방법은 [Microsoft 지원](https://azure.microsoft.com/support/options/)에 연결 하는 것입니다.|
| 파일 업로드 | 장치 당 10 개의 동시 파일 업로드. |
| 작업<sup>1</sup> | 최대 동시 작업 수는 1 (무료 및 S1), 5 (S2의 경우) 및 10 (s 3의 경우)입니다. 그러나 최대 동시 [장치 가져오기/내보내기 작업](iot-hub-bulk-identity-mgmt.md) 은 모든 계층에 대해 1입니다. <br/>작업 기록은 30 일까지 유지 됩니다. |
| 추가 엔드포인트 | 유료 SKU 허브에는 10개, 무료 SKU 허브에는 하나의 추가 엔드포인트가 있을 수 있습니다. |
| 메시지 라우팅 쿼리 | 유료 SKU 허브에는 100 라우팅 쿼리가 있을 수 있습니다. 무료 SKU 허브에는 5 개의 라우팅 쿼리가 있을 수 있습니다. |
| 메시지 보강 | 유료 SKU 허브에는 최대 10 개의 메시지 강화 있을 수 있습니다. 무료 SKU 허브에는 최대 2 개의 메시지 강화 있을 수 있습니다.|
| 디바이스-클라우드 메시징 | 최대 메시지 크기 256KB |
| 클라우드-디바이스 메시징<sup>1</sup> | 최대 메시지 크기 64KB 배달 보류 중인 최대 메시지 수는 장치당 50입니다. |
| 직접 메서드<sup>1</sup> | 최대 직접 메서드 페이로드 크기는 128KB입니다. |
| 자동 장치 및 모듈 구성<sup>1</sup> | 유료 SKU 허브당 100개 구성입니다. 체험 SKU 허브당 20개 구성입니다. |
| 자동 배포 IoT Edge<sup>1</sup> | 배포당 20개의 모듈 100 유료 SKU 허브 당 배포 (계층화 된 배포 포함) 무료 SKU 허브 당 10 개의 배포. |
| 쌍<sup>1</sup> | Desired 속성의 최대 크기 및 보고 된 속성 섹션은 각각 32 KB입니다. 태그 섹션의 최대 크기는 8kb입니다. |

<sup>1</sup>이 기능은 IoT Hub의 기본 계층에서 사용할 수 없습니다. 자세한 내용은 [올바른 IoT Hub를 선택하는 방법](iot-hub-scaling.md)을 참조하세요.

## <a name="increasing-the-quota-or-throttle-limit"></a>할당량 또는 스로틀 제한을 늘립니다.

지정 된 시간에 [IoT hub에서 프로 비전 된 단위 수를 늘려](iot-hub-upgrade.md)할당량 또는 스로틀 제한을 늘릴 수 있습니다.

## <a name="latency"></a>대기 시간

IoT Hub는 모든 작업에 낮은 대기 시간을 제공하기 위해 노력합니다. 그러나 네트워크 상태 및 예측할 수 없는 기타 요인으로 인해 특정 대기 시간을 보장할 수 없습니다. 솔루션을 설계할 때 다음을 수행해야 합니다.

* IoT Hub 작업의 최대 대기 시간을 가정하지 마세요.
* 디바이스에 가장 가까운 Azure 지역에서 IoT Hub를 프로비전합니다.
* Azure IoT Edge를 사용하여 디바이스 또는 디바이스에 가까운 게이트웨이에서 대기 시간에 민감한 작업을 수행하는 것이 좋습니다.

여러 IoT Hub 단위는 앞에서 설명한 대로 제한에 영향을 주지만 추가 대기 시간을 주거나 보장하지 않습니다.

작업 대기 시간에 예기치 않은 증가가 발생한 경우 [Microsoft 지원](https://azure.microsoft.com/support/options/)에 문의하세요.

## <a name="next-steps"></a>다음 단계

IoT Hub 제한 동작에 대한 자세한 내용은 [IoT Hub 제한](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/) 블로그 게시물을 참조하세요.

이 IoT Hub 개발자 가이드의 다른 참조 자료:

* [IoT Hub 엔드포인트](iot-hub-devguide-endpoints.md)
