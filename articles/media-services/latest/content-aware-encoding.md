---
title: 콘텐츠 인식 인코딩에 대 한 실험적 사전 설정-Azure | Microsoft Docs
description: 이 문서에서는 Microsoft Azure Media Services v3의 콘텐츠 인식 인코딩에 대해 설명 합니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9389466b6291542563c068706479bf981c5880da
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692759"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>콘텐츠 인식 인코딩에 대 한 실험적 사전 설정

[적응 비트 전송률 스트리밍을](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)통해 콘텐츠를 준비할 수 있도록 하려면 비디오를 여러 비트 전송률 (높음-낮음)으로 인코딩해야 합니다. 비트 전송률이 감소 하므로 품질의 정상 저하를 보장 하기 위해는 비디오 해상도입니다. 이로 인해이를 위한 인코딩 사다리 (해상도 및 비트 전송률 표)가 생성 됩니다. [기본 제공 인코딩 기본 설정](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset)Media Services을 참조 하세요.

## <a name="overview"></a>개요

Netflix가 12 월 2015에 [블로그](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2) 를 게시 한 후 미리 설정 된 전체 비디오 접근 방식을 벗어나 이동 하는 것이 중요 합니다. 이후 콘텐츠 인식 인코딩에 대 한 여러 솔루션이 marketplace에서 릴리스 되었습니다. 개요는 [이 문서](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx) 를 참조 하세요. 개념은 개별 비디오의 복잡성에 맞게 인코딩 사다리를 사용자 지정 하거나 조정 하기 위해 콘텐츠를 인식 하는 것입니다. 각 해상도에서 품질 증가는 perceptive 하지 않는 비트 전송률이 있습니다. 인코더는이 최적의 비트 전송률 값에서 작동 합니다. 다음 수준의 최적화는 콘텐츠를 기반으로 해상도를 선택 하는 것입니다. 예를 들어 PowerPoint 프레젠테이션 비디오에서는 720p를 사용 하는 것이 좋습니다. 더 나아가 인코더는 비디오 내에서 각 샷의 설정을 최적화 하는 일을 할 수 있습니다. Netflix에서는 2018의 [이러한 접근 방식을](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830) 설명 했습니다.

2017 초기에 Microsoft는 [적응 스트리밍](autogen-bitrate-ladder.md) 사전 설정을 출시 하 여 원본 비디오의 품질 및 해상도에 대 한 변동 문제를 해결 했습니다. 고객은 다양 한 콘텐츠를 포함 하 고 있습니다. 일부는 1080p, 720p에서는 몇 가지를 포함 하 고 있습니다. 또한 모든 소스 콘텐츠가 필름 또는 TV 스튜디오의 고품질 mezzanines. 적응 스트리밍 사전 설정은 비트 전송률 사다리가 입력 된 메자닌의 해상도 또는 평균 비트 전송률을 초과 하지 않도록 하 여 이러한 문제를 해결 합니다.

실험적 콘텐츠 인식 인코딩 사전 설정은 인코더가 지정 된 해상도에 대 한 최적의 비트 전송률 값을 검색할 수 있도록 하는 사용자 지정 논리를 통합 하 여 광범위 한 계산 분석을 요구 하지 않고 해당 메커니즘을 확장 합니다. 결과적으로이 새로운 사전 설정은 적응 스트리밍 사전 설정 보다 높은 비트 전송률이 있는 출력을 생성 하지만 품질은 높아집니다. [Psnr](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) 및 [vmaf](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion)와 같은 품질 메트릭을 사용 하 여 비교를 보여 주는 다음 샘플 그래프를 참조 하세요. 소스는 인코더를 스트레스 하기 위해 영화 및 TV 쇼에서 복잡성이 높은 샷의 짧은 클립을 연결 하 여 만들어졌습니다. 정의에 따라이 미리 설정은 콘텐츠와 콘텐츠가 다른 결과를 생성 합니다. 또한 일부 콘텐츠의 경우 비트 전송률 또는 품질 향상을 크게 줄일 수 없습니다.

![PSNR을 사용 하는 TS (요율 비틀기) 곡선](media/cae-experimental/msrv1.png)

**그림 1: 높은 복잡성의 원본에 대 한 PSNR 메트릭을 사용 하는 RD (요율 비틀기) 곡선**

![VMAF를 사용 하는 TS (요율 비틀기) 곡선](media/cae-experimental/msrv2.png)

**그림 2: 높은 복잡성의 원본에 대 한 VMAF 메트릭을 사용 하는 TS (요율 비틀기) 곡선**

현재 미리 설정은 높은 복잡성, 고품질 원본 비디오 (영화, TV 쇼)에 맞게 조정 됩니다. 쿼리보다 성능이 quality 비디오 뿐만 아니라 덜 복잡 한 콘텐츠 (예: PowerPoint 프레젠테이션)에 적용 하기 위한 작업이 진행 중입니다. 또한이 사전 설정은 적응 스트리밍 사전 설정으로 동일한 해상도 집합을 사용 합니다. Microsoft는 콘텐츠에 기반 하 여 최소 해상도 집합을 선택 하는 방법에 대해 작업 하 고 있습니다. 다음과 같이 인코더에서 입력의 품질이 저하 되었음을 확인할 수 있는 다른 범주의 원본 콘텐츠 (비트 전송률이 낮은 경우 많은 압축 아티팩트)의 결과입니다. 실험적 미리 설정을 사용 하는 경우 인코더는 하나의 출력 계층도 생성 하기로 결정 했습니다. 따라서 대부분의 클라이언트는 상태일 없이 스트림을 재생할 수 있습니다.

![PSNR을 사용 하는 RD 곡선](media/cae-experimental/msrv3.png)

**그림 3: 낮은 품질의 입력에 PSNR을 사용 하는 RD 곡선 (1080p)**

![VMAF를 사용 하는 RD 곡선](media/cae-experimental/msrv4.png)

**그림 4: 낮은 품질의 입력을 위해 VMAF를 사용 하는 RD 곡선 (1080p)**

## <a name="use-the-experimental-preset"></a>실험적 사전 설정 사용

다음과 같이이 사전 설정을 사용 하는 변환을 만들 수 있습니다. [이와 같은](stream-files-tutorial-with-api.md)자습서를 사용 하는 경우 다음과 같이 코드를 업데이트할 수 있습니다.

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new experimental preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncodingExperimental
      }
   }
};
```

> [!NOTE]
> "실험적" 접두사는 기본 알고리즘이 여전히 진화 하 고 있음을 알리기 위해 사용 됩니다. 비트 전송률 ladders를 생성 하는 데 사용 되는 논리에 따라 시간이 지남에 따라 변경 될 수 있으며, 강력한 알고리즘에서 수렴 하 고 다양 한 입력 조건에 적응 하는 목표가 있습니다. 이 사전 설정을 사용 하는 인코딩 작업에는 출력 시간 (분)을 기준으로 요금이 청구 되 고, 대시 및 HLS와 같은 프로토콜의 스트리밍 끝점에서 출력 자산이 배달 될 수 있습니다.

## <a name="next-steps"></a>다음 단계

이제 비디오를 최적화 하는이 새로운 옵션에 대해 알아보았습니다. 이제 사용해 볼 수 있습니다. 이 문서의 끝에 있는 링크를 사용 하 여 의견을 보내 주시기 바랍니다. 또는 <amsved@microsoft.com>에서 직접 도움을 받을 수 있습니다.
