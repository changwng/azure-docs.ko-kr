---
title: Azure Cosmos DB 오프셋 제한 절
description: OFFSET LIMIT 절을 사용 하 여에서 쿼리할 때 특정 값을 건너뛰거나 가져오는 방법에 대해 알아봅니다 Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: a8df220be211c3c8d8cdeab8a8aebfd35e77ebf8
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732589"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>Azure Cosmos DB 오프셋 제한 절

OFFSET LIMIT 절은 건너뛸 선택적 절입니다 .이 절은 쿼리에서 특정 개수의 값을 사용 합니다. Offset LIMIT 절에는 오프셋 수와 제한 수가 필요 합니다.

OFFSET LIMIT를 ORDER BY 절과 함께 사용 하면 결과 집합은 skip을 수행 하 고 순서가 지정 된 값을 사용 하 여 생성 됩니다. ORDER BY 절을 사용 하지 않으면 값의 결정적 순서가 됩니다.

## <a name="syntax"></a>구문
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>인수

- `<offset_amount>`

   쿼리 결과에서 건너뛸 항목의 정수 수를 지정 합니다.

- `<limit_amount>`
  
   쿼리 결과에 포함 해야 하는 정수 항목 수를 지정 합니다.

## <a name="remarks"></a>설명
  
  Offset LIMIT 절에는 오프셋과 제한 수가 모두 필요 합니다. 선택적 `ORDER BY` 절을 사용 하는 경우에는 정렬 된 값에 대해 skip을 수행 하 여 결과 집합을 생성 합니다. 그렇지 않은 경우 쿼리는 값의 고정 순서를 반환 합니다. 이 절은 이제 단일 파티션 내의 쿼리와 파티션 간 쿼리를 지원 합니다.

## <a name="examples"></a>예시

예를 들어 다음은 첫 번째 값을 건너뛰고 두 번째 값 (상주 도시 이름 순)을 반환 하는 쿼리입니다.

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

결과는 다음과 같습니다.

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

다음은 첫 번째 값을 건너뛰고 순서를 지정 하지 않고 두 번째 값을 반환 하는 쿼리입니다.

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

결과는 다음과 같습니다.

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>다음 단계

- [시작](sql-query-getting-started.md)
- [SELECT 절](sql-query-select.md)
- [ORDER BY 절](sql-query-order-by.md)
