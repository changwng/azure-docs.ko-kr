---
title: Azure DevTest Labs에서 VM에 대 한 자동 종료 설정 구성 Microsoft Docs
description: Vm이 사용 되지 않을 때 vm이 자동으로 종료 되도록 vm (가상 머신)에 대 한 자동 종료 설정을 구성 하는 방법에 대해 알아봅니다.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2019
ms.author: spelluru
ms.openlocfilehash: 934e8fd71c901c89f328c777103a8cb39bf21ac4
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68361571"
---
# <a name="configure-autoshutdown-settings-for-a-vm-in-azure-devtest-labs"></a>Azure DevTest Labs에서 VM에 대 한 자동 종료 설정 구성
Azure DevTest Labs를 통해 각 랩에 대한 정책(설정)을 관리하여 비용을 제어하고 랩에서의 낭비를 최소화할 수 있습니다. 이 문서에서는 랩 계정에 대 한 자동 종료 정책을 구성 하 고 랩 계정에서 랩에 대 한 자동 종료 설정을 구성 하는 방법을 보여 줍니다. 모든 랩 정책을 설정하는 방법을 보려면 [Azure DevTest Labs에서 랩 정책 정의](devtest-lab-set-lab-policy.md)를 참조하세요.  

## <a name="autoshutdown-policy-for-a-lab"></a>랩에 대 한 autoshutdown 정책
랩 소유자로서 랩에서 모든 VM에 대한 종료 일정을 구성할 수 있습니다. 이 작업을 수행하면 사용되지 않는(유휴 상태) 머신의 실행 비용을 절약할 수 있습니다. 모든 랩 VM에서 중앙 집중식으로 종료 정책을 적용할 수 있을 뿐만 아니라 랩 사용자가 개별 머신에 대한 일정을 설정하는 수고를 줄일 수도 있습니다. 이 기능을 사용하면 랩 사용자에게 모든 권한에 대한 권한 없음 제공에서 시작하는 랩 일정에 대한 정책을 설정할 수 있습니다. 랩 소유자로서 다음 단계를 수행하여 이 정책을 구성할 수 있습니다.

