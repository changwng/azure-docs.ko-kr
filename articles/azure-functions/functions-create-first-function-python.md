---
title: Azure에서 HTTP 트리거 Python 함수 만들기
description: Azure에서 Azure Functions Core Tools 및 Azure CLI를 사용하여 첫 번째 Azure 함수를 만드는 방법을 알아봅니다.
ms.date: 11/07/2019
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 18ae1ed000ffe61ce1ea9ff5c18aae98a0ffae65
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227185"
---
# <a name="quickstart-create-an-http-triggered-python-function-in-azure"></a>빠른 시작: Azure에서 HTTP 트리거 Python 함수 만들기

이 문서에서는 명령줄 도구를 사용하여 Azure Functions에서 실행되는 Python 프로젝트를 만드는 방법을 보여줍니다. HTTP 요청에 의해 트리거되는 함수도 만듭니다. 로컬로 실행한 후에 Azure에서 [서버리스 함수](functions-scale.md#consumption-plan)로 실행되도록 프로젝트를 게시합니다. 

이 문서는 Azure Functions에 대한 두 편의 Python 빠른 시작 중 첫 번째입니다. 이 빠른 시작을 완료하면 함수에 [Azure Storage 큐 출력 바인딩](functions-add-output-binding-storage-queue-python.md)을 추가할 수 있습니다.

또한 이 문서의 [Visual Studio Code 기반 버전](/azure/python/tutorial-vs-code-serverless-python-01)도 있습니다.

## <a name="prerequisites"></a>필수 조건

시작하기 전에 다음을 수행해야 합니다.

+ [Python 3.7.4](https://www.python.org/downloads/)를 설치합니다. 이 버전의 Python은 Functions로 확인됩니다. Python 3.8 이상 버전은 아직 지원되지 않습니다.

+ [Azure Functions Core Tools](./functions-run-local.md#v2) 버전 2.7.1846 이상을 설치합니다.

+ [Azure CLI](/cli/azure/install-azure-cli) 버전 2.0.76 이상을 설치합니다.

+ 활성 Azure 구독

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>가상 환경 만들기 및 활성화

Python 함수를 로컬에서 개발하려면 Python 3.7 환경을 사용해야 합니다. 다음 명령을 실행하여 `.venv`라는 가상 환경을 만들고 활성화합니다.

> [!NOTE]
> Python이 Linux 배포에 venv를 설치하지 않은 경우, 다음 명령을 사용하여 설치할 수 있습니다.
> ```command
> sudo apt-get install python3-venv

### <a name="bash"></a>Bash:

```bash
python -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell 또는 Windows 명령 프롬프트:

```powershell
py -m venv .venv
.venv\scripts\activate
```

가상 환경을 활성화했으면 여기에서 나머지 명령을 실행합니다. 가상 환경에서 벗어나려면 `deactivate`를 실행합니다.

## <a name="create-a-local-functions-project"></a>로컬 함수 프로젝트 만들기

함수 프로젝트는 동일한 로컬 및 호스팅 구성을 공유하는 함수를 여러 개 포함할 수 있습니다.

가상 환경에서 다음 명령을 실행합니다.

```console
func init MyFunctionProj --python
cd MyFunctionProj
```

`func init` 명령은 _MyFunctionProj_ 폴더를 만듭니다. 이 폴더의 Python 프로젝트에는 아직 함수가 없습니다. 다음번에 추가합니다.

## <a name="create-a-function"></a>함수 만들기

프로젝트에 함수를 추가하려면 다음 명령을 실행합니다.

```console
func new --name HttpTrigger --template "HTTP trigger"
```

이 명령은 다음 파일을 포함하는 _HttpTrigger_라는 하위 폴더를 만듭니다.

* *function.json*: 함수, 트리거 및 기타 바인딩을 정의하는 구성 파일입니다. 이 파일에서 `scriptFile` 값은 함수가 포함된 파일을 가리키고 `bindings` 배열은 호출 트리거와 바인딩을 정의합니다.

    각 바인딩에는 명령, 형식 및 고유한 이름이 필요합니다. HTTP 트리거의 입력 바인딩은 [`httpTrigger`](functions-bindings-http-webhook.md#trigger) 형식이고, 출력 바인딩은 [`http`](functions-bindings-http-webhook.md#output) 형식입니다.

* *\_\_init\_\_.py*: HTTP 트리거 함수인 스크립트 파일입니다. 이 스크립트에는 기본 `main()`이 있습니다. 트리거의 HTTP 데이터는 `binding parameter`라는 `req`를 사용하여 함수로 전달됩니다. function.json에 정의되는 `req`는 [azure.functions.HttpRequest 클래스](/python/api/azure-functions/azure.functions.httprequest)의 인스턴스입니다. 

    *function.json*에 `$return`으로 정의되는 반환 개체는 [azure.functions.HttpResponse 클래스](/python/api/azure-functions/azure.functions.httpresponse)의 인스턴스입니다. 자세한 내용은 [Azure Functions HTTP 트리거 및 바인딩](functions-bindings-http-webhook.md)을 참조하세요.

이제 로컬 컴퓨터에서 새 함수를 실행할 수 있습니다.

## <a name="run-the-function-locally"></a>로컬에서 함수 실행

이 명령은 Azure Functions 런타임(func.exe)을 사용하여 함수 앱을 시작합니다.

```console
func host start
```

출력에 기록된 다음 정보가 표시됩니다.

```output
Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger    
```

출력에서 `HttpTrigger` 함수의 URL을 복사하고 브라우저의 주소 표시줄에 붙여 넣습니다. 이 URL에 쿼리 문자열 `?name=<yourname>`을 추가하고 요청을 실행합니다. 다음 스크린샷은 로컬 함수가 브라우저로 반환하는 GET 요청에 대한 응답을 보여줍니다.

![브라우저에서 로컬로 확인](./media/functions-create-first-function-python/function-test-local-browser.png)

Ctrl+C를 사용하여 함수 앱 실행을 종료합니다.

함수를 로컬로 실행했으므로 Azure에 함수 코드를 배포할 수 있습니다.  
앱을 배포하기 전에 몇 가지 Azure 리소스를 만들어야 합니다.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Azure에서 함수 앱 만들기

함수 앱은 함수 코드 실행을 위한 환경을 제공합니다. 이를 통해 함수를 논리 단위로 그룹화하여 더욱 쉽게 리소스를 관리, 배포 및 공유할 수 있습니다. 

다음 명령을 실행합니다. `<APP_NAME>`을 고유한 함수 앱 이름으로 바꿉니다. `<STORAGE_NAME>`을 스토리지 계정 이름으로 바꿉니다. `<APP_NAME>`은 함수 앱의 기본 DNS 도메인이기도 합니다. 이 이름은 Azure의 모든 앱에서 고유해야 합니다.

> [!NOTE]
> Linux 및 Windows 앱을 동일한 리소스 그룹에 호스트할 수 없습니다. Windows 함수 앱 또는 웹앱이 포함된 `myResourceGroup`이라는 기존 리소스 그룹이 있는 경우 다른 리소스 그룹을 사용해야 합니다.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python --runtime-version 3.7 \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

이전 명령은 Python 3.7.4를 실행하는 함수 앱을 만듭니다. 또한 이 명령은 동일한 리소스 그룹에서 연결된 Azure Application Insights 인스턴스를 프로비저닝합니다. 이 인스턴스를 사용하여 함수 앱을 모니터링하고 로그를 볼 수 있습니다. 

이제 로컬 함수 프로젝트를 Azure의 함수 앱에 게시할 준비가 되었습니다.

## <a name="deploy-the-function-app-project-to-azure"></a>함수 앱 프로젝트를 Azure에 배포

Azure에서 함수 앱을 만든 후에는 [func azure functionapp publish](functions-run-local.md#project-file-deployment) Core Tools 명령을 사용하여 Azure에 프로젝트 코드를 배포할 수 있습니다. 이 예제에서는 `<APP_NAME>`을 앱 이름으로 바꿉니다.

```console
func azure functionapp publish <APP_NAME> --build remote
```

`--build remote` 옵션은 권장되는 대로, 배포 패키지의 파일에서 원격으로 Python 프로젝트를 Azure에 빌드합니다. 

다음 메시지와 유사한 출력이 표시됩니다. 여기에는 보기 편하도록 잘려 있습니다.

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

`HttpTrigger`에 대한 `Invoke url` 값을 복사하여 Azure에서 함수를 확인하는 데 사용할 수 있습니다. URL에는 함수 키인 `code` 쿼리 문자열 값이 포함되어 있기 때문에 다른 사용자가 Azure에서 HTTP 트리거 엔드포인트를 호출하기 어렵습니다.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> 게시된 Python 앱의 거의 실시간 로그를 보려면 [Application Insights 라이브 메트릭 스트림](functions-monitoring.md#streaming-logs)을 사용합니다.

## <a name="next-steps"></a>다음 단계

HTTP 트리거 함수를 사용하여 Python 함수 프로젝트를 만들고, 로컬 머신에서 실행하고, Azure에 배포했습니다. 이제 아래 방법으로 함수를 확장하겠습니다.

> [!div class="nextstepaction"]
> [Azure Storage 큐 출력 바인딩 추가](functions-add-output-binding-storage-queue-python.md)
