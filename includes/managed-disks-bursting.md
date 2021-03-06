---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7564d40aa9344288c0368818b0b6501dc22a5a27
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74705513"
---
디스크 버스트는 현재 프리미엄 Ssd의 미리 보기 기능입니다. 버스트는 프리미엄 SSD 디스크 크기 < = 512 GiB (P20)에서 지원 됩니다. 이러한 디스크 크기는 최고 노력으로 버스트를 지원 하 고 신용 시스템을 활용 하 여 버스트를 관리 합니다. 크레딧을 디스크 크기에 대 한 프로 비전 된 성능 목표 미만이 될 때마다 버스트 버킷에 누적 되며, 목표를 초과 하는 트래픽 버스트 시 크레딧을 소비 합니다. 디스크 트래픽은 프로 비전 된 대상의 IOPS 및 대역폭 모두에 대해 추적 됩니다.

디스크 버스트는이를 지 원하는 디스크 크기의 새 배포에서 기본적으로 사용 하도록 설정 됩니다. 디스크 버스트를 지 원하는 경우 기존 디스크 크기는 다음 방법 중 하나를 통해 버스트를 사용 하도록 설정할 수 있습니다.

- 디스크를 분리 하 고 다시 연결 합니다.
- VM을 중지 하 고 시작 합니다.

## <a name="burst-states"></a>버스트 상태

디스크가 가상 컴퓨터에 연결 되어 있는 경우 모든 버스트 디스크 크기는 전체 버스트 크레딧 버킷으로 시작 됩니다. 버스트의 최대 기간은 버스트 크레딧 버킷의 크기에 따라 결정 됩니다. 사용 되지 않는 크레딧을 신용 버킷의 크기까지 누적 시킬 수 있습니다. 언제 든 지 디스크 버스트 크레딧 버킷은 다음 세 가지 상태 중 하나일 수 있습니다. 

- 발생 디스크 트래픽이 프로 비전 된 성능 목표 보다 작은 경우에 사용 됩니다. 디스크 트래픽이 IOPS 또는 대역폭 목표를 초과 하거나 둘 다를 초과 하는 경우 크레딧을 누적 할 수 있습니다. 전체 디스크 대역폭을 사용 하는 경우에도 IO 크레딧을 누적 할 수 있습니다.  

- 거절-디스크 트래픽이 프로 비전 된 성능 목표를 초과 하 여 사용 하는 경우 버스트 트래픽은 IOPS 또는 대역폭의 크레딧을 독립적으로 사용 합니다. 

- 나머지 상수 (디스크 트래픽이 프로 비전 된 성능 목표에 정확히 일치 하는 경우) 

버스트 사양과 함께 버스트 지원을 제공 하는 디스크 크기는 아래 표에 요약 되어 있습니다.

## <a name="regional-availability"></a>지역별 가용성

현재 디스크 버스트는 미국 서 부 지역 에서만 사용할 수 있습니다.

## <a name="disk-sizes"></a>디스크 크기

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="example-scenarios"></a>예제 시나리오

이 작업을 수행 하는 방법을 더 잘 이해 하기 위해 몇 가지 예제 시나리오는 다음과 같습니다.

- 디스크 버스트의 이점을 누릴 수 있는 일반적인 시나리오 중 하나는 OS 디스크에서 VM 부팅 및 응용 프로그램 시작 속도 보다 빠릅니다. 8 GiB OS 이미지를 포함 하는 Linux VM을 예로 들어 보겠습니다. P2 디스크를 OS 디스크로 사용 하는 경우 프로 비전 된 대상은 120 IOPS 및 25mbps입니다. VM이 시작 되 면 부팅 파일을 로드 하는 OS 디스크에 대 한 읽기 급증이 발생 합니다. 버스트의 도입으로 3500 IOPS 및 170 MBps의 최대 버스트 속도로 읽어 로드 시간을 6x 이상으로 가속화할 수 있습니다. 응용 프로그램에 대 한 대부분의 데이터 작업은 연결 된 데이터 디스크에 대 한 것 이므로 VM 부팅 후에는 일반적으로 OS 디스크의 트래픽 수준이 낮습니다. 트래픽이 프로 비전 된 대상 보다 낮은 경우 크레딧을 누적 됩니다.

- 원격 가상 데스크톱 환경을 호스트 하는 경우 활성 사용자가 AutoCAD와 같은 응용 프로그램을 시작할 때마다 OS 디스크에 대 한 읽기 트래픽이 크게 증가 합니다. 이 경우 버스트 트래픽은 누적 크레딧을 소비 하므로 프로 비전 된 목표를 벗어나 응용 프로그램을 훨씬 빠르게 시작할 수 있습니다.

- P1 디스크의 프로 비전 된 대상은 120 IOPS 및 25mbps입니다. 디스크의 실제 트래픽이 지난 1 초 간격으로 100 IOPS 및 20Mbps 인 경우 사용 하지 않은 20 개의 Io 및 5mb는 디스크의 버스트 버킷으로 제공한 됩니다. 트래픽이 최대 버스트 제한까지 프로 비전 된 목표를 초과 하는 경우 버스트 버킷의 크레딧을 나중에 사용할 수 있습니다. 최대 버스트 제한은 사용할 버스트 크레딧을 보유 하 고 있는 경우에도 디스크 트래픽 상한을 정의 합니다. 이 경우 신용 버킷에 1만 IOs가 있는 경우에도 P1 디스크는 초당 3500 IO의 최대 버스트 수를 초과 하 여 발급할 수 없습니다.  
