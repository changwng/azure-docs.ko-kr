---
title: 권한 관리란? -Azure AD
description: Azure Active Directory 자격 관리에 대 한 개요와이를 사용 하 여 내부 및 외부 사용자의 그룹, 응용 프로그램 및 SharePoint Online 사이트에 대 한 액세스를 관리 하는 방법을 알아봅니다.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/24/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0a99b9089e568351cf736310e778ba477441407
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422567"
---
# <a name="what-is-azure-ad-entitlement-management"></a>Azure AD 권한 관리란?

Azure AD (Azure Active Directory) 자격 관리는 조직에서 액세스 요청 워크플로, 액세스 할당, 검토 및 만료를 자동화 하 여 규모에 따라 id 및 액세스 수명 주기를 관리할 수 있는 [id 거 버 넌 스](identity-governance-overview.md) 기능입니다.

조직의 직원은 각자의 작업을 수행 하기 위해 다양 한 그룹, 응용 프로그램 및 사이트에 액세스 해야 합니다. 이러한 액세스를 관리 하는 것은 요구 사항이 변경 되 면 새로운 응용 프로그램이 추가 되거나 사용자에 게 추가 액세스 권한이 필요 하므로 어렵습니다.  외부 조직과 공동 작업 하는 경우이 시나리오는 더 복잡해 집니다. 조직의 리소스에 액세스 해야 하는 다른 조직의 사용자를 모를 수 있으며 조직에서 사용 하는 응용 프로그램, 그룹 또는 사이트를 알 수 없습니다.

Azure AD 자격 관리를 사용 하면 내부 사용자의 그룹, 응용 프로그램 및 SharePoint Online 사이트와 이러한 리소스에 액세스 해야 하는 조직 외부 사용자에 대 한 액세스를 보다 효율적으로 관리할 수 있습니다.

## <a name="why-use-entitlement-management"></a>자격 관리를 사용 하는 이유

기업 조직은 종종 다음과 같은 리소스에 대 한 직원 액세스를 관리할 때 문제를 직면 하 게 됩니다.

- 사용자는 액세스 권한이 무엇 인지 알 수 없으며, 사용자가 액세스 권한을 승인 하기 위해 올바른 개인을 찾는 데 어려움이 있을 수 있습니다.
- 사용자가 리소스에 대 한 액세스를 찾고 받은 후에는 비즈니스 목적에 필요한 것 보다 더 긴 액세스를 유지할 수 있습니다.

이러한 문제는 공급망 조직 또는 다른 비즈니스 파트너의 외부 사용자와 같은 다른 조직에서 액세스 해야 하는 사용자에 게 복잡해 집니다. 예:

- 아무도 초대할 수 있는 다른 조직의 디렉터리에 있는 특정 사용자를 모두 알지 못할 수 있습니다.
- 이러한 사용자를 초대할 수 있는 경우에도 해당 조직의 사용자가 모든 사용자의 액세스를 일관 되 게 관리 하는 것을 기억할 수 있는 것은 아닙니다.

