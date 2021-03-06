---
title: Azure Lab Services의 클래스 유형 예제 | Microsoft Docs
description: Azure Lab Services를 사용하여 랩을 설정할 수 있는 몇 가지 유형의 클래스를 제공합니다.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: e1c5504b30c2784e8657ccc0dc4ec18689fe2a68
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806816"
---
# <a name="class-types-overview---azure-lab-services"></a>클래스 유형 개요 - Azure Lab Services

Azure Lab Services를 사용하면 클라우드에서 클래스룸 랩 환경을 빠르게 설정할 수 있습니다. 이 섹션의 문서에서는 Azure Lab Services를 사용하여 여러 유형의 클래스룸 랩을 설정하는 방법에 대한 지침을 제공합니다.

## <a name="deep-learning-in-natural-language-processing"></a>자연어 처리의 딥 러닝

Azure Lab Services를 사용하여 NLP(자연어 처리)의 딥 러닝에 초점을 맞춘 랩을 설정할 수 있습니다. NLP(자연어 처리)는 번역, 음성 인식 및 기타 언어 이해 기능을 갖춘 컴퓨터를 가능케하는 AI(인공 지능) 형태입니다. NLP 클래스를 수강하는 학생은 Linux VM(가상 머신)을 통해 작성된 인간 언어 분석에 사용되는 딥 러닝 모델을 개발하는 신경망 알고리즘을 적용하는 방법을 알아봅니다.

이러한 유형의 랩을 설정하는 방법에 대한 자세한 내용은 [Azure Lab Services를 사용한 자연어 처리의 딥 러닝에 초점을 맞춘 랩 설정](class-type-deep-learning-natural-processing.md)을 참조하세요.

## <a name="shell-scripting-on-linux"></a>Linux에서 셸 스크립팅

Linux에서 셸 스크립팅을 학습하도록 랩을 설정할 수 있습니다. 스크립팅은 관리자가 반복적인 작업을 방지할 수 있도록 하는 시스템 관리의 유용한 부분입니다. 이 샘플 시나리오에서 클래스는 기존 bash 스크립트와 향상된 스크립트를 포함합니다. 향상된 스크립트는 bash 명령과 Ruby를 결합하는 스크립트입니다. 이 접근 방식을 사용하면 Ruby에서 데이터를 전달하고, bash 명령이 셸과 상호 작용할 수 있습니다.

이러한 스크립팅 클래스를 수강하는 학생은 Linux 가상 머신을 통해 Linux의 기본 사항을 알아보고 bash 셸 스크립팅에 익숙해질 수 있습니다. Linux 가상 머신은 원격 데스크톱 액세스를 사용하도록 설정되어 있고 [gedit](https://help.gnome.org/users/gedit/stable/) 및 [Visual Studio Code](https://code.visualstudio.com/) 텍스트 편집기가 설치 되어 있습니다.

이 유형의 랩을 설정하는 방법에 대한 자세한 내용은 [Linux에서 셸 스크립팅](class-type-shell-scripting-linux.md)을 참조하세요.

## <a name="ethical-hacking"></a>윤리적 해킹

윤리적 해킹의 법적 고지에 초점을 맞춘 클래스에 대해 랩을 설정할 수 있습니다. 윤리적 해킹 커뮤니티에서 사용하는 방법인 침투 테스트는 누군가가 악의적인 공격자가 악용할 수 있는 취약성을 입증하기 위해 시스템 또는 네트워크에 대한 액세스 권한을 얻으려고 할 때 발생합니다.

윤리적 해킹 클래스에서 학생은 취약점을 방어하기 위한 최신 기술을 배울 수 있습니다. 각 학생에게는 두 개의 중첩된 가상 머신이 있는 Windows Server 호스트 가상 머신이 있습니다. 즉 [Metasploitable3](https://github.com/rapid7/metasploitable3) 이미지가 있는 가상 머신과 [Kali Linux](https://www.kali.org/) 이미지가 있는 가상 머신이 있습니다. Metasploitable 가상 머신은 악용 목적으로 사용됩니다.  Kali Linux 가상 머신은 법정 작업을 실행하는 데 필요한 도구에 대한 액세스를 제공합니다.

이 유형의 랩을 설정하는 방법에 대한 자세한 내용은 [윤리적 해킹 클래스를 가르치기 위한 랩 설정](class-type-ethical-hacking.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

다음 문서를 참조하세요.

- [Azure Lab Services를 사용하여 자연어 처리의 딥 러닝에 초점을 맞춘 랩을 설정](class-type-deep-learning-natural-processing.md)
- [Linux에서 셸 스크립팅](class-type-shell-scripting-linux.md)
- [윤리적 해킹](class-type-ethical-hacking.md)
