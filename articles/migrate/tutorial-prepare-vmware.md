---
title: Azure Migrate를 사용하여 평가/마이그레이션을 위해 VMware VM 준비
description: Azure Migrate를 사용하여 VMware VM의 평가/마이그레이션을 준비하는 방법을 알아봅니다.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: cc1eb4c4fce1398365145b2f3d63db984635d667
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196217"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>평가 후 Azure로 마이그레이션할 VMware VM 준비

이 문서는 [Azure Migrate](migrate-services-overview.md)를 사용하여 온-프레미스 VMware VM을 평가하고 Azure로 마이그레이션하기 위해 준비하는 데 유용합니다.

[Azure Migrate](migrate-overview.md)는 앱, 인프라 및 워크로드를 검색, 평가 및 Microsoft Azure로 마이그레이션하는 데 도움이 되는 도구의 허브를 제공합니다. 허브에는 Azure Migrate 도구와 타사 ISV(독립 소프트웨어 공급업체) 제품이 포함되어 있습니다.


이 자습서는 VMware VM을 평가하고 마이그레이션하는 방법을 보여 주는 시리즈의 두 번째 자습서입니다. 이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Azure Migrate를 사용할 수 있게 Azure를 준비합니다.
> * VMware의 VM 평가를 준비합니다.
> * Mware의 VM 마이그레이션을 준비합니다.

> [!NOTE]
> 자습서는 시나리오에 맞는 가장 간단한 배포 경로를 보여줍니다. 배포를 설정하는 방법을 배우고 빠르게 개념을 증명하는 데 유용합니다. 자습서는 가능한 경우 기본 옵션을 사용하며, 가능한 모든 설정과 경로는 보여 주지 않습니다. 자세한 지침은 VMware 평가 및 마이그레이션 방법을 검토하세요.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/pricing/free-trial/)을 만듭니다.


## <a name="prepare-azure"></a>Azure 준비

다음 권한이 필요합니다.

**Task** | **권한**
--- | ---
**Azure Migrate 프로젝트 만들기** | Azure 계정에는 프로젝트를 만들 수 있는 권한이 필요합니다.
**Azure Migrate 어플라이언스 등록** | Azure Migrate는 경량 Azure Migrate 어플라이언스를 사용하여 Azure Migrate 서버 평가를 통해 VMware VM을 평가하고, Azure Migrate 서버 평가를 사용해서 VMware VM의 [에이전트 없는 마이그레이션](server-migrate-overview.md)을 실행합니다. 이 어플라이언스는 VM을 검색하고 VM 메타데이터 및 성능 데이터를 Azure Migrate로 보냅니다.<br/><br/>등록하는 동안 Azure Migrate는 어플라이언스를 고유하게 식별하는 두 개의 Azure AD(Azure Active Directory) 앱을 만들며, 이러한 앱을 만들기 위한 권한이 필요합니다.<br/> - 첫 번째 앱은 Azure Migrate 서비스 엔드포인트와 통신합니다.<br/> - 두 번째 앱은 등록 중에 만든 Azure Key Vault에 액세스하여 Azure AD 앱 정보 및 어플라이언스 구성 설정을 저장합니다.
**Key Vault 만들기** | Azure Migrate 서버 마이그레이션을 사용하여 VMware VM을 마이그레이션하기 위해 Azure Migrate는 Key Vault를 만들어 구독의 복제 스토리지 계정에 대한 액세스 키를 관리합니다. 자격 증명 모음을 만들려면 Azure Migrate 프로젝트가 있는 리소스 그룹에 대한 역할 할당 권한이 필요합니다.


### <a name="assign-permissions-to-create-project"></a>프로젝트를 만들 수 있는 권한 할당

1. Azure Portal에서 구독을 열고 **액세스 제어(IAM)** 를 선택합니다.
2. **액세스 확인**에서 관련 계정을 찾아서 클릭하여 권한을 확인합니다.
3. **기여자** 또는 **소유자** 권한이 있어야 합니다.
    - Azure 체험 계정을 방금 만든 경우 자신이 구독에 대한 소유자입니다.
    - 구독 소유자가 아닌 경우 해당 역할을 할당해 주도록 소유자에게 문의합니다.

