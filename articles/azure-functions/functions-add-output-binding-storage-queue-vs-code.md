---
title: Visual Studio Code를 사용하여 Azure Storage에 함수 연결
description: Visual Studio Code를 사용하여 Azure Storage 큐에 함수를 연결하도록 출력 바인딩을 추가하는 방법을 알아봅니다.
ms.date: 06/25/2019
ms.topic: quickstart
ms.openlocfilehash: baddb6f02fe3d9c66e3c52d826ffe70c151d313e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227448"
---
# <a name="connect-functions-to-azure-storage-using-visual-studio-code"></a>Visual Studio Code를 사용하여 Azure Storage에 함수 연결

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

이 문서에서는 Visual Studio Code를 사용하여 [이전의 빠른 시작 문서](functions-create-first-function-vs-code.md)에서 만든 함수를 Azure Storage에 연결하는 방법을 보여 줍니다. 이 함수에 추가되는 출력 바인딩은 HTTP 요청의 데이터를 Azure Queue storage 큐의 메시지에 씁니다. 

대부분의 바인딩은 Functions에서 바인딩된 서비스에 액세스할 때 사용할 저장된 연결 문자열이 필요합니다. 이 부분을 간편하게 해결하려면 함수 앱으로 만든 Storage 계정을 사용합니다. 이 계정에 대한 연결은 이미 `AzureWebJobsStorage` 앱 설정에 저장되어 있습니다.  

## <a name="prerequisites"></a>필수 조건

이 문서를 시작하기 전에 다음 요구 사항을 충족해야 합니다.