Azure AD 자격 관리는 이러한 문제를 해결 하는 데 도움이 될 수 있습니다.  고객이 Azure AD 자격 관리를 사용 하는 방법에 대해 자세히 알아보려면 [Avanade 사례 연구](https://aka.ms/AvanadeELMCase) 및 [Centrica 사례 연구](https://aka.ms/CentricaELMCase)를 읽을 수 있습니다.  이 비디오는 자격 관리 및 해당 값에 대 한 개요를 제공 합니다.

>[!VIDEO https://www.youtube.com/embed/_Lss6bFrnQ8]

## <a name="what-can-i-do-with-entitlement-management"></a>자격 관리로 무엇을 할 수 있나요?

권한 관리의 몇 가지 기능은 다음과 같습니다.

- 관리자가 아닌 관리자에 게 액세스 패키지를 만들 수 있는 권한을 위임 합니다. 이러한 액세스 패키지에는 사용자가 요청할 수 있는 리소스가 포함 되어 있으며, 위임 된 액세스 패키지 관리자는 사용자가 요청할 수 있는 규칙, 액세스를 승인 해야 하는 사용자 및 액세스가 만료 되는 시기에 대 한 규칙을 정의 합니다.
- 사용자가 액세스를 요청할 수 있는 연결 된 조직을 선택 합니다.  디렉터리에 아직 없는 사용자가 액세스를 요청 하 고 승인 되 면 자동으로 디렉터리에 초대 되 고 액세스 권한이 할당 됩니다.  액세스 권한이 만료 되 면 다른 액세스 패키지 할당이 없는 경우 디렉터리의 B2B 계정을 자동으로 제거할 수 있습니다.

자습서를 시작 [하 여 첫 번째 액세스 패키지를 만들](entitlement-management-access-package-first.md)수 있습니다. 다음을 포함 하 여 [일반적인 시나리오](entitlement-management-scenarios.md)를 읽거나 비디오를 볼 수도 있습니다.

- [조직에서 Azure AD 자격 관리를 배포 하는 방법](https://www.youtube.com/watch?v=zaaKvaaYwI4)
- [Azure AD 자격 관리의 사용을 모니터링 하 고 크기를 조정 하는 방법](https://www.youtube.com/watch?v=omtNJ7ySjS0)
- [자격 관리에서 위임 하는 방법](https://www.youtube.com/watch?v=Fmp1eBxzrqw)

## <a name="what-are-access-packages-and-what-resources-can-i-manage-with-them"></a>액세스 패키지는 무엇 이며 어떤 리소스를 사용 하 여 관리할 수 있나요?

자격 관리는 *액세스 패키지*의 개념을 Azure AD에 도입 합니다. 액세스 패키지는 사용자가 프로젝트에서 작업을 수행 하거나 작업을 수행 하는 데 필요한 액세스 권한이 있는 모든 리소스의 번들입니다. 액세스 패키지는 내부 직원 및 조직 외부 사용자에 대 한 액세스를 제어 하는 데 사용 됩니다.

 권한 관리를 사용 하 여에 대 한 사용자의 액세스를 관리할 수 있는 리소스 유형은 다음과 같습니다.

- Azure AD 보안 그룹의 멤버 자격
- Office 365 그룹 및 팀의 구성원
- 페더레이션/single sign on 및/또는 프로 비전을 지 원하는 SaaS 응용 프로그램 및 사용자 지정 통합 응용 프로그램을 포함 하 여 Azure AD 엔터프라이즈 응용 프로그램에 할당
- SharePoint Online 사이트의 멤버 자격

Azure AD 보안 그룹 또는 Office 365 그룹을 사용 하는 다른 리소스에 대 한 액세스를 제어할 수도 있습니다.  예:

- 액세스 패키지의 Azure AD 보안 그룹을 사용 하 고 해당 그룹에 대 한 [그룹 기반 라이선스](../users-groups-roles/licensing-groups-assign.md) 를 구성 하 여 Microsoft Office 365에 대 한 라이선스를 사용자에 게 제공할 수 있습니다.
- 액세스 패키지의 Azure AD 보안 그룹을 사용 하 여 Azure 리소스를 관리 하 고 해당 그룹에 대 한 [azure 역할 할당](../../role-based-access-control/role-assignments-portal.md) 을 만들어 사용자에 게 액세스 권한을 부여할 수 있습니다.

## <a name="how-do-i-control-who-gets-access"></a>액세스 권한을 받는 사용자를 제어 어떻게 할까요?

액세스 패키지를 사용 하면 관리자 또는 위임 된 액세스 패키지 관리자가 리소스 (그룹, 앱 및 사이트)와 해당 리소스에 대해 사용자가 필요로 하는 역할을 나열 합니다.

액세스 패키지에는 하나 이상의 *정책도*포함 됩니다. 정책은 패키지 액세스에 대 한 할당에 대 한 규칙 또는 guardrails를 정의 합니다. 각 정책을 사용 하면 적절 한 사용자만 액세스를 요청할 수 있고, 요청에 대 한 승인자가 있으며, 해당 리소스에 대 한 액세스 권한이 제한 되어 갱신 되지 않은 경우 만료 됩니다.

![패키지 및 정책 액세스](./media/entitlement-management-overview/elm-overview-access-package.png)

각 정책 내에서 관리자 또는 액세스 패키지 관리자는 다음을 정의 합니다.

- 이미 존재 하는 사용자 (일반적으로 직원 또는 이미 초대한 게스트) 또는 외부 사용자의 파트너 조직 (액세스를 요청 하는 데 적합 함)
- 승인 프로세스 및 액세스를 승인 하거나 거부할 수 있는 사용자
- 할당이 만료 되기 전에 승인 되 면 사용자의 액세스 할당 기간

다음 다이어그램은 자격 관리의 다양 한 요소에 대 한 예를 보여 줍니다. 두 개의 예제 액세스 패키지를 포함 하는 하나의 카탈로그를 표시 합니다.

- **액세스 패키지 1** 에는 단일 그룹인 리소스로 포함 됩니다. 액세스는 디렉터리의 사용자 집합에서 액세스를 요청할 수 있도록 하는 정책으로 정의 됩니다.
- **액세스 패키지 2** 는 그룹, 응용 프로그램 및 SharePoint Online 사이트를 리소스로 포함 합니다. 액세스는 두 개의 서로 다른 정책을 사용 하 여 정의 됩니다. 첫 번째 정책을 사용 하면 디렉터리의 사용자 집합에서 액세스를 요청할 수 있습니다. 두 번째 정책을 사용 하면 외부 디렉터리의 사용자가 액세스를 요청할 수 있습니다.

![자격 관리 개요](./media/entitlement-management-overview/elm-overview.png)

## <a name="when-should-i-use-access-packages"></a>액세스 패키지는 언제 사용 해야 하나요?

액세스 패키지는 액세스 할당을 위해 다른 메커니즘을 대체 하지 않습니다.  이러한 작업은 다음과 같은 상황에서 가장 적합 합니다.

- 직원에 게는 특정 작업에 대 한 시간이 제한 된 액세스가 필요 합니다.  예를 들어 그룹 기반 라이선스 및 동적 그룹을 사용 하 여 모든 직원에 게 Exchange Online 사서함이 있는지 확인 한 다음, 직원이 다른 직원의 부서 리소스를 읽는 등의 추가 액세스 권한이 필요한 상황에 액세스 패키지를 사용할 수 있습니다. 부서로.
- 직원의 관리자 또는 다른 지정 된 개인에 게 액세스 권한을 승인 해야 합니다.
- 부서는 IT 참여 없이 자신의 리소스에 대 한 자신의 액세스 정책을 관리 하려고 합니다.  
- 둘 이상의 조직이 프로젝트에서 공동 작업 하 고 있으므로 한 조직의 여러 사용자를 Azure AD B2B를 통해 가져와서 다른 조직의 리소스에 액세스 해야 합니다.

## <a name="how-do-i-delegate-access"></a>위임 액세스를 어떻게 할까요? 하 시겠습니까?

 액세스 패키지는 카탈로그(*catalogs*)라는 컨테이너에 정의됩니다.  모든 액세스 패키지에 단일 카탈로그를 사용 하거나 고유한 카탈로그를 만들고 소유 하도록 개인을 지정할 수 있습니다. 관리자는 모든 카탈로그에 리소스를 추가할 수 있지만 관리자가 아닌 사용자는 자신이 소유한 리소스만 카탈로그에 추가할 수 있습니다. 카탈로그 소유자는 다른 사용자를 카탈로그 공동 소유자로 추가 하거나 패키지 관리자에 액세스할 수 있습니다.  이러한 시나리오는 [AZURE AD 자격 관리의 위임 및 역할](entitlement-management-delegate.md)문서에 자세히 설명 되어 있습니다.

## <a name="summary-of-terminology"></a>용어 요약

권한 관리 및 해당 설명서를 더 잘 이해 하기 위해 다음 조건 목록을 다시 참조할 수 있습니다.

| 조건 | Description |
| --- | --- |
| 액세스 패키지 | 팀 또는 프로젝트에서 필요로 하 고 정책을 적용 하는 리소스 번들입니다. 액세스 패키지는 항상 카탈로그에 포함 되어 있습니다. 사용자가 액세스를 요청 해야 하는 시나리오에 대 한 새 액세스 패키지를 만듭니다.  |
| 액세스 요청 | 액세스 패키지의 리소스에 액세스 하는 요청입니다. 요청은 일반적으로 승인 워크플로를 거칩니다.  승인 된 경우 요청 하는 사용자는 액세스 패키지 할당을 수신 합니다. |
| 대입 | 사용자에 게 액세스 패키지를 할당 하면 해당 액세스 패키지의 모든 리소스 역할이 사용자에 게 할당 됩니다.  액세스 패키지 할당은 일반적으로 만료 되기 전에 시간 제한이 있습니다. |
| 카탈로그 | 관련 리소스 및 액세스 패키지의 컨테이너입니다.  카탈로그는 위임에 사용 되므로 관리자가 아닌 사용자가 자신의 액세스 패키지를 만들 수 있습니다. 카탈로그 소유자는 자신이 소유한 리소스를 카탈로그에 추가할 수 있습니다. |
| 카탈로그 작성자 | 새 카탈로그를 만들 수 있는 권한이 있는 사용자의 컬렉션입니다.  카탈로그 작성자로 인증 된 관리자가 아닌 사용자가 새 카탈로그를 만들 경우 자동으로 해당 카탈로그의 소유자가 됩니다. |
| 연결 된 조직 | 관계가 있는 외부 Azure AD 디렉터리 또는 도메인입니다. 연결 된 조직의 사용자는 액세스 요청을 허용 하는 정책에 지정할 수 있습니다. |
| policy | 액세스 수명 주기를 정의 하는 규칙 집합 (예: 사용자가 액세스 하는 방법, 승인할 수 있는 사용자 및 할당을 통해 사용자에 게 액세스 하는 시간) 정책은 액세스 패키지에 연결 됩니다. 예를 들어 액세스 패키지에는 직원 들이 액세스를 요청 하 고 다른 하나는 외부 사용자가 액세스를 요청 하는 두 개의 정책이 있을 수 있습니다. |
| resource | 사용자에 게 사용 권한을 부여할 수 있는 역할을 가진 Office 그룹, 보안 그룹, 응용 프로그램 또는 SharePoint Online 사이트와 같은 자산입니다. |
| 리소스 디렉터리 | 공유할 리소스가 하나 이상 있는 디렉터리입니다. |
| 리소스 역할 | 리소스에 의해 정의 되 고 정의 된 사용 권한 컬렉션입니다. 그룹에는 구성원 및 소유자의 두 역할이 있습니다. SharePoint 사이트에는 일반적으로 3 개의 역할이 있지만 추가 사용자 지정 역할이 있을 수 있습니다. 응용 프로그램에는 사용자 지정 역할이 있을 수 있습니다. |


## <a name="license-requirements"></a>라이선스 요구 사항

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Azure Government, Azure 독일 및 Azure 중국 21Vianet과 같은 특수 클라우드는 현재 사용할 수 없습니다.

### <a name="which-users-must-have-licenses"></a>어떤 사용자에게 라이선스가 있어야 하나요?

권한 부여 관리에서 구성원 사용자가 활성화 되어 있으므로 테 넌 트에는 적어도 Azure AD Premium P2 라이선스가 있어야 합니다. 자격 관리의 활성 구성원 사용자는 다음과 같습니다.

- 액세스 패키지에 대 한 요청을 시작 하거나 승인 하는 사용자입니다.
- 액세스 패키지가 할당 된 사용자입니다.
- 액세스 패키지를 관리 하는 사용자입니다.

멤버 사용자에 대 한 라이선스의 일부로 많은 게스트 사용자가 자격 관리와 상호 작용 하도록 허용할 수도 있습니다. 포함할 수 있는 게스트 사용자 수를 계산 하는 방법에 대 한 자세한 내용은 [AZURE ACTIVE DIRECTORY B2B 공동 작업 라이선스 지침](../b2b/licensing-guidance.md)을 참조 하세요.

사용자에 게 라이선스를 할당 하는 방법에 대 한 자세한 내용은 [Azure Active Directory 포털을 사용 하 여 라이선스 할당 또는 제거](../fundamentals/license-users-groups.md)를 참조 하세요. 자격 관리는 현재 사용자에 대 한 라이선스 할당을 적용 하지 않습니다.

## <a name="next-steps"></a>다음 단계

- [자습서: 첫 번째 액세스 패키지 만들기](entitlement-management-access-package-first.md)
- [일반적인 시나리오](entitlement-management-scenarios.md)
