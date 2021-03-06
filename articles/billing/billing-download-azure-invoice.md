---
title: Microsoft Azure 청구서 보기 및 다운로드
description: Microsoft Azure 청구서를 보고 다운로드하는 방법을 설명합니다.
keywords: 청구서, 청구서 다운로드, azure 청구서, azure 사용 현황
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 7d52a589ee7fcb8891f5ea839b2baeb5e4b30733
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224035"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure 청구서 보기 및 다운로드

대부분의 구독에 대한 청구서는 [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)에서 다운로드하거나 메일로 전송 받을 수 있습니다. 기업계약이 체결된 Azure 고객(EA 고객)의 경우 조직의 청구서를 다운로드할 수 없습니다. 등록에 대한 청구서를 받도록 설정된 사람에게 청구서가 전송됩니다.

특정 역할에만 청구서를 볼 수 있는 권한이 있습니다. 예를 들어 계정 관리자 또는 엔터프라이즈 관리자가 있습니다. 청구 정보에 액세스하는 방법에 대한 자세한 내용은 [역할을 사용하여 Azure 청구에 대한 액세스 관리](billing-manage-access.md)를 참조하세요.

MCA(Microsoft 고객 계약)가 있는 경우 청구서를 받으려면 다음 역할 중 하나가 있어야 합니다.

- 청구 프로필 소유자
- 참가자
- 판독기
- 청구서 관리자

