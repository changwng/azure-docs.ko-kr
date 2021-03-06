---
title: azcopy list | Microsoft Docs
description: 이 문서에서는 azcopy list 명령에 대 한 참조 정보를 제공 합니다.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: f02c1afadf18a7d3170eb178696487464e4a0bd3
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74034088"
---
# <a name="azcopy-list"></a>azcopy list

지정 된 리소스의 엔터티를 나열 합니다.

## <a name="synopsis"></a>개요

현재 릴리스에서는 Blob 컨테이너만 지원 됩니다.

```azcopy
azcopy list [containerURL] [flags]
```

## <a name="related-conceptual-articles"></a>관련 개념 문서

- [AzCopy 시작](storage-use-azcopy-v10.md)
- [AzCopy 및 Blob 저장소를 사용 하 여 데이터 전송](storage-use-azcopy-blobs.md)
- [AzCopy 및 파일 스토리지를 사용하여 데이터 전송](storage-use-azcopy-files.md)
- [AzCopy 구성, 최적화 및 문제 해결](storage-use-azcopy-configure.md)

## <a name="examples"></a>예

```azcopy
azcopy list [containerURL]
```

## <a name="options"></a>옵션

|옵션|설명|
|--|--|
|-h, --help|목록 명령에 대 한 도움말 콘텐츠를 표시 합니다.|
|--컴퓨터에서 읽을 수 있음|파일 크기 (바이트)를 나열 합니다.|
|--단위|1024이 아닌 1000의 주문 단위를 표시 합니다.|
|--실행 중-집계|총 파일 수와 해당 크기를 계산 합니다.|

## <a name="options-inherited-from-parent-commands"></a>부모 명령에서 상속 된 옵션

|옵션|설명|
|---|---|
|--0mbps uint32|전송 률 (메가 비트/초)을 대문자로 처리 합니다. 순간 처리량은 cap와 약간 다를 수 있습니다. 이 옵션을 0으로 설정 하거나 생략 하면 처리량이 생략 되지 않습니다.|
|--출력 형식 문자열|명령의 출력 형식입니다. 텍스트, json 등을 선택할 수 있습니다. 기본값은 "text"입니다.|

## <a name="see-also"></a>참고 항목:

- [azcopy](storage-ref-azcopy.md)
