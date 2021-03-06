---
title: 데이터 흐름 서로게이트 키 변환 매핑
description: Azure Data Factory의 데이터 흐름의 매핑 서로게이트 키 변환을 사용 하 여 순차적 키 값을 생성 하는 방법
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: bab48aa9079c1b8020bb828a6bb91bd244a78cf1
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930211"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>데이터 흐름 서로게이트 키 변환 매핑



서로게이트 키 변환을 사용하여 증가하는 비업무용 임의 키 값을 데이터 흐름 행 집합에 추가합니다. 이는 차원 테이블의 각 구성원이 Kimball DW 방법론의 일부인 비업무용 키인 고유 키를 가져야 하는 별모양 스키마 분석 데이터 모델에서 차원 테이블을 디자인할 때 유용합니다.

![서로게이트 키 변환](media/data-flow/surrogate.png "서로게이트 키 변환")

"키 열"은 새로운 서로게이트 키 열에 제공할 이름입니다.

"시작 값"은 증분 값의 시작점입니다.

## <a name="increment-keys-from-existing-sources"></a>기존 원본에서 키 증가

원본에 있는 값에서 시퀀스를 시작 하려는 경우 서로게이트 키 변환 바로 다음에 파생 열 변환을 사용 하 고 두 값을 함께 추가할 수 있습니다.

![최대 추가](media/data-flow/sk006.png "서로게이트 키 변환 최대값 추가")

키 값을 이전 max로 초기값으로 사용할 수 있는 두 가지 방법이 있습니다.

### <a name="database-sources"></a>데이터베이스 원본

원본 변환을 사용 하 여 원본에서 MAX ()를 선택 하려면 "쿼리" 옵션을 사용 합니다.

![서로게이트 키 쿼리](media/data-flow/sk002.png "서로게이트 키 변환 쿼리")

### <a name="file-sources"></a>파일 원본

이전 max 값이 파일에 있으면 집계 변환과 함께 원본 변환을 사용 하 고 MAX () 식 함수를 사용 하 여 이전 max 값을 가져올 수 있습니다.

![서로게이트 키 파일](media/data-flow/sk008.png "서로게이트 키 파일")

두 경우 모두 들어오는 새 데이터를 이전 max 값을 포함 하는 원본과 함께 조인 해야 합니다.

![서로게이트 키 조인](media/data-flow/sk004.png "서로게이트 키 조인")

## <a name="next-steps"></a>다음 단계

이 예에서는 [조인](data-flow-join.md) 및 [파생 열](data-flow-derived-column.md) 변환을 사용 합니다.