MPA(Microsoft 파트너 계약)가 있는 경우 Azure 청구서를 보고 다운로드하려면 파트너 조직에서 글로벌 관리자 또는 관리 에이전트여야 합니다. [청구 계정 유형을 확인](#check-your-billing-account-type)하여 필요한 사용 권한을 파악합니다.

<!-- For more information about billing roles for Microsoft Customer Agreements, see [Billing profile roles and tasks](billing-understand-mca-roles.md#billing-profile-roles-and-tasks). -->

## <a name="noinvoice"></a> 청구서를 받지 못하는 이유

몇 가지 이유로 인해 청구서가 보이지 않을 수 있습니다.

- Azure를 구독한 지 30일 이내입니다.

- Azure는 청구서 기간이 끝날 때 사용자에게 청구합니다. 따라서 청구서가 아직 생성되지 않았을 수 있습니다. 청구 기간이 끝날 때까지 기다립니다.

- 청구서를 볼 수 있는 권한이 없습니다. MCA 또는 MPA가 있는 경우 청구 프로필 소유자, 기여자, 읽기 권한자 또는 청구서 관리자여야 합니다. 다른 구독의 경우 계정 관리자가 아니면 이전 청구서가 보이지 않을 수 있습니다. 청구 정보에 액세스하는 방법에 대한 자세한 내용은 [역할을 사용하여 Azure 청구에 대한 액세스 관리](billing-manage-access.md)를 참조하세요.

- 구독에 대한 평가판 또는 월별 크레딧 금액이 있는 경우 월별 크레딧 금액을 초과하는 경우에만 청구서를 받게 됩니다. Microsoft 고객 계약 또는 Microsoft 파트너 계약이 있는 경우 항상 청구서를 받게 됩니다.

## <a name="download-invoices-in-the-azure-portal"></a>Azure Portal에서 청구서 다운로드

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. *Cost Management + 청구*를 검색합니다.
1. 액세스 권한에 따라 청구 계정 또는 청구 프로필을 선택해야 할 수도 있습니다.
1. 왼쪽 메뉴에서 **청구** 아래의 **청구서**를 선택합니다.
1. 청구서 그리드에서 다운로드하려는 청구서의 행을 찾습니다.
1. 다운로드 아이콘 또는 행 끝에 있는 줄임표(`...`)를 클릭합니다.
1. 다운로드 메뉴에서 **청구서**를 선택합니다.

청구서에 대한 자세한 내용은 [Microsoft Azure 청구서 이해](billing-understand-your-bill.md)를 참조하세요. 비용 관리에 대한 도움말은 [Azure 청구 및 비용 관리를 사용하여 예상치 못한 비용 방지](billing-getting-started.md)를 참조하세요.

## <a name="get-your-subscriptions-invoices-in-email"></a>이메일로 구독의 청구서 받기

Azure 청구서를 전자 메일로 받을 추가 수취인을 옵트인하고 구성할 수 있습니다. 지원 제안, 기업 계약, Azure in Open 등의 특정 구독에는 이 기능이 제공되지 않을 수도 있습니다.

1. [구독 페이지](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)에서 구독을 선택합니다. 소유하고 있는 각 구독에 대해 옵트인합니다. **청구서**를 클릭한 다음 **전자 메일로 청구서 받기**를 클릭합니다.

    ![옵트인 흐름을 보여주는 스크린샷](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. **옵트인**을 클릭하고 조건에 동의합니다.

    ![옵트인 흐름 2단계를 보여 주는 스크린샷](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. 규약에 동의하면 추가 수취인을 구성할 수 있습니다. 받는 사람이 제거된 경우 전자 메일 주소는 더 이상 저장되지 않습니다. 생각이 바뀌었다면 제거한 받는 사람을 다시 추가해야 합니다.

    ![옵트인 흐름 3단계를 보여 주는 스크린샷](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

이 단계를 따른 후 전자 메일을 받지 못하면 [프로필의 통신 기본 설정](https://account.windowsazure.com/profile)에서 전자 메일 주소가 올바른지 확인합니다.

## <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>이메일에서 구독의 청구서를 가져오는 옵트아웃

이메일로 청구서를 받을 수 있도록 옵트아웃하려면 이전 단계를 수행하고 **이메일로 보낸 청구서 옵트아웃**을 클릭합니다. 그러면 청구서를 메일로 받기 위해 설정된 메일 주소가 모두 제거됩니다. 다시 옵트인하는 경우 받는 사람을 다시 구성할 수 있습니다.

 ![옵트아웃 흐름을 보여 주는 스크린샷](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

<!-- Does following section apply to MPA too? -->
## <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>이메일로 Microsoft 고객 계약 청구서 받기

Microsoft 고객 계약이 있으면 이메일로 청구서 받기로 옵트인할 수 있습니다. 모든 청구 프로필 소유자, 기여자, 독자 및 청구서 관리자는 이메일로 청구서를 받습니다. 독자는 이메일 청구서 기본 설정을 업데이트할 수 없습니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **Cost Management + 청구**를 검색합니다.
1. 청구 프로필을 선택합니다. 액세스 권한에 따라 청구 계정을 먼저 선택해야 할 수도 있습니다.
1. **설정** 아래에서 **속성**을 선택합니다.
1. **이메일 청구서**에서 **이메일 청구서 기본 설정 업데이트**를 선택합니다.

    ![이메일 청구서 속성을 보여 주는 스크린샷](./media/billing-download-azure-invoice/billingprofile-email.png)

1. **옵트인**을 선택합니다.
1. **업데이트**를 클릭합니다.

## <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>이메일에서 Microsoft 고객 계약 청구서 받기 옵트아웃

이메일로 청구서를 받을 수 있도록 옵트아웃하려면 이전 단계를 수행하고 **옵트아웃**을 클릭합니다. 모든 소유자, 기여자, 독자 및 청구서 관리자는 이메일로 청구서를 받도록 옵트아웃됩니다. 독자라면 이메일 청구서 기본 설정을 변경할 수 없습니다.

## <a name="check-your-billing-account-type"></a>청구 계정 유형 확인
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>도움 필요 시 문의하세요.

질문이 있거나 도움이 필요한 경우 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>다음 단계

청구서 및 요금에 대한 자세한 내용은 다음을 참조하세요.

- [Microsoft Azure 사용량 및 요금 보기 및 다운로드](billing-download-azure-daily-usage.md)
- [Microsoft Azure 요금 청구 방식 이해](billing-understand-your-bill.md)
- [Azure 청구서의 용어 이해](billing-understand-your-invoice.md)
- [Microsoft Azure 세부 사용량의 용어 이해](billing-understand-your-usage.md)
- [조직의 Azure 가격 책정 보기](billing-ea-pricing.md)

Microsoft 고객 계약이 있는 경우 다음을 참조하세요.

- [청구 프로필 청구서의 요금 이해](billing-mca-understand-your-bill.md)
- [청구 프로필 청구서의 용어 이해](billing-mca-understand-your-invoice.md)
- [청구 프로필에 대한 Azure 사용량 및 요금 파일 이해](billing-mca-understand-your-usage.md)
- [청구 프로필에 대한 세금 문서 보기 및 다운로드](billing-mca-download-tax-document.md)
- [조직의 Azure 가격 책정 보기](billing-ea-pricing.md)
