---
title: QnA Maker 서비스란?
titleSuffix: Azure Cognitive Services
description: QnA Maker는 데이터를 통해 자연스러운 대화형 계층을 쉽게 만드는 클라우드 기반 NLP 서비스입니다. 사용자 지정 KB(기술 자료) 정보에서 지정된 자연어 입력에 가장 적합한 대답을 찾는 데 사용할 수 있습니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 11/22/2019
ms.author: diberry
ms.openlocfilehash: 944ddb7f83a4d10861e5a16dbc69b8f9e4dabfe0
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422674"
---
# <a name="what-is-the-qna-maker-service"></a>QnA Maker 서비스란?

QnA Maker는 데이터를 통해 자연스러운 대화형 계층을 쉽게 만드는 클라우드 기반 NLP(자연어 처리) 서비스입니다. 사용자 지정 KB(기술 자료) 정보에서 지정된 자연어 입력에 가장 적합한 대답을 찾는 데 사용할 수 있습니다.

QnA Maker용 클라이언트 애플리케이션은 사용자와 자연어로 통신하여 질문에 대답하는 대화형 애플리케이션입니다. 클라이언트 애플리케이션의 예로는 소셜 미디어 앱, 챗봇 및 음성 지원 데스크톱 애플리케이션을 들 수 있습니다.

## <a name="when-to-use-qna-maker"></a>QnA Maker를 사용하는 경우

* **정적 정보가 있는 경우** - 정적 정보가 대답의 기술 자료에 있는 경우 QnA Maker를 사용합니다. 이 기술 자료는 사용자의 요구에 맞게 사용자 지정되며, [PDF 및 URL](../concepts/data-sources-supported.md)과 같은 문서를 사용하여 작성되었습니다.
* **동일한 대답을 요청, 질문 또는 명령에 제공하려는 경우** - 다른 사용자가 동일한 질문을 제출하면 동일한 대답이 반환됩니다. 
* **메타 정보에 따라 정적 정보를 필터링하려는 경우** - [메타데이터](../how-to/metadata-generateanswer-usage.md) 태그를 추가하여 클라이언트 애플리케이션의 사용자 및 정보와 관련된 추가 필터링 옵션을 제공합니다. 일반 메타데이터 정보에는 [잡담](../how-to/chit-chat-knowledge-base.md)(chit-chat), 콘텐츠 형식 또는 형식, 콘텐츠 용도 및 콘텐츠 새로 고침이 포함됩니다.
* **정적 정보가 포함된 봇 대화를 관리하려는 경우** - 기술 자료에서 사용자의 대화형 텍스트 또는 명령을 사용하여 대답합니다. 대답이 [다중 턴 컨텍스트](../how-to/multiturn-conversation.md)를 사용하여 기술 자료에 표시된 미리 결정된 대화 흐름의 일부인 경우 봇에서 이 흐름을 쉽게 제공할 수 있습니다.  

## <a name="use-qna-maker-knowledge-base-in-a-chat-bot"></a>채팅 봇에서 QnA Maker 기술 자료 사용

QnA Maker 기술 자료가 게시되면 클라이언트 애플리케이션에서 질문을 기술 자료 엔드포인트에 보내고 결과를 JSON 응답으로 받습니다. 일반적인 QnA Maker용 클라이언트 애플리케이션은 채팅 봇입니다.

![봇에 질문을 하고 기술 자료 콘텐츠에서 대답을 받습니다.](../media/qnamaker-overview-learnabout/bot-chat-with-qnamaker.png)

|단계|조치|
|:--|:--|
|1|클라이언트 애플리케이션에서 사용자 고유 단어의 텍스트인 사용자의 _질문_("내 기술 자료를 프로그래밍 방식으로 업데이트하려면 어떻게 해야 하나요?")을 기술 자료 엔드포인트에 보냅니다.|
|2|QnA Maker에서 학습된 기술 자료를 사용하여 정확한 대답과 최상의 대답 검색을 구체화하는 데 사용할 수 있는 추가 작업 프롬프트를 제공합니다. QnA Maker에서 JSON 형식의 응답을 반환합니다.|
|3|클라이언트 애플리케이션에서 JSON 응답을 사용하여 대화를 계속하는 방법을 결정합니다. 이러한 결정에는 최상의 대답을 표시하고 최상의 대답 검색을 구체화하기 위한 더 많은 선택 항목을 제시하는 것이 포함될 수 있습니다. |
|||

## <a name="what-is-a-knowledge-base"></a>기술 자료란? 

