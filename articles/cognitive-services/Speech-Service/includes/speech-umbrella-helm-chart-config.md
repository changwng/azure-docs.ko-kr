---
title: 음성 컨테이너 설치
titleSuffix: Azure Cognitive Services
description: 음성 파라솔 투구 차트 구성 옵션에 대해 자세히 설명 합니다.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: ed64412ccf9d192506fafe546b1ccee7941aa43a
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73523411"
---
### <a name="speech-umbrella-chart"></a>Speech (파라솔 차트)

최상위 "파라솔" 차트의 값은 해당 하위 차트 값을 재정의 합니다. 따라서 모든 온-프레미스 사용자 지정 값을 여기에 추가 해야 합니다.

|매개 변수|설명|기본값|
| -- | -- | -- | -- |
| `speechToText.enabled` | **음성 텍스트** 서비스의 사용 여부를 지정 합니다. | `true` |
| `speechToText.verification.enabled` | **음성 텍스트** 서비스에 대 한 `helm test` 기능을 사용할 수 있는지 여부를 나타냅니다. | `true` |
| `speechToText.verification.image.registry` | `helm test`에서 **음성 텍스트** 서비스를 테스트 하는 데 사용 하는 docker 이미지 리포지토리입니다. 투구는 테스트를 위해 클러스터 내부에 별도의 pod를 만들고이 레지스트리에서 *테스트 사용* 이미지를 끌어옵니다. | `docker.io` |
| `speechToText.verification.image.repository` | `helm test`에서 **음성 텍스트** 서비스를 테스트 하는 데 사용 하는 docker 이미지 리포지토리입니다. 투구 테스트 pod는이 리포지토리를 사용 하 여 *테스트 사용* 이미지를 끌어옵니다. | `antsu/on-prem-client` |
| `speechToText.verification.image.tag` | **음성 텍스트** 서비스에 대 한 `helm test`에 사용 되는 docker 이미지 태그입니다. 투구 테스트 pod는이 태그를 사용 하 여 *테스트 사용* 이미지를 가져옵니다. | `latest` |
| `speechToText.verification.image.pullByHash` | *테스트 사용* docker 이미지를 해시로 끌어올 지 여부를 지정 합니다. `true`경우 올바른 이미지 해시 값을 사용 하 여 `speechToText.verification.image.hash`를 추가 해야 합니다. | `false` |
| `speechToText.verification.image.arguments` | *테스트 사용* docker 이미지를 실행 하는 데 사용 되는 인수입니다. 투구 테스트 pod `helm test`실행 될 때 이러한 인수를 컨테이너에 전달 합니다. | `"./speech-to-text-client"`<br/> `"./audio/whatstheweatherlike.wav"` <br/> `"--expect=What's the weather like"`<br/>`"--host=$(SPEECH_TO_TEXT_HOST)"`<br/>`"--port=$(SPEECH_TO_TEXT_PORT)"` |
| `textToSpeech.enabled` | **텍스트 음성 변환** 서비스의 사용 여부를 지정 합니다. | `true` |
| `textToSpeech.verification.enabled` | **음성 텍스트** 서비스에 대 한 `helm test` 기능을 사용할 수 있는지 여부를 나타냅니다. | `true` |
| `textToSpeech.verification.image.registry` | `helm test`에서 **음성 텍스트** 서비스를 테스트 하는 데 사용 하는 docker 이미지 리포지토리입니다. 투구는 테스트를 위해 클러스터 내부에 별도의 pod를 만들고이 레지스트리에서 *테스트 사용* 이미지를 끌어옵니다. | `docker.io` |
| `textToSpeech.verification.image.repository` | `helm test`에서 **음성 텍스트** 서비스를 테스트 하는 데 사용 하는 docker 이미지 리포지토리입니다. 투구 테스트 pod는이 리포지토리를 사용 하 여 *테스트 사용* 이미지를 끌어옵니다. | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | **음성 텍스트** 서비스에 대 한 `helm test`에 사용 되는 docker 이미지 태그입니다. 투구 테스트 pod는이 태그를 사용 하 여 *테스트 사용* 이미지를 가져옵니다. | `latest` |
| `textToSpeech.verification.image.pullByHash` | *테스트 사용* docker 이미지를 해시로 끌어올 지 여부를 지정 합니다. `true`경우 올바른 이미지 해시 값을 사용 하 여 `textToSpeech.verification.image.hash`를 추가 해야 합니다. | `false` |
| `textToSpeech.verification.image.arguments` | *테스트 사용* docker 이미지를 사용 하 여 실행할 인수입니다. 투구 테스트 pod는 `helm test`을 실행할 때 이러한 인수를 컨테이너에 전달 합니다. | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |