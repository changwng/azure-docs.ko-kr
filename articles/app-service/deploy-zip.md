---
title: ZIP 또는 WAR 파일을 사용 하 여 코드 배포
description: ZIP 파일(또는 Java 개발자의 경우 WAR 파일)을 사용하여 Azure App Service에 앱을 배포하는 방법을 알아봅니다.
ms.topic: article
ms.date: 08/12/2019
ms.reviewer: sisirap
ms.custom: seodec18
ms.openlocfilehash: 3569c6a066b09daa0c24975b9de840a844b6ba2c
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74670220"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>ZIP 또는 WAR 파일을 사용하여 Azure App Service에 앱 배포

이 문서는 ZIP 파일 또는 WAR 파일을 사용하여 웹앱을 [Azure App Service](overview.md)에 배포하는 방법을 보여 줍니다. 

이 ZIP 파일 배포는 지속적인 통합 기반 배포를 지원하는 동일한 Kudu 서비스를 사용합니다. Kudu는 ZIP 파일 배포에 대해 다음 기능을 지원합니다. 

- 이전 배포의 남은 파일 삭제.
- 패키지 복원을 포함하는 기본 빌드 프로세스를 켜는 옵션.
- 배포 스크립트 실행을 포함한 [배포 사용자 지정](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings).  
- 배포 로그. 
- 파일 크기 제한은 2048 MB입니다.

자세한 내용은 [Kudu 설명서](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)를 참조하세요.

WAR 파일 배포를 사용하면 [WAR](https://wikipedia.org/wiki/WAR_(file_format)) 파일이 App Service에 배포되고 Java 웹앱이 실행됩니다. [WAR 파일 배포](#deploy-war-file)를 참조하세요.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>전제 조건

이 문서의 단계를 완료하려면 다음을 수행합니다.

* [App Service 앱을 만들거나](/azure/app-service/) 다른 자습서에서 만든 앱을 사용합니다.

## <a name="create-a-project-zip-file"></a>프로젝트 ZIP 파일 만들기

>[!NOTE]
> 파일을 ZIP 파일로 다운로드한 경우 먼저 파일을 추출합니다. 예를 들어 GitHub에서 ZIP 파일을 다운로드한 경우 해당 파일을 그대로 배포할 수 없습니다. GitHub에서는 App Service에서 작동하지 않는 추가적인 중첩된 디렉터리를 추가합니다. 
>

로컬 터미널 창에서 앱 프로젝트의 루트 디렉터리로 이동합니다. 

이 디렉터리는 _index.html_, _index.php_ 및 _app.js_와 같은 웹앱에 대한 입력 파일을 포함해야 합니다. 또한 _project.json_, _composer.json_, _package.json_, _bower.json_ 및 _requirements.txt_와 같은 패키지 관리 파일을 포함할 수 있습니다.

프로젝트의 모든 것에 대한 ZIP 아카이브를 만듭니다. 다음 명령은 터미널의 기본 도구를 사용합니다.

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]
지금은 위의 끝점이 Linux App Services에 대해 작동 하지 않습니다. 대신 FTP 또는 [ZIP 배포 API](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-faq#continuous-integration-and-deployment) 를 사용 하는 것이 좋습니다.

## <a name="deploy-zip-file-with-azure-cli"></a>Azure CLI를 사용하여 ZIP 파일 배포

Azure CLI 버전이 2.0.21 이상인지 확인합니다. 사용 중인 버전을 확인하려면 터미널 창에서 `az --version` 명령을 실행합니다.

[az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) 명령을 사용하여 업로드된 ZIP 파일을 웹앱에 배포합니다.  

다음 예제에서는 업로드한 ZIP 파일을 배포합니다. Azure CLI의 로컬 설치를 사용할 경우 `--src`에 대해 로컬 ZIP 파일 경로를 지정합니다.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

이 명령은 ZIP 파일의 파일과 디렉터리를 기본 App Service 애플리케이션 폴더(`\home\site\wwwroot`)에 배포하고 앱을 다시 시작합니다.

기본적으로 배포 엔진은 ZIP 파일을 있는 그대로 실행할 준비가 된 것으로 가정 하 고 빌드 자동화를 실행 하지 않습니다. [Git 배포](deploy-local-git.md)와 동일한 빌드 자동화를 사용 하도록 설정 하려면 [Cloud Shell](https://shell.azure.com)에서 다음 명령을 실행 하 여 `SCM_DO_BUILD_DURING_DEPLOYMENT` 앱 설정을 설정 합니다.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings SCM_DO_BUILD_DURING_DEPLOYMENT=true
```



자세한 내용은 [Kudu 설명서](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url)를 참조하세요.

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>WAR 파일 배포

WAR 파일을 App Service에 배포 하려면 `https://<app_name>.scm.azurewebsites.net/api/wardeploy`에 POST 요청을 보냅니다. POST 요청은 메시지 본문에 .war 파일을 포함해야 합니다. 앱에 대한 배포 자격 증명은 HTTP 기본 인증을 사용하여 요청으로 제공됩니다.

HTTP BASIC 인증의 경우 App Service 배포 자격 증명이 필요합니다. 배포 자격 증명을 설정하는 방법을 알아보려면 [사용자 수준 자격 증명 설정 및 다시 설정](deploy-configure-credentials.md#userscope)을 참조하세요.

### <a name="with-curl"></a>cURL 사용

다음 예제에서는 cURL 도구를 사용하여 .war 파일을 배포합니다. `<username>`, `<war-file-path>` 및 `<app-name>` 자리 표시자를 바꿉니다. cURL에서 프롬프트가 표시되면 암호를 입력합니다.

```bash
curl -X POST -u <username> --data-binary @"<war-file-path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>PowerShell 사용

다음 예제에서는 [AzWebapp](/powershell/module/az.websites/publish-azwebapp) 를 사용 하 여 war 파일을 업로드 합니다. `<group-name>`, `<app-name>` 및 `<war-file-path>` 자리 표시자를 바꿉니다.

```powershell
Publish-AzWebapp -ResourceGroupName <group-name> -Name <app-name> -ArchivePath <war-file-path>
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>다음 단계

고급 배포 시나리오에 대해서는 [Git를 사용하여 Azure에 배포](deploy-local-git.md)를 시도하세요. Azure로의 Git 기반 배포를 수행하면 버전 제어, 패키지 복원, MSBuild 등을 수행할 수 있습니다.

## <a name="more-resources"></a>추가 리소스

* [Kudu: zip 파일에서 배포](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Azure App Service 배포 자격 증명](deploy-ftp.md)
