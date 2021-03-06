---
title: Azure Site Recovery에서 Hyper-V VM에 대한 장애 조치(failover) 및 장애 복구(failback) 설정
description: Azure Site Recovery 서비스를 사용하여 Azure로 재해 복구 중에 Hyper-V VM 장애 조치(Failover) 및 장애 복구(Failback)를 수행하는 방법을 알아봅니다.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: a8c197c2f0875bb31d091fb5839730ee1568b471
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082642"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>복제된 Hyper-V VM을 Azure로 장애 조치(failover) 및 장애 복구(failback)

이 자습서에서는 Hyper-V VM을 Azure로 장애 조치(failover)하는 방법을 설명합니다. 장애 조치(failover)한 후에 온-프레미스 사이트가 사용 가능해지면 장애 복구(failback)합니다. 이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Hyper-V VM 속성을 확인하여 Azure 요구 사항을 준수하는지 검토
> * Azure에 대한 장애 조치(Failover) 실행
> * Azure에서 온-프레미스로 장애 복구(failback)
> * 온-프레미스 VM을 역방향 복제하고 Azure에 대한 복제 다시 시작

이 자습서는 시리즈의 다섯 번째 자습서입니다. 여기에서는 이전 자습서의 작업을 이미 완료했다고 가정합니다.    

1. [Azure 준비](tutorial-prepare-azure.md)
2. [온-프레미스 Hyper-V 준비](tutorial-prepare-on-premises-hyper-v.md)
3. [Hyper-V VM](tutorial-hyper-v-to-azure.md) 또는 [System Center VMM 클라우드에서 관리되는 Hyper-V VM](tutorial-hyper-v-vmm-to-azure.md)용 재해 복구 설정
4. [재해 복구 훈련 실행](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>장애 조치(failover) 및 장애 복구(failback) 준비

VM에서 스냅샷이 없으며 장애 복구(failback) 시 온-프레미스 VM이 꺼져 있는지 확인해야 합니다. 이렇게 하면 복제 중 데이터 일관성을 보장할 수 있습니다. 장애 복구(failback) 시 온-프레미스 VM을 켜지 마십시오. 

장애 조치(failover)및 장애 복구(failback)는 다음 3단계로 진행됩니다.

1. **Azure로 장애 조치**: Hyper-V VM을 온-프레미스 사이트에서 Azure로 장애 조치합니다.
2. **온-프레미스로 장애 복구**: 온-프레미스 사이트를 사용할 수 있는 경우 Azure VM을 온-프레미스 사이트로 장애 조치합니다. Azure에서 온-프레미스로 데이터 동기화를 시작하고, 완료되면 온-프레미스에서 VM을 실행합니다.  
3. **온-프레미스 VM에 역방향 복제**: 온-프레미스로 장애 복구된 후에 온-프레미스 VM을 역방향으로 복제하여 Azure로 복제를 시작합니다.

## <a name="verify-vm-properties"></a>VM 속성 확인

장애 조치(failover) 전에 VM 속성을 확인하고 VM이 [Azure 요구 사항](hyper-v-azure-support-matrix.md#replicated-vms)을 충족하는지 확인합니다.

**보호된 항목**에서 **복제된 항목** > VM을 클릭합니다.

1. **복제된 항목** 창에 VM 정보, 상태 및 최신 사용 가능한 복구 지점의 요약이 제공됩니다. 자세한 내용을 보려면 **속성**을 클릭합니다.

1. **컴퓨팅 및 네트워크**에서 Azure 이름, 리소스 그룹, 대상 크기, [가용성 집합](../virtual-machines/windows/tutorial-availability-sets.md) 및 관리 디스크 설정을 수정할 수 있습니다.

1. 장애 조치(failover) 후 Azure VM이 배치될 네트워크/서브넷 및 할당되는 IP 주소를 포함한 네트워크 설정을 보고 수정할 수 있습니다.

1. **디스크**에서 VM의 운영 체제 및 데이터 디스크에 대한 정보를 볼 수 있습니다.

## <a name="failover-to-azure"></a>Azure에 장애 조치

1. **설정** > **복제된 항목**에서 VM > **장애 조치(failover)** 를 클릭합니다.
2. **장애 조치(failover)** 에서 **최신** 복구 지점을 선택합니다. 
3. **장애 조치(failover)를 시작하기 전에 컴퓨터를 종료합니다.** 를 선택합니다. Site Recovery는 장애 조치(failover)를 트리거하기 전에 원본 VM 종료를 시도합니다. 종료가 실패하더라도 장애 조치는 계속됩니다. **작업** 페이지에서 장애 조치 진행 상황 확인을 수행할 수 있습니다.
4. 장애 조치(failover)를 확인하고 **커밋**을 클릭합니다. 그러면 사용 가능한 복구 지점이 모두 삭제됩니다.

> [!WARNING]
> **진행 중인 장애 조치(failover)는 취소하지 마세요**. 진행 중인 장애 조치(failover)를 취소하면 장애 조치(failover)가 중지되지만 VM은 다시 복제되지 않습니다.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Azure VM을 온-프레미스로 장애 복구(failback)하고 온-프레미스 VM을 역방향 복제

장애 복구(failback) 작업은 기본적으로 Azure에서 온-프레미스 사이트로의 장애 조치(failover)이며 역방향 복제에서는 온-프레미스 사이트에서 Azure로 VM 복제를 다시 시작합니다.

1. **설정** > **복제된 항목**에서 VM > **계획된 장애 조치(failover)** 를 클릭합니다.
2. **계획된 장애 조치(Failover) 확인**에서 장애 조치(failover) 방향(Azure에서)을 확인하고 원본 위치와 대상 위치를 선택합니다.
3. **장애 조치(failover) 전 데이터 동기화(델타 변경 내용만 동기화)** 를 선택합니다. 이 옵션은 VM을 종료하지 않고 동기화하므로 VM 가동 중지 시간을 최소화합니다.
4. 장애 조치를 시작합니다. **작업** 탭에서 장애 조치 진행 상황을 따를 수 있습니다.
5. 초기 데이터 동기화가 완료되고 Azure VM을 종료할 준비가 되면 **작업** > 계획된 장애 조치(failover) 작업 이름 > **장애 조치(failover) 완료**를 차례로 클릭합니다. Azure VM이 종료되고, 온-프레미스로 최신 변경 내용이 전송되며, 온-프레미스 VM이 시작됩니다.
6. 온-프레미스 VM에 로그온하여 예상대로 사용 가능한지 확인합니다.
7. 온-프레미스 VM은 이제 **커밋 보류 중** 상태가 됩니다. **커밋**을 클릭합니다. Azure VM 및 해당 디스크가 삭제되고, 온-프레미스 VM이 역방향 복제를 위해 준비됩니다.
온-프레미스 VM을 Azure에 복제하기 시작하려면 **역방향 복제**를 사용합니다. Azure VM을 끈 이후에 수행된 델타 변경 내용의 복제가 트리거됩니다.  
