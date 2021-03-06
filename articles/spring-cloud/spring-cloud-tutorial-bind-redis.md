---
title: 자습서 - Azure Cache for Redis를 Azure Spring Cloud 애플리케이션에 바인딩하는 방법
description: 이 자습서에서는 Azure Cache for Redis를 Azure Spring Cloud 애플리케이션에 바인딩하는 방법을 보여줍니다.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: 1653db3619fd569238872ca1fcfd6d0c439e84c9
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74708775"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-cache-for-redis"></a>자습서: Azure 서비스를 Azure Spring Cloud애플리케이션에 바인딩합니다. Azure Cache for Redis

Azure Spring Cloud를 사용하면 Spring Boot 애플리케이션을 수동으로 구성하는 대신 선택한 Azure Services를 바인딩할 수 있습니다. 이 문서에서는 Azure Cache for Redis에 애플리케이션을 바인딩하는 방법을 보여 줍니다.

## <a name="prerequisites"></a>필수 조건

* 배포된 Azure Spring Cloud 인스턴스
* Azure Cache for Redis 서비스 인스턴스
* Azure CLI용 Azure Spring Cloud 확장

배포된 Azure Spring Cloud 인스턴스가 없는 경우 이 [빠른 시작](spring-cloud-quickstart-launch-app-portal.md)의 단계에 따라 첫 번째 Spring Cloud 앱을 배포합니다.

## <a name="bind-azure-cache-for-redis"></a>Azure Cache for Redis 바인딩

1. 프로젝트의 `pom.xml`에 다음 종속성 추가

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. `application.properties` 파일에서 `spring.redis.*` 속성 제거(있는 경우)

1. `az spring-cloud app update`를 사용하여 현재 배포를 업데이트하거나 `az spring-cloud app deployment create`를 사용하여 새 배포를 만듭니다.

1. Azure Portal에서 Azure Spring Cloud 서비스 페이지로 이동합니다. **Application Dashboard**를 찾아 Azure Cache for Redis에 바인딩할 애플리케이션을 선택합니다.  이는 이전 단계에서 업데이트하거나 배포한 동일한 애플리케이션입니다. 그런 다음, `Service binding`을 선택하고 `Create service binding` 단추를 선택합니다. 양식을 작성하여 **Binding 유형** `Azure Cache for Redis`, Redis 서버 및 기본 키 옵션을 선택합니다. 

1. 현재 앱을 다시 시작하면 이 바인딩이 작동합니다.

1. 서비스 바인딩이 올바른지 확인하려면 바인딩 이름을 선택하고 해당 세부 정보를 확인합니다. `property` 필드는 다음과 같이 표시되어야 합니다.
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Spring Cloud 애플리케이션을 Azure Redis 캐시에 바인딩하는 방법에 대해 알아보았습니다.  애플리케이션에 서비스를 바인딩하는 방법에 대한 자세한 내용은 MySQL DB에 애플리케이션을 바인딩하는 자습서를 계속 진행하세요.

> [!div class="nextstepaction"]
> [Azure MySql 서비스를 Azure Spring Cloud 서비스에 바인딩하는 방법에 대해 알아봅니다](spring-cloud-tutorial-bind-mysql.md).