---
title: Author 엔터티 특성 - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Academic Knowledge API의 Author 엔터티와 함께 사용할 수 있는 특성에 대해 알아봅니다.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: d5fc770c380397f605f8810fa41d3a8907f2358e
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146498"
---
# <a name="author-entity"></a>Author 엔터티

> [!NOTE]
> 다음 특성은 author 엔터티와 관련 됩니다. (Ty = ' 1 ')

name | 설명 | Type | 운영
--- | --- | --- | ---
Id      | 엔터티 ID                             |Int64      |같음
AuN     | 작성자의 정규화된 이름                    |string     |같음
CC      | 작성자의 총 인용 횟수           |Int32      |없음  
DAuN    | 작성자 표시 이름                   |string     |없음
E | 확장 메타 데이터</br></br>**중요**:이 특성은 사용 되지 않으며 레거시 응용 프로그램에만 지원 됩니다. 이 특성을 개별적으로 요청 (즉, 특성 = Id, Ti, E) 하면 모든 확장 메타 데이터 특성이 *직렬화 된 JSON 문자열* 에서 반환 됩니다.</br></br>이제 확장 된 메타 데이터에 포함 된 모든 특성을 최상위 특성으로 사용할 수 있으며, 이러한 특성 (예: attributes = Id, Ti, DOI, IA)으로 요청할 수 있습니다. | [확장이](#extended) | 없음
ECC     | 작성자의 총 예상 인용 횟수 |Int32      |없음
LKA.AfId | 작성자에 대해 발견 된 마지막으로 알려진 항목의 엔터티 ID | Int64 | 없음
LKA. AfN | 작성자에 대해 발견 된 마지막으로 알려진 회원의 정규화 된 이름입니다. | string | 없음
PC | 작성자 총 게시 수 | Int32 | 없음

## <a name="extended"></a>확장이

> [!IMPORTANT]
> 이 특성은 더 이상 사용 되지 않으며 레거시 응용 프로그램에 대해서만 지원 됩니다. 이 특성을 개별적으로 요청 (즉, 특성 = Id, Ti, E) 하면 모든 확장 메타 데이터 특성이 *직렬화 된 JSON 문자열* 에서 반환 됩니다.</br></br>이제 확장 된 메타 데이터에 포함 된 모든 특성을 최상위 특성으로 사용할 수 있으며, 이러한 특성 (예: attributes = Id, Ti, DOI, IA)으로 요청할 수 있습니다.

> [!IMPORTANT]
> "E"를 사용 하 여 개별 확장 특성 요청을 지원 합니다. 범위 (예: "E. DN")는 더 이상 사용 되지 않습니다. 이는 여전히 기술적으로 지원 되지만 "E"를 사용 하 여 개별 확장 특성을 요청 합니다. 범위는 JSON 응답의 두 위치 ("E" 개체의 일부로, 최상위 특성)로 특성 값을 반환 합니다.

name | 설명 | Type | 운영
--- | --- | --- | ---
LKA.AfId | 작성자에 대해 발견 된 마지막으로 알려진 항목의 엔터티 ID | Int64 | 없음
LKA. AfN | 작성자에 대해 발견 된 마지막으로 알려진 회원의 정규화 된 이름입니다. | string | 없음
PC | 작성자 총 게시 수 | Int32 | 없음