### <a name="assign-permissions-to-register-the-appliance"></a>어플라이언스를 등록할 수 있는 권한 할당

어플라이언스를 등록하려면 어플라이언스 등록 중에 Azure AD 앱을 만들기 위해 Azure Migrate에 대한 권한을 할당합니다. 다음 방법 중 하나를 사용하여 권한을 할당할 수 있습니다.

- 테넌트/글로벌 관리자는 Azure AD 앱을 만들고 등록할 수 있는 권한을 테넌트의 사용자에게 부여할 수 있습니다.
- 테넌트/글로벌 관리자는 권한이 있는 애플리케이션 개발자 역할을 계정에 할당할 수 있습니다.

> [!NOTE]
> - 앱에는 위에서 설명한 구독에 대한 액세스 권한 이외의 다른 권한이 없습니다.
> - 이러한 권한은 새 어플라이언스를 등록할 때만 필요합니다. 어플라이언스가 설정되면 해당 권한을 제거할 수 있습니다.


#### <a name="grant-account-permissions"></a>계정 권한 부여

테넌트/글로벌 관리자는 다음과 같이 권한을 부여할 수 있습니다.

1. Azure AD에서 테넌트/글로벌 관리자는 **Azure Active Directory** > **사용자** > **사용자 설정**으로 이동해야 합니다.
2. 관리자는 **앱 등록**을 **예**로 설정해야 합니다. 이것이 중요한 내용이 포함되지 않는 기본 설정입니다. [자세히 알아보기](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Azure AD 권한](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>애플리케이션 개발자 역할 할당

테넌트/글로벌 관리자는 애플리케이션 개발자 역할을 계정에 할당할 수 있습니다. [자세히 알아보기](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

### <a name="assign-role-assignment-permissions"></a>역할 할당 권한 할당

Azure Migrate에서 Key Vault를 만들도록 설정하려면 다음과 같이 역할 할당 권한을 할당합니다.

1. Azure Portal의 리소스 그룹에서 **액세스 제어(IAM)** 를 선택합니다.
2. **액세스 확인**에서 관련 계정을 찾아서 클릭하여 권한을 확인합니다.

    - 서버 평가를 실행하는 데는 **기여자** 권한으로 충분합니다.
    - 에이전트 없는 서버 마이그레이션을 실행하려면 **소유자**(또는 **기여자** 및 **사용자 액세스 관리자**) 권한이 있어야 합니다.

3. 필요한 권한이 없으면 리소스 그룹 소유자에게 요청합니다.



## <a name="prepare-for-vmware-vm-assessment"></a>VMware VM 평가 준비

VMware VM 평가를 준비하려면 다음을 수행해야 합니다.

- **VMware 설정 확인**. 마이그레이션하려는 vCenter Server 및 VM이 요구 사항을 충족하는지 확인합니다.
- **평가 계정 설정**. 평가를 위해 VM을 검색하려면 Azure Migrate에서 vCenter Server에 액세스해야 합니다.
- **어플라이언스 요구 사항 확인**. 평가에 사용되는 Azure Migrate 어플라이언스에 대한 배포 요구 사항을 확인합니다.

### <a name="verify-vmware-settings"></a>VMware 설정 확인

1. 평가에 필요한 VMware 서버 요구 사항을 [확인](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements)합니다.
2. 필요한 포트가 vCenter Server에서 열려 있는지 [확인](migrate-support-matrix-vmware.md#assessment-port-requirements)합니다.


### <a name="set-up-an-account-for-assessment"></a>평가를 위한 계정 설정

평가 및 에이전트 없는 마이그레이션을 위해 VM을 검색하려면 Azure Migrate에서 vCenter Server에 액세스해야 합니다.

- 애플리케이션을 검색하거나 에이전트 없는 방식으로 종속성을 시각화하려는 경우 **가상 머신** > **게스트 작업**에 대해 사용하도록 설정된 권한과 함께 읽기 전용 액세스 권한이 있는 vCenter Server 계정을 만듭니다.

  ![vCenter Server 계정 권한](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- 애플리케이션 검색 및 에이전트 없는 종속성 시각화를 수행하지 않으려는 경우 vCenter Server에 대한 읽기 전용 계정을 설정합니다.

### <a name="verify-appliance-settings-for-assessment"></a>평가를 위한 어플라이언스 설정 확인

어플라이언스를 배포하기 전에 어플라이언스 요구 사항을 확인하세요.

1. 어플라이언스 요구 사항 및 제한 사항을 [확인](migrate-support-matrix-vmware.md#assessment-appliance-requirements)합니다.
2. URL 기반 방화벽 프록시를 사용하는 경우 어플라이언스에서 액세스해야 하는 Azure URL을 [검토](migrate-support-matrix-vmware.md#assessment-url-access-requirements)합니다. URL을 조회하는 동안 수신된 CNAME 레코드를 프록시가 확인해야 합니다.
3. 검색 및 평가 중에 어플라이언스가 수집하는 [성능 데이터](migrate-appliance.md#collected-performance-data-vmware) 및 [메타데이터](migrate-appliance.md#collected-metadata-vmware)를 검토합니다.
4. 어플라이언스에서 액세스하는 포트를 [적어둡니다](migrate-support-matrix-vmware.md#assessment-port-requirements).
5. vCenter Server에서 계정에 OVA 파일을 사용하여 VM을 만들 수 있는 권한이 있는지 확인합니다. OVA 파일을 사용하여 Azure Migrate 어플라이언스를 VMware VM으로 배포합니다.

URL 기반 firewall.proxy를 사용하는 경우 필요한 [Azure URL](migrate-support-matrix-vmware.md#assessment-url-access-requirements)에 대한 액세스를 허용하세요.




## <a name="prepare-for-agentless-vmware-migration"></a>에이전트 없는 VMware 마이그레이션 준비

VMware VM의 에이전트 없는 마이그레이션에 대한 요구 사항을 검토합니다.

1. VMware 서버 요구 사항을 [검토](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements)합니다.
2. Azure Migrate에서 Azure Migrate 서버 마이그레이션을 사용하여 에이전트 없는 마이그레이션을 위해 vCenter Server에 액세스할 수 있도록 [필요한 권한](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions)으로 계정을 설정합니다.
3. 에이전트 없는 마이그레이션을 사용하여 Azure로 마이그레이션하려는 VMware VM에 대한 요구 사항을 [검토](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements)합니다.
4. 에이전트 없는 마이그레이션에 Azure Migrate 어플라이언스를 사용하기 위한 요구 사항을 [검토](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements)합니다.
5. Azure Migrate 어플라이언스에서 에이전트 없는 마이그레이션에 필요한 [URL 액세스](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) 및 [포트 액세스](migrate-support-matrix-vmware.md#agentless-migration-port-requirements)를 적어둡니다.


## <a name="prepare-for-agent-based-vmware-migration"></a>에이전트 기반 VMware 마이그레이션 준비

VMware VM의 [에이전트 기반 마이그레이션](server-migrate-overview.md)에 대한 요구 사항을 검토합니다.

1. VMware 서버 요구 사항을 [검토](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements)합니다.
2. [필요한 권한](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions)이 있는 계정을 설정하여 Azure Migrate에서 Azure Migrate 서버 마이그레이션을 통해 에이전트 기반 마이그레이션을 위해 vCenter Server에 액세스할 수 있도록 합니다.
3. 마이그레이션할 각 VM에 Mobility Service 설치를 포함하여, 에이전트 기반 마이그레이션을 사용해 Azure로 마이그레이션하려는 VMware VM에 대한 요구 사항을 [검토](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements)합니다.
4. [URL 액세스](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements)에 유의하세요.
5. Azure Migrate 구성 요소의 에이전트 기반 액세스에 필요한 [포트 액세스](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements)를 검토합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음을 수행합니다.

> [!div class="checklist"]
> * Azure 사용 권한 설정
> * VMware 평가 및 마이그레이션 준비


Azure Migrate 프로젝트를 설정하고 Azure로 마이그레이션하기 위해 VMware VM을 평가하려면 두 번째 자습서로 진행합니다.

> [!div class="nextstepaction"]
> [VMware VM 평가](./tutorial-assess-vmware.md)
