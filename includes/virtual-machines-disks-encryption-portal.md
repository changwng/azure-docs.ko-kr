---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/10/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 165a115f35810c1bfe463a571affb2e44ed74205
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75893831"
---
### <a name="portal"></a>포털

디스크에 대해 고객이 관리 하는 키를 설정 하려면 특정 순서로 리소스를 만들어야 합니다 .이 작업을 처음 수행 하는 경우에는이 작업을 수행 해야 합니다. 먼저 Azure Key Vault를 만들고 설정 해야 합니다.

#### <a name="setting-up-your-azure-key-vault"></a>Azure Key Vault 설정

1. [Azure Portal](https://portal.azure.com/) 에 로그인 하 여 Key Vault를 검색 합니다.
1. **키 자격 증명 모음**을 검색 하 고 선택 합니다.

![sse-key-vault-portal-search](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)

> [!IMPORTANT]
> 배포에 성공 하려면 Azure 주요 자격 증명 모음, 디스크 암호화 집합, VM, 디스크 및 스냅숏이 모두 동일한 지역 및 구독에 있어야 합니다.

1. **+ 추가** 를 선택 하 여 새 Key Vault을 만듭니다.
1. 새 리소스 그룹 만들기
1. 키 자격 증명 모음 이름을 입력 하 고, 지역을 선택 하 고, 가격 책정 계층을 선택 합니다.
1. **검토 + 만들기**를 선택 하 고, 선택 사항을 확인 한 다음, **만들기**를 선택 합니다.

![sse-create-a-key-vault](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. 키 자격 증명 모음이 배포를 완료 한 후 선택 합니다.
1. **설정**에서 **키** 를 선택 합니다.
1. **생성/가져오기** 선택

![sse-key-vault-generate-settings](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. **Rsa** 와 **rsa 키 크기** 를 모두 **2080**로 설정 하 여 두 **키 유형을** 모두 유지 합니다.
1. 원하는 대로 나머지 선택 항목을 입력 하 고 **만들기**를 선택 합니다.

![sse-create-a-key-generate](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>디스크 암호화 집합 설정

디스크 암호화 집합을 만들고 구성 하려면 다음 링크를 사용 해야 합니다. https://aka.ms/diskencryptionsets. 공용 Azure Portal에서 디스크 암호화 집합 만들기를 아직 사용할 수 없습니다.

1. [디스크 암호화 세트 링크](https://aka.ms/diskencryptionsets)를 엽니다.
1. **+추가**를 선택합니다.

![sse-create-disk-encryption-set](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. 리소스 그룹을 선택 하 고, 암호화 집합의 이름을로 설정 하 고, 키 자격 증명 모음과 동일한 지역을 선택 합니다.
1. **키 자격 증명 모음 및 키를**선택 합니다.
1. 이전에 만든 키 자격 증명 모음 및 키와 버전을 선택 합니다.
1. **선택**을 누릅니다.
1. **검토 + 만들기** 및 **만들기**를 차례로 선택 합니다.

![sse-disk-enc-set-blade-key](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. 만들기가 완료 되 면 디스크 암호화 집합을 열고 팝업 되는 경고를 선택 합니다.

![sse-disk-enc-alert-fix](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

두 개의 알림이 표시 되 고 성공 합니다. 이렇게 하면 키 자격 증명 모음에 집합을 사용할 수 있습니다.

![disk-enc-notification-success](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>VM 배포

이제 키 자격 증명 모음 및 디스크 암호화 집합을 만들고 설정 했으므로 암호화를 사용 하 여 VM을 배포할 수 있습니다.
VM 배포 프로세스는 표준 배포 프로세스와 유사 합니다. 유일한 차이점은 다른 리소스와 동일한 지역에 VM을 배포 하 고 고객 관리 키를 사용 하도록 선택 하는 것입니다.

1. **Virtual Machines** 를 검색 하 고 **+ 추가** 를 선택 하 여 VM을 만듭니다.
1. **기본** 탭에서 디스크 암호화 집합과 동일한 지역을 선택 하 고 Azure Key Vault 합니다.
1. **기본** 탭에 있는 다른 값을 원하는 대로 입력 합니다.

![sse-create-a-vm-region](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. **디스크** 탭에서 **고객이 관리 하는 키를 사용 하 여 미사용 암호화**를 선택 합니다.
1. **디스크 암호화 집합** 드롭다운에서 설정 된 디스크 암호화를 선택 합니다.
1. 원하는 대로 나머지 항목을 선택 합니다.

![sse-create-vm-select-cmk-encryption-set](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)