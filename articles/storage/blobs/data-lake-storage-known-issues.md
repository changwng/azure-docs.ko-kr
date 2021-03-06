---
title: Azure Data Lake Storage Gen2에서 알려진 문제 | Microsoft Docs
description: Azure Data Lake Storage Gen2에서 제한 사항 및 알려진 문제에 대해 알아보기
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 951d707c898ad0efa1f21480c12f0c733f5218ee
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834938"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2에서 알려진 문제

이 문서에는 아직 지원 되지 않거나 계층적 네임 스페이스 (Azure Data Lake Storage Gen2)를 포함 하는 저장소 계정 에서만 부분적으로 지원 되는 기능 및 도구가 나열 되어 있습니다.

<a id="blob-apis-disabled" />

## <a name="issues-and-limitations-with-using-blob-apis"></a>Blob Api 사용과 관련 된 문제 및 제한 사항

Blob Api 및 Data Lake Storage Gen2 Api는 동일한 데이터에 대해 작동할 수 있습니다.

이 섹션에서는 동일한 데이터에 대해 작동 하는 blob Api 및 Data Lake Storage Gen2 Api를 사용 하는 경우의 문제 및 제한 사항에 대해 설명 합니다.

* Blob Api와 Data Lake Storage Api를 모두 사용 하 여 동일한 파일 인스턴스에 쓸 수는 없습니다. Data Lake Storage Gen2 Api를 사용 하 여 파일에 쓰는 경우 해당 파일의 블록이 [Get Block List](https://docs.microsoft.com/rest/api/storageservices/get-block-list) blob API에 대 한 호출에 표시 되지 않습니다. Data Lake Storage Gen2 Api 또는 Blob Api를 사용 하 여 파일을 덮어쓸 수 있습니다. 이는 파일 속성에 영향을 주지 않습니다.

* 구분 기호를 지정 하지 않고 [Blob 나열](https://docs.microsoft.com/rest/api/storageservices/list-blobs) 작업을 사용 하는 경우 결과에는 디렉터리와 blob이 모두 포함 됩니다. 구분 기호를 사용 하도록 선택 하는 경우 슬래시 (`/`)만 사용 합니다. 유일 하 게 지원 되는 구분 기호입니다.

* [Blob 삭제](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API를 사용 하 여 디렉터리를 삭제 하는 경우 해당 디렉터리는 비어 있는 경우에만 삭제 됩니다. 즉, Blob API delete 디렉터리를 재귀적으로 사용할 수 없습니다.

이러한 Blob REST Api는 지원 되지 않습니다.

* [Blob 배치 (페이지)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [페이지 배치](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [페이지 범위 가져오기](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Blob 증분 복사](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [URL에서 페이지 배치](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Blob 배치 (추가)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [추가 블록](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [URL의 블록 추가](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

계층 네임 스페이스가 있는 계정에서는 관리 되지 않는 VM 디스크가 지원 되지 않습니다. 저장소 계정에서 계층적 네임 스페이스를 사용 하도록 설정 하려면 계층 구조 네임 스페이스 기능이 사용 하도록 설정 되지 않은 저장소 계정에 관리 되지 않는 VM 디스크를 저장 합니다.

<a id="api-scope-data-lake-client-library" />

## <a name="filesystem-support-in-sdks"></a>Sdk에서 파일 시스템 지원

- [.Net](data-lake-storage-directory-file-acl-dotnet.md), [Java](data-lake-storage-directory-file-acl-java.md) 및 [Python](data-lake-storage-directory-file-acl-python.md) 지원은 공개 미리 보기로 제공 됩니다. 다른 Sdk는 현재 지원 되지 않습니다.
- Get 및 set ACL 작업은 현재 재귀적이 아닙니다.

## <a name="filesystem-support-in-powershell-and-azure-cli"></a>PowerShell 및 Azure CLI에서 파일 시스템 지원

- [PowerShell](data-lake-storage-directory-file-acl-powershell.md) 및 [Azure CLI](data-lake-storage-directory-file-acl-cli.md) 지원은 공개 미리 보기로 제공 됩니다.
- Get 및 set ACL 작업은 현재 재귀적이 아닙니다.

## <a name="support-for-other-blob-storage-features"></a>다른 Blob Storage 기능에 대 한 지원

다음 표에는 아직 지원 되지 않거나 계층적 네임 스페이스 (Azure Data Lake Storage Gen2)를 포함 하는 저장소 계정 에서만 부분적으로 지원 되는 기타 모든 기능 및 도구가 나열 되어 있습니다.

| 기능/도구    | 자세한 정보    |
|--------|-----------|
| **계정 장애 조치** |아직 지원 되지 않음|
| **AZCopy** | 버전별 지원 <br><br>최신 버전의 AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json))만 사용 합니다. AzCopy v 8.1과 같은 이전 버전의 AzCopy는 지원 되지 않습니다.|
| **Azure Blob Storage 수명 주기 관리 정책** | 수명 주기 관리 정책이 지원 됩니다 (미리 보기).  [여기](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VURjFLTDRGS0Q4VVZCRFY5MUVaTVJDTkROMi4u)에서 수명 주기 관리 정책 및 보관 액세스 계층의 미리 보기에 등록 합니다.   <br><br>모든 액세스 계층이 지원 됩니다. 보관 액세스 계층은 현재 미리 보기로 제공 됩니다. Blob 스냅숏의 삭제는 아직 지원 되지 않습니다.  현재 수명 주기 관리 정책 및 보관 액세스 계층에 영향을 주는 몇 가지 버그가 있습니다.  |
| **Azure CDN(Content Delivery Network)** | 아직 지원 되지 않음|
| **Azure 검색** |지원 됨 (미리 보기)|
| **Azure Storage Explorer** | 버전별 지원. <br><br>버전 `1.6.0` 이상만 사용 합니다. <br> 현재 특정 시나리오에서 인증 오류가 발생할 수 있는 버전 `1.11.0`에 영향을 주는 저장소 버그가 있습니다. 저장소 버그에 대 한 수정 사항이 롤아웃 되 고 있습니다 .이 문제를 해결 하려면 [무료로 다운로드할](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-relnotes)수 있는 버전 `1.10.x` 사용 하는 것이 좋습니다. `1.10.x`는 저장소 버그의 영향을 받지 않습니다.|
| **Blob 컨테이너 Acl** |아직 지원 되지 않음|
| **Blobfuse** |아직 지원 되지 않음|
| **사용자 지정 도메인** |아직 지원 되지 않음|
| **Azure Portal Storage 탐색기** | 제한적으로 지원됩니다. Acl은 아직 지원 되지 않습니다. |
| **진단 로깅** |진단 로그가 지원 됩니다 (미리 보기). <br><br>Azure Storage 탐색기 1.10. x를 사용 하 여 진단 로그를 볼 수 없습니다. 로그를 보려면 AzCopy 또는 Sdk를 사용 하세요.
| **변경할 수 없는 저장소** |아직 지원 되지 않음 <br><br>변경할 수 없는 저장소는 데이터를 [웜 (한 번 쓰기, 많은 읽기)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) 상태에 저장 하는 기능을 제공 합니다.|
| **개체 수준 계층** |쿨 및 archive 계층이 지원 됩니다. 보관 계층은 미리 보기 상태입니다. 다른 모든 액세스 계층은 아직 지원 되지 않습니다. <br><br> 현재 보관 액세스 계층에 영향을 주는 일부 버그가 있습니다.  [여기](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VURjFLTDRGS0Q4VVZCRFY5MUVaTVJDTkROMi4u)에서 보관 액세스 계층의 미리 보기에 등록 합니다.|
| **정적 웹 사이트** |아직 지원 되지 않음 <br><br>특히 [정적 웹 사이트](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)에 파일을 제공 하는 기능입니다.|
| **타사 응용 프로그램** | 제한된 지원 <br><br>REST Api를 사용 하 여 작동 하는 타사 응용 프로그램은 Data Lake Storage Gen2와 함께 사용 하는 경우 계속 작동 합니다. <br>Blob Api를 호출 하는 응용 프로그램은 작동할 가능성이 높습니다.|
|**일시 삭제** |아직 지원 되지 않음|
| **버전 관리 기능** |아직 지원 되지 않음 <br><br>여기에는 [일시 삭제](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)및 [스냅숏과](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)같은 기타 버전 관리 기능이 포함 됩니다.|