* [Visual Studio Code용 Azure Storage 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)을 설치합니다.
* [Azure Storage Explorer](https://storageexplorer.com/)를 설치합니다. Storage Explorer는 출력 바인딩에서 생성하는 큐 메시지를 검토하는 데 사용하는 도구입니다. Storage Explorer는 macOS, Windows, Linux 기반 운영 체제에서 지원됩니다.
* [.NET Core CLI 도구](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)(C# 프로젝트에만 해당)를 설치합니다.
* [Visual Studio Code 빠른 시작의 1부](functions-create-first-function-vs-code.md)의 단계를 완료합니다. 

이 문서는 Visual Studio Code에서 Azure 구독에 이미 로그인했다고 가정합니다. 명령 팔레트에서 `Azure: Sign In`을 실행하여 로그인할 수 있습니다. 

## <a name="download-the-function-app-settings"></a>함수 앱 설정 다운로드

[이전의 빠른 시작 문서](functions-create-first-function-vs-code.md)에서는 Azure에서 필요한 스토리지 계정과 함께 함수 앱을 만들었습니다. 이 계정의 연결 문자열은 Azure의 앱 설정에 안전하게 저장됩니다. 이 문서에서는 같은 계정의 Storage 큐에 메시지를 작성합니다. 함수를 로컬로 실행할 때 Storage 계정에 연결하려면 앱 설정을 local.settings.json 파일에 다운로드해야 합니다. 

1. F1 키를 눌러 명령 팔레트를 연 다음, `Azure Functions: Download Remote Settings....` 명령을 검색하고 실행합니다. 

1. 이전 문서에서 만든 함수 앱을 선택합니다. **모두 예**를 선택하여 기존 로컬 설정을 덮어씁니다. 

    > [!IMPORTANT]  
    > local.settings.json 파일은 비밀을 포함하고 있으므로 절대 게시되지 않으며, 소스 제어에서 제외됩니다.

1. 스토리지 계정 연결 문자열 값에 대한 키인 `AzureWebJobsStorage` 값을 복사합니다. 이 연결을 사용하여 출력 바인딩이 예상대로 작동하는지 확인합니다.

## <a name="register-binding-extensions"></a>바인딩 확장 등록

Queue storage 출력 바인딩을 사용하므로 프로젝트를 실행하기 전에 스토리지 바인딩 확장을 설치해야 합니다. 

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

HTTP 및 타이머 트리거를 제외하고 바인딩은 확장 패키지로 구현됩니다. 터미널 창에서 다음 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 명령을 실행하여 프로젝트에 스토리지 확장 패키지를 추가합니다.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```
---
이제 프로젝트에 스토리지 출력 바인딩을 추가할 수 있습니다.

## <a name="add-an-output-binding"></a>출력 바인딩 추가

Functions에서 각 바인딩 형식의 `direction`, `type` 및 고유한 `name`을 function.json 파일에 정의해야 합니다. 이러한 특성을 정의하는 방법은 함수 앱의 언어에 따라 달라집니다.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

---

## <a name="add-code-that-uses-the-output-binding"></a>출력 바인딩을 사용하는 코드 추가

바인딩이 정의되면 바인딩의 `name`을 사용하여 함수 시그니처의 특성으로 액세스할 수 있습니다. 출력 바인딩을 사용하면 인증을 받거나 큐 참조를 가져오거나 데이터를 쓸 때 Azure Storage SDK 코드를 사용할 필요가 없습니다. Functions 런타임 및 큐 출력 바인딩이 이러한 작업을 알아서 처리합니다.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]

---

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

**outqueue**라는 새 큐는 출력 바인딩이 처음 사용될 때 함수 런타임에 의해 스토리지 계정에 만들어집니다. Storage Explorer를 사용하여 새 메시지와 함께 큐가 만들어졌는지 확인합니다.

### <a name="connect-storage-explorer-to-your-account"></a>Storage Explorer를 계정에 연결

Azure Storage Explorer를 이미 설치했고 Azure 계정에 연결한 경우 이 섹션을 건너뜁니다.

1. [Azure Storage Explorer] 도구를 실행하여 왼쪽에 있는 연결 아이콘을 선택한 다음, **계정 추가**를 선택합니다.

    ![Microsoft Azure Storage Explorer에 Azure 계정 추가](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. **연결** 대화 상자에서 **Azure 계정 추가**를 선택한 다음, **Azure 환경**과 **로그인...** 을 차례대로 선택합니다. 

    ![Azure 계정에 로그인](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

계정에 성공적으로 로그인하면 계정에 연결된 모든 Azure 구독이 표시됩니다.

### <a name="examine-the-output-queue"></a>출력 큐 검토

1. Visual Studio Code에서 F1 키를 눌러 명령 팔레트를 연 다음, `Azure Storage: Open in Storage Explorer` 명령을 검색하여 실행하고 스토리지 계정 이름을 선택합니다. 스토리지 계정이 Azure Storage Explorer에서 열립니다.  

1. **큐** 노드를 확장한 다음 이름이 **outqueue**인 큐를 선택합니다. 

   이 큐에는 HTTP 트리거 함수를 실행했을 때 만들어진 큐 출력 바인딩 메시지가 포함되어 있습니다. 기본 `name` 값 Azure로 함수를 호출했다면 큐 메시지는 ‘함수에 전달된 이름:   Azure’입니다.

    ![Azure Storage Explorer에 표시되는 큐 메시지](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. 함수를 다시 실행한 다음, 다른 요청을 보내면 큐에 새 메시지가 표시되는 것을 알 수 있습니다.  

이제 업데이트된 함수 앱을 Azure에 다시 게시할 차례입니다.

## <a name="redeploy-and-verify-the-updated-app"></a>업데이트된 앱 재배포 및 확인

1. Visual Studio Code에서 F1 키를 눌러 명령 팔레트를 엽니다. 명령 팔레트에서 `Azure Functions: Deploy to function app...`을 검색하여 선택합니다.

1. 처음 문서에서 만든 함수 앱을 선택합니다. 동일한 앱에 프로젝트를 다시 배포하므로 **배포**를 선택하여 파일 덮어쓰기에 대한 경고를 해제합니다.

1. 배포가 완료되면 cURL 또는 브라우저를 다시 사용하여 재배포된 함수를 테스트할 수 있습니다. 앞에서와 같이 쿼리 문자열을 `&name=<yourname>` URL에 추가합니다. 다음 예제를 참조하세요.

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. 다시 [스토리지 큐의 메시지를 보고](#examine-the-output-queue) 출력 바인딩이 큐에 새 메시지를 다시 생성하는지 확인합니다.

## <a name="clean-up-resources"></a>리소스 정리

Azure에서 *리소스*란 함수 앱, 함수, 스토리지 계정 등을 의미합니다. 리소스는 *리소스 그룹*으로 그룹화되며 그룹을 삭제하면 그룹의 모든 항목을 삭제할 수 있습니다.

이러한 빠른 시작을 완료하기 위해 리소스를 만들었습니다. [계정 상태](https://azure.microsoft.com/account/) 및 [서비스 가격 책정](https://azure.microsoft.com/pricing/)에 따라 리소스에 대해 요금이 청구될 수 있습니다. 리소스가 더 이상 필요하지 않게 되면 다음과 같이 삭제합니다.

1. Visual Studio Code에서 F1 키를 눌러 명령 팔레트를 엽니다. 명령 팔레트에서 `Azure Functions: Open in portal`을 검색하여 선택합니다.

1. 함수 앱을 선택한 다음, Enter 키를 누릅니다. 함수 앱 페이지는 [Azure Portal](https://portal.azure.com)에서 열립니다.

1. **개요** 탭에서 **리소스 그룹** 아래에 있는 명명된 링크를 선택합니다.

    ![함수 앱 페이지에서 삭제할 리소스 그룹을 선택합니다.](./media/functions-add-output-binding-storage-queue-vs-code/functions-app-delete-resource-group.png)

1. **리소스 그룹** 페이지에서 포함된 리소스 목록을 검토하고 삭제하려는 항목인지 확인합니다.
 
1. **리소스 그룹 삭제**를 선택하고 지시를 따릅니다.

   삭제는 몇 분 정도 소요됩니다. 완료되면 알림이 잠시 표시됩니다. 페이지 위쪽의 종 모양 아이콘을 선택해도 알림을 확인할 수 있습니다.

## <a name="next-steps"></a>다음 단계

Storage 큐에 데이터를 쓰도록 HTTP 트리거 함수를 업데이트했습니다. Functions 개발에 대한 자세한 내용은 [Visual Studio Code를 사용하여 Azure Functions 개발](functions-develop-vs-code.md)을 참조하세요.

다음으로, 함수 앱에 Application Insights 모니터링을 사용하도록 설정해야 합니다.

> [!div class="nextstepaction"]
> [Application Insights 통합 사용](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/