QnA Maker는 사용자 콘텐츠를 질문 및 대답 세트의 기술 자료로 [가져옵니다](../concepts/data-sources-supported.md). 가져오기 프로세스는 정형 콘텐츠와 반정형 콘텐츠의 부분 간의 관계에 대한 정보를 추출하여 질문과 대답 세트 간의 관계를 암시합니다. 이러한 질문과 대답 세트를 편집하거나 새 세트를 추가할 수 있습니다.  

질문 및 답변 세트의 내용에는 다음이 포함됩니다.
* 모든 형식의 대체 질문
* 검색 중에 답변 선택을 필터링하는 데 사용되는 메타데이터 태그
* 검색 구체화를 계속하는 후속 프롬프트

![메타데이터를 사용한 질문 및 대답 예제](../media/qnamaker-overview-learnabout/example-question-and-answer-with-metadata.png)

기술 자료가 게시되면 클라이언트 애플리케이션에서 사용자의 질문을 엔드포인트에 보냅니다. QnA Maker 서비스는 질문을 처리하고 최상의 대답으로 응답합니다. 

## <a name="create-manage-and-publish-to-a-bot-without-code"></a>코드 없이 봇 만들기, 관리 및 게시

QnA Maker 포털은 완벽한 기술 자료 작성 환경을 제공합니다. 현재 양식의 문서를 기술 자료로 가져올 수 있습니다. 이러한 문서(예: FAQ, 제품 설명서, 스프레드시트 또는 웹 페이지)는 질문 및 대답 세트로 변환됩니다. 각 세트는 추가 작업 프롬프트에 대해 분석되고 다른 세트에 연결됩니다. 최종 _markdown_ 형식은 이미지와 링크를 포함하여 다양한 표현을 지원합니다. 

기술 자료가 편집되면 아무 코드도 작성하지 않고 기술 자료를 작업 중인 [Azure Web App 봇](https://azure.microsoft.com/services/bot-service/)에 게시합니다. [Azure Portal](https://portal.azure.com)에서 봇을 테스트하거나 다운로드하여 개발을 계속합니다. 

## <a name="search-quality-and-ranking-provides-the-best-possible-answer"></a>검색 품질 및 순위 지정을 통해 최상의 대답 제공

QnA Maker의 시스템은 계층화된 순위 지정 방법입니다. 데이터는 첫 번째 순위 지정 계층으로도 사용되는 Azure Search에 저장됩니다. 그런 다음, Azure Search의 상위 결과는 QnA Maker의 NLP 순위 다시 지정 모델을 통과하여 최종 결과와 신뢰도 점수를 생성합니다.

## <a name="qna-maker-improves-the-conversation-process"></a>QnA Maker를 통해 대화 프로세스 향상

QnA Maker는 기본 질문 및 대답 세트를 향상시키는 데 도움이 되는 다중 턴 프롬프트 및 활성 학습을 제공합니다. 

**다중 턴 프롬프트**는 질문 및 대답 쌍을 연결할 수 있는 기회를 제공합니다. 이 연결을 통해 클라이언트 애플리케이션은 최상의 대답을 제공하고, 최종 대답의 검색을 구체화하기 위한 추가 질문을 제공합니다. 

기술 자료가 게시된 엔드포인트에서 사용자의 질문을 받으면 QnA Maker에서 **활성 학습**을 이러한 실제 질문에 적용하여 품질을 향상시키기 위해 기술 자료의 변경을 제안합니다. 

## <a name="development-lifecycle"></a>개발 수명 주기

QnA Maker는 협업 권한과 함께 작성, 학습 및 게시를 제공하여 전체 개발 수명 주기에 통합합니다. 

## <a name="how-do-i-start"></a>시작하는 방법

**1단계**: [Azure Portal](https://portal.azure.com)에서 QnA Maker 리소스를 만듭니다. 

**2단계**: [QnA Maker](https://www.qnamaker.ai) 포털에서 기술 자료를 만듭니다. [파일 및 URL](../concepts/data-sources-supported.md)을 추가하여 기술 자료를 만듭니다.  

**3단계**: [cURL 또는 Postman](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md)을 사용하여 기술 자료를 게시하고 사용자 지정 엔드포인트에서 테스트합니다. 

**4단계**: 클라이언트 애플리케이션에서 기술 자료의 엔드포인트를 프로그래밍 방식으로 호출합니다. 클라이언트 애플리케이션이 JSON 응답을 처리하여 사용자에게 가장 적합한 답변을 표시합니다.  

## <a name="next-steps"></a>다음 단계
QnA Maker는 사용자 지정 기술 자료를 작성, 관리 및 배포하는 데 필요한 모든 것을 제공합니다. 

> [!div class="nextstepaction"]
> [최신 변경 내용 검토](../whats-new.md)
