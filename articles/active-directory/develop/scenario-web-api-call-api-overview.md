---
title: 웹 Api를 호출 하는 web API 빌드-Microsoft identity platform | Microsoft
description: 다운스트림 웹 Api (개요)를 호출 하는 web API를 빌드하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b357def86b77d4bbb294e2253dacfbd129998ec
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965129"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>시나리오: web Api를 호출 하는 Web API

Web Api를 호출 하는 web API를 빌드하는 데 필요한 모든 것을 알아보세요.

## <a name="prerequisites"></a>Előfeltételek

이 시나리오에서 웹 api를 호출 하는 보호 된 web API는 "web API 보호" 시나리오를 기반으로 빌드됩니다. 이 기본적인 시나리오에 대 한 자세한 내용은 [보호 된 웹 API-시나리오](scenario-protected-web-api-overview.md) 를 먼저 참조 하세요.

## <a name="overview"></a>Áttekintés

- 아래 다이어그램에 표시 되지 않는 클라이언트 (웹, 데스크톱, 모바일 또는 단일 페이지 응용 프로그램)는 보호 된 웹 API를 호출 하 고 "Authorization" Http 헤더에 JWT 전달자 토큰을 제공 합니다.
- 보호 된 웹 API는 토큰의 유효성을 검사 하 고 MSAL `AcquireTokenOnBehalfOf` 메서드를 사용 하 여 다른 토큰을 요청 (Azure AD에서) 하 여 사용자 대신 두 번째 웹 API (다운스트림 웹 API)를 호출할 수 있도록 합니다.
- 보호 된 웹 API는이 토큰을 사용 하 여 다운스트림 API를 호출 합니다. 나중에 다른 다운스트림 Api에 대 한 토큰을 요청 하는 `AcquireTokenSilent`를 호출할 수도 있습니다 (동일한 사용자를 대신 하 여). `AcquireTokenSilent` 필요 하면 토큰을 새로 고칩니다.

![Web api를 호출 하는 web API](media/scenarios/web-api.svg)

## <a name="specifics"></a>자세히

API 권한과 관련 된 앱 등록의 일부는 클래식입니다. 응용 프로그램 구성에는 OAuth 2.0를 사용 하 여 다운스트림 API에 대 한 토큰과 JWT 전달자 토큰을 교환 하는 흐름이 포함 됩니다. 이 토큰은 웹 API의 컨트롤러에서 사용할 수 있는 토큰 캐시에 추가 되며, 자동으로 토큰을 획득 하 여 다운스트림 Api를 호출할 수 있습니다.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Alkalmazásregisztráció](scenario-web-api-call-api-app-registration.md)
