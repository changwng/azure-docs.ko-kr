---
title: Machine Learning 알고리즘 참고 자료 시트
titleSuffix: Azure Machine Learning
description: 인쇄 가능한 Machine Learning 알고리즘 참고 자료 시트를 사용 하면 Azure Machine Learning 디자이너에서 예측 모델에 적합 한 알고리즘을 선택할 수 있습니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: FrancescaLazzeri
ms.author: lazzeri
ms.date: 11/04/2019
ms.openlocfilehash: b43f2f351345f05c3eb56a84fb1a0eadb4826707
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771515"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-azure-machine-learning-designer"></a>Azure Machine Learning 디자이너에 대 한 Machine Learning 알고리즘 참고 자료 시트

**Azure Machine Learning 알고리즘 치트 시트**를 사용하면 예측 분석 모델에 대한 적합한 알고리즘을 선택할 수 있습니다.

Azure Machine Learning에는 ***분류***, ***추천 시스템***, ***클러스터링***, ***변칙 검색***, ***회귀*** 및 ***텍스트 분석*** 제품군의 많은 알고리즘 라이브러리가 있습니다. 각 항목은 다양한 유형의 기계 학습 문제를 해결하기 위해 고안되었습니다.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>다운로드: Machine Learning 알고리즘 참고 자료 시트

**여기서 치트 시트 다운로드: [Machine Learning 알고리즘 치트 시트(11x17인치)](https://download.microsoft.com/download/3/5/b/35bb997f-a8c7-485d-8c56-19444dafd757/azure-machine-learning-algorithm-cheat-sheet-nov2019.pdf?WT.mc_id=docs-article-lazzeri)**

![Machine Learning 알고리즘 참고 자료 시트: Machine Learning 알고리즘을 선택 하는 방법에 대해 알아봅니다.](./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet.svg)

간편하게 보관하면서 알고리즘을 선택할 수 있도록 tabloid 크기로 Machine Learning 알고리즘 치트 시트를 다운로드하여 인쇄합니다.

## <a name="how-to-use-the-machine-learning-algorithm-cheat-sheet"></a>Machine Learning 알고리즘 참고 자료 시트를 사용 하는 방법

알고리즘 치트 시트에서 제공된 제안 사항은 대략적인 위치 조정 규칙입니다. 제안을 변형하거나 명백하게 위반할 수 있습니다. 시작점을 제안하기 위한 용도로만 제공됩니다. 데이터에 대해 여러 알고리즘을 실행하고 비교해 보세요. 각 알고리즘과 데이터를 생성 한 시스템의 원칙을 이해 하는 것은 간단 하지 않습니다.

모든 기계 학습 알고리즘에는 고유한 스타일 또는 귀납적 바이어스가 있습니다. 특정 문제에 대해 여러 알고리즘이 적합할 수 있으며 한 알고리즘이 다른 알고리즘 보다 더 적합할 수 있습니다. 하지만 어떤 것이 가장 적합한지 항상 미리 알 수 없습니다. 이러한 경우 여러 알고리즘이 참고 자료에 함께 나열됩니다. 적절한 전략은 한 알고리즘을 시도하고, 결과가 만족스럽지 않을 경우 다른 알고리즘을 시도하는 것입니다. 

Azure Machine Learning 알고리즘에 대 한 자세한 내용을 보려면 [알고리즘 및 모듈 참조](algorithm-module-reference/module-reference.md)로 이동 하세요.

## <a name="kinds-of-machine-learning"></a>기계 학습의 종류

Machine Learning의 세 가지 주요 범주는 *감독 학습*, *자율 학습* 및 *보충 학습*입니다.

### <a name="supervised-learning"></a>감독 학습

감독 학습에서 각 데이터 요소는 범주 또는 관련 값에 대 한 레이블 또는 연결 됩니다. 범주 레이블의 예는 이미지를 'cat' 또는 'dog'로 할당하는 것입니다. 값 레이블의 예는 중고차와 연결된 판매 가격입니다. 감독 학습의 목표는 이와 같은 많은 레이블이 지정된 예제를 연구한 다음 향후 데이터 요소에 대한 예측을 수행할 수 있도록 하는 것입니다. 예를 들어 올바른 동물로 새 사진을 식별하거나 다른 중고 자동차에 정확한 판매 가격을 할당합니다. 이는 널리 사용되고 유용한 기계 학습 유형입니다.

### <a name="unsupervised-learning"></a>자율 학습

자율 학습에서는 데이터 요소에 연결된 레이블이 없습니다. 대신, 자율 학습 알고리즘의 목표는 어떤 방식으로든 데이터를 구성하거나 해당 구조를 설명하는 것입니다. 이는 K-Means처럼 클러스터로 그룹화하거나 더 간단하게 표시되도록 복잡한 데이터를 보는 다양한 방법을 찾는 것을 의미할 수 있습니다.

### <a name="reinforcement-learning"></a>강화 학습

보충 학습에서는 알고리즘이 각 데이터 요소에 대한 응답으로 작업을 선택합니다. 이는 지정 시간 센서 판독값 집합이 한 데이터 요소이며 알고리즘으로 로봇의 다음 동작을 선택해야 하는 로봇 공학의 일반적인 접근 방식입니다. 사물 인터넷의 애플리케이션에 적합한 학습이기도 합니다. 또한 학습 알고리즘은 짧은 시간 후에 결정이 얼마나 효율적이었는지를 나타내는 보상 신호를 받습니다. 이 신호에 따라 알고리즘은 가장 높은 보상을 달성하기 위해 해당 전략을 수정합니다. 

## <a name="next-steps"></a>다음 단계

* [Azure Machine Learning 및 Azure Portal의 스튜디오에 대해 알아봅니다](overview-what-is-azure-ml.md).

* [알고리즘 및 모듈 참조](algorithm-module-reference/module-reference.md)에서 알고리즘 및 모듈의 목록을 참조 하세요.

* [자습서: Azure Machine Learning designer에서 예측 모델을 작성](tutorial-designer-automobile-price-train-score.md)합니다.

* [심층 학습 및 기계 학습에 대해 알아봅니다](concept-deep-learning-vs-machine-learning.md).