1. 랩의 홈 페이지에서 **구성 및 정책**을 선택합니다.
2. 왼쪽 메뉴의 **일정** 섹션에서 **자동 종료 정책**을 선택합니다.
3. 옵션 중 하나를 선택합니다. 다음 섹션에서는 이러한 옵션에 대 한 자세한 정보를 제공 합니다. 설정 정책은 기존 Vm이 아닌 랩에서 생성 된 새 Vm에만 적용 됩니다. 

    ![자동 종료 정책 옵션](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-autoshutdown-settings-for-a-lab"></a>랩에 대 한 자동 종료 설정 구성
자동 종료 정책은이 랩의 vm이 종료 되는 시간을 지정할 수 있도록 하 여 랩 낭비를 최소화 하는 데 도움이 됩니다.

랩에 대한 정책을 보거나 변경하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. **모든 서비스**를 선택한 다음 목록에서 **DevTest Labs**를 선택합니다.
3. 랩 목록에서 원하는 랩을 탭합니다.   
4. **구성 및 정책**을 선택합니다.

    ![정책 설정 창](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. 랩의 **구성 및 정책** 창의 **일정**에서 **자동 종료** 를 선택 합니다.
   
    ![자동 종료](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. 이 정책을 사용하도록 설정하려면 **설정**을 선택하고 사용하지 않도록 설정하려면 **해제**를 선택합니다.
7. 이 정책을 사용하도록 설정한 경우 현재 랩의 모든 VM을 종료하는 시간(및 표준 시간대)을 지정합니다.
8. 지정 된 자동 종료 시간 보다 15 분 전에 알림을 보내는 옵션에 대해 **예** 또는 **아니요** 를 지정 합니다. **예**를 선택할 경우 알림을 게시하거나 보낼 위치를 지정하는 이메일 주소 또는 웹후크 URL 엔드포인트를 입력합니다. 사용자에게 알림이 전송되고 종료를 지연할 수 있는 옵션이 제공됩니다. 자세한 내용은 [알림](#notifications) 섹션을 참조 하세요. 
9.           **저장**을 선택합니다.

    기본적으로 이 정책을 사용하도록 설정하면 현재 랩의 모든 VM에 적용됩니다. 특정 VM에서이 설정을 제거 하려면 VM의 관리 창을 열고 **자동 종료** 설정을 변경 합니다.

## <a name="configure-autoshutdown-settings-for-a-vm"></a>VM에 대 한 자동 종료 설정 구성

### <a name="user-sets-a-schedule-and-can-opt-out"></a>사용자가 일정을 설정하고 옵트아웃할 수 있음
랩에 대해이 정책 옵션을 설정 하면 사용자가 랩 일정을 재정의 하거나 옵트아웃 (opt out) 할 수 있습니다. 이 옵션은 랩 사용자에게 해당 VM의 자동 종료 일정에 대한 모든 권한을 부여합니다. 랩 사용자는 해당 VM 자동 종료 일정 페이지에 변경 내용이 표시되지 않습니다.

![자동 종료 정책 옵션-1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>사용자가 일정을 설정하지만 옵트아웃할 수 없음
랩에 대해이 정책 옵션을 설정 하면 사용자가 랩 일정을 재정의할 수 있습니다. 그러나 자동 종료 정책을 옵트아웃할 수 없습니다. 이 옵션을 통해 랩의 모든 머신이 자동 종료 일정에 포함됩니다. 랩 사용자는 해당 VM의 자동 종료 일정을 업데이트하고, 종료 알림을 설정할 수 있습니다.

![자동 종료 정책 옵션-2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>랩 관리자가 설정한 일정을 사용자가 제어할 수 없음
랩에 대해이 정책 옵션이 설정 된 경우 사용자는 랩 일정을 재정의 하거나 옵트아웃 (opt out) 할 수 없습니다. 이 옵션은 랩 관리자에게 랩의 모든 머신에 대한 일정의 완전한 권한을 제공합니다. 랩 사용자는 해당 VM에 대한 자동 종료 알림만 설정할 수 있습니다.

![자동 종료 정책 옵션-3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>알림
자동 종료가 랩 소유자에 의해 설정 되 면 Vm의 영향을 받는 경우 자동 종료를 트리거하는 데 15 분 전에 랩 사용자에 게 알림이 전송 됩니다. 이 옵션을 사용 하면 랩 사용자는 종료 되기 전에 작업을 저장할 수 있습니다. 또한 알림은 각 VM에 대 한 링크를 제공 하 여 다음 작업을 수행 합니다.

- 이 시간 동안 자동 종료 건너뛰기
- VM에서 계속 작업을 수행할 수 있도록 1 시간 또는 2 시간의 자동 종료를 다시 알림 합니다.

알림은 구성 된 웹 후크 끝점 또는 자동 종료 설정의 랩 소유자가 지정한 전자 메일 주소를 통해 전송 됩니다. 웹 후크를 사용 하면 특정 이벤트를 구독 하는 통합을 빌드하거나 설정할 수 있습니다. 이러한 이벤트 중 하나가 트리거되면 DevTest Labs는 HTTP POST 페이로드를 webhook의 구성 된 URL에 보냅니다. 웹후크에 대한 자세한 내용은 [웹후크 또는 API Azure Function 만들기](../azure-functions/functions-create-a-web-hook-or-api-function.md)를 참조하세요. 

웹 후크는 다양 한 앱에서 광범위 하 게 지원 되므로 (예: 여유 시간, Azure Logic Apps 등) 웹 후크를 사용 하는 것이 좋습니다 .이를 통해 알림을 보내는 고유한 방법을 구현할 수 있습니다. Azure Logic Apps를 사용 하 여 전자 메일에서 자동 종료 알림을 받는 방법에 대 한 예제는[전자 메일 알림을 수신 하는 논리 앱 만들기](devtest-lab-auto-shutdown.md#create-a-logic-app-that-receives-email-notifications)를 참조 하세요. 


## <a name="next-steps"></a>다음 단계
[Azure DevTest Labs에서 랩에 대 한 자동 종료 정책 관리](devtest-lab-auto-shutdown.md) 를 참조 하세요.
