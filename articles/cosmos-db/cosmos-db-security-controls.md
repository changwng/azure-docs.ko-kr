---
title: Azure Cosmos DB에 대 한 보안 제어
description: 네트워크, 모니터링, id 및 데이터 보호와 같은 보안 컨트롤에 대 한 검사 목록을 확인 하 여 평가 Azure Cosmos DB
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.openlocfilehash: 5ab4281f1ad591befda5a439906604331a1ab323
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74872148"
---
# <a name="security-controls-for-azure-cosmos-db"></a>Azure Cosmos DB에 대 한 보안 제어

이 문서에서는 Azure Cosmos DB에 기본 제공 되는 보안 컨트롤을 설명 합니다.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>네트워크

| 보안 제어 | 예/아니요 | 참고 |
|---|---|--|
| 서비스 엔드포인트 지원| yes |  |
| VNet 삽입 지원| yes | VNet 서비스 끝점을 사용 하 여 가상 네트워크 (VNet)의 특정 서브넷 에서만 액세스할 수 있도록 Azure Cosmos DB 계정을 구성할 수 있습니다. 또한 VNet 액세스와 방화벽 규칙을 결합할 수 있습니다. 자세히 알아보려면 [가상 네트워크에서 Azure Cosmos DB 액세스](VNet-service-endpoint.md)를 참조 하세요. |
| 네트워크 격리 및 방화벽 지원| yes | 방화벽 지원을 통해 승인 된 IP 주소 집합, IP 주소 및/또는 클라우드 서비스 에서만 액세스할 수 있도록 Azure Cosmos 계정을 구성할 수 있습니다. 자세히 알아보려면 [Azure Cosmos DB에서 IP 방화벽 구성](how-to-configure-firewall.md)을 참조 하세요.|
| 강제 터널링 지원| yes | 가상 컴퓨터가 있는 VNet의 클라이언트 쪽에서 구성할 수 있습니다.   |

## <a name="monitoring--logging"></a>& 로깅 모니터링

| 보안 제어 | 예/아니요 | 참고|
|---|---|--|
| Azure 모니터링 지원 (Log analytics, App insights 등)| yes | Azure Cosmos DB로 전송 되는 모든 요청이 로깅됩니다. Azure [모니터링](../azure-monitor/overview.md), azure 메트릭, Azure 감사 로깅이 지원 됩니다.  데이터 평면 요청, 쿼리 런타임 통계, 쿼리 텍스트, MongoDB 요청에 해당 하는 정보를 기록할 수 있습니다. 경고를 설정할 수도 있습니다. |
| 제어 및 관리 평면 로깅 및 감사| yes | 방화벽, Vnet, 키 액세스, IAM 등의 계정 수준 작업에 대 한 Azure 활동 로그입니다. |
| 데이터 평면 로깅 및 감사 | yes | 컨테이너 만들기, 처리량 프로 비전, 인덱싱 정책 및 문서에 대 한 CRUD 작업과 같은 컨테이너 수준 작업에 대 한 진단 모니터링 로깅입니다. |

## <a name="identity"></a>ID

| 보안 제어 | 예/아니요 | 참고|
|---|---|--|
| Authentication| yes | 예, 데이터베이스 계정 수준에서 데이터 평면 수준에서 Cosmos DB는 리소스 토큰과 키 액세스를 사용 합니다. |
| 권한 부여| yes | 마스터 키 (기본 및 보조) 및 리소스 토큰을 사용 하는 Azure Cosmos 계정에서 지원 됩니다. 마스터 키를 사용 하 여 데이터에 대 한 읽기/쓰기 또는 읽기 전용 액세스 권한을 얻을 수 있습니다. 리소스 토큰은 문서 및 컨테이너와 같은 리소스에 대 한 제한 된 시간 액세스를 허용 합니다. |

## <a name="data-protection"></a>데이터 보호

| 보안 제어 | 예/아니요 | 참고 |
|---|---|--|
| 미사용 서버 쪽 암호화: Microsoft 관리 키 | yes | 모든 Azure Cosmos 데이터베이스 및 백업은 기본적으로 암호화 됩니다. [Azure Cosmos DB의 데이터 암호화를](database-encryption-at-rest.md)참조 하세요. 고객 관리 키를 사용 하는 서버 쪽 암호화는 지원 되지 않습니다. |
| 미사용 서버 쪽 암호화: 고객 관리 키 (BYOK) | 아닙니다. |  |
| 열 수준 암호화 (Azure Data Services)| yes | 테이블 API Premium 에서만. 모든 Api에서이 기능을 지 원하는 것은 아닙니다. [Azure Cosmos DB 소개: Table API를](table-introduction.md)참조 하세요. |
| 전송 중 암호화 (예: Express 경로 암호화, VNet 암호화 및 VNet-VNet 암호화)| yes | 전송 시 모든 Azure Cosmos DB 데이터가 암호화 됩니다. |
| API 호출 암호화| yes | Azure Cosmos DB에 대 한 모든 연결은 HTTPS를 지원 합니다. Azure Cosmos DB는 TLS 1.2 연결도 지원 하지만 아직 적용 되지 않습니다. 고객이 더 낮은 수준의 TLS를 종료 하는 경우 Cosmos DB에 연결 하도록 보장할 수 있습니다.  |

## <a name="configuration-management"></a>Configuration Management

| 보안 제어 | 예/아니요 | 참고|
|---|---|--|
| 구성 관리 지원 (구성의 버전 관리 등)| 아닙니다.  | | 

## <a name="additional-security-controls-for-cosmos-db"></a>Cosmos DB에 대 한 추가 보안 제어

| 보안 제어 | 예/아니요 | 참고|
|---|---|--|
| CORS (원본 간 리소스 공유) | yes | [CORS (원본 간 리소스 공유) 구성을](how-to-configure-cross-origin-resource-sharing.md)참조 하세요. |

## <a name="next-steps"></a>다음 단계

- [Azure 서비스에서 기본 제공 되는 보안 컨트롤](../security/fundamentals/security-controls.md)에 대해 자세히 알아보세요.