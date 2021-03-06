---
title: Azure PowerShell 스크립트 - Azure Cosmos DB SQL(Core) API 데이터베이스 및 컨테이너 만들기
description: Azure PowerShell 스크립트 - Azure Cosmos DB SQL(Core) API 데이터베이스 및 컨테이너 만들기
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 09/20/2019
ms.author: mjbrown
ms.openlocfilehash: eee1e31808412dc5e4308dee92f3685507e771f3
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2019
ms.locfileid: "71178799"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-core-api"></a>Azure Cosmos DB - SQL(Core) API에 대한 데이터베이스 및 컨테이너 만들기

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>샘플 스크립트

이 스크립트는 세션 수준 일관성, 공유 처리량이 있는 데이터베이스 및 파티션 키가 있는 컨테이너, 사용자 지정 인덱싱 정책, 고유 키 정책, TTL, 전용 처리량이 포함된 두 개의 지역에서 SQL(Core) API에 대한 Cosmos 계정을 만들고 마지막 기록기는 `multipleWriteLocations=true`일 때 사용할 사용자 지정 충돌 해결 경로를 통해 충돌 해결 정책을 얻습니다.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-create.ps1 "Create an account, database, and container for SQL (Core) API")]

## <a name="clean-up-deployment"></a>배포 정리

스크립트 샘플을 실행한 후에 다음 명령을 사용하여 리소스 그룹 및 관련된 모든 리소스를 제거할 수 있습니다.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
|**Azure 리소스**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | 리소스를 만듭니다. |
|**Azure 리소스 그룹**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |
|||

## <a name="next-steps"></a>다음 단계

Azure PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](https://docs.microsoft.com/powershell/)를 참조하세요.

추가 Azure Cosmos DB PowerShell 스크립트 샘플은 [Azure Cosmos DB PowerShell 스크립트](../../../powershell-samples.md)에 있습니다.
