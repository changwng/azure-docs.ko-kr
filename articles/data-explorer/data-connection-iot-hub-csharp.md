---
title: 을 사용 하 여 Azure 데이터 탐색기에 대 한 IoT Hub 데이터 연결 만들기C#
description: 이 문서에서는를 사용 C#하 여 Azure 데이터 탐색기에 대 한 IoT Hub 데이터 연결을 만드는 방법에 대해 알아봅니다.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 0cac03e50bf46910f8430b745803107b60905769
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74667378"
---
# <a name="create-an-iot-hub-data-connection-for-azure-data-explorer-by-using-c-preview"></a>(미리 보기)를 사용 하 C# 여 Azure 데이터 탐색기에 대 한 IoT Hub 데이터 연결 만들기

> [!div class="op_single_selector"]
> * [Portal](ingest-data-iot-hub.md)
> * [C#](data-connection-iot-hub-csharp.md)
> * [Python](data-connection-iot-hub-python.md)
> * [Azure Resource Manager 템플릿](data-connection-iot-hub-resource-manager.md)

Azure 데이터 탐색기는 로그 및 원격 분석 데이터에 사용 가능한 빠르고 확장성이 우수한 데이터 탐색 서비스입니다. Azure 데이터 탐색기는 blob 컨테이너에 기록 된 Event Hubs, IoT Hub 및 blob에서 수집 (데이터 로드)을 제공 합니다. 이 문서에서는를 사용 C#하 여 Azure 데이터 탐색기에 대 한 IoT Hub 데이터 연결을 만듭니다.

## <a name="prerequisites"></a>전제 조건

* Visual Studio 2019가 설치되지 않은 경우 **체험판** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)을 다운로드하고 사용할 수 있습니다. Visual Studio를 설정하는 동안 **Azure 개발**을 사용할 수 있는지 확인합니다.
* Azure 구독이 아직 없는 경우 시작하기 전에 [Azure 체험 계정](https://azure.microsoft.com/free/)을 만듭니다.
* [클러스터 및 데이터베이스](create-cluster-database-csharp.md) 만들기
* [테이블 및 열 매핑](net-standard-ingest-data.md#create-a-table-on-your-test-cluster) 만들기
* [데이터베이스 및 테이블 정책](database-table-policies-csharp.md) 설정 (선택 사항)
* [공유 액세스 정책이 구성 된 IoT Hub](ingest-data-iot-hub.md#create-an-iot-hub)를 만듭니다.

[!INCLUDE [data-explorer-data-connection-install-nuget-csharp](../../includes/data-explorer-data-connection-install-nuget-csharp.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-iot-hub-data-connection"></a>IoT Hub 데이터 연결 추가 

다음 예제에서는 프로그래밍 방식으로 IoT Hub 데이터 연결을 추가 하는 방법을 보여 줍니다. Azure Portal를 사용 하 여 Iot Hub 데이터 연결을 추가 [하려면 IoT Hub Azure 데이터 탐색기 테이블 연결을](ingest-data-iot-hub.md#connect-azure-data-explorer-table-to-iot-hub) 참조 하세요.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
var dataConnectionName = "myeventhubconnect";
//The IoT hub that is created as part of the Prerequisites
var iotHubResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Devices/IotHubs/xxxxxx";
var sharedAccessPolicyName = "iothubforread";
var consumerGroup = "$Default";
var location = "Central US";
//The table and column mapping are created as part of the Prerequisites
var tableName = "StormEvents";
var mappingRuleName = "StormEvents_CSV_Mapping";
var dataFormat = DataFormat.CSV;

await kustoManagementClient.DataConnections.CreateOrUpdate(resourceGroupName, clusterName, databaseName, dataConnectionName,
            new IotHubDataConnection(iotHubResourceId, consumerGroup, sharedAccessPolicyName, tableName: tableName, location: location, mappingRuleName: mappingRuleName, dataFormat: dataFormat));
```

|**설정** | **제안 값** | **필드 설명**|
|---|---|---|
| tenantId | *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx-xxxxx-xxxxxxxxx* | 테넌트 ID 디렉터리 ID 라고도 합니다.|
| subscriptionId | *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx-xxxxx-xxxxxxxxx* | 리소스를 만드는 데 사용 하는 구독 ID입니다.|
| clientId | *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx-xxxxx-xxxxxxxxx* | 테 넌 트의 리소스에 액세스할 수 있는 응용 프로그램의 클라이언트 ID입니다.|
| clientSecret | *xxxxxxxxxxxxxx* | 테 넌 트의 리소스에 액세스할 수 있는 응용 프로그램의 클라이언트 암호입니다. |
| resourceGroupName | *testrg* | 클러스터를 포함 하는 리소스 그룹의 이름입니다.|
| clusterName | *mykustocluster* | 클러스터의 이름입니다.|
| databaseName | *mykustodatabase* | 클러스터에 있는 대상 데이터베이스의 이름입니다.|
| dataConnectionName | *myeventhubconnect* | 원하는 데이터 연결 이름입니다.|
| tableName | *StormEvents* | 대상 데이터베이스에 있는 대상 테이블의 이름입니다.|
| mappingRuleName | *StormEvents_CSV_Mapping* | 대상 테이블과 관련 된 열 매핑의 이름입니다.|
| dataFormat | *csv* | 메시지의 데이터 형식입니다.|
| iotHubResourceId | *리소스 ID* | 수집할 데이터를 보유 하는 IoT hub의 리소스 ID입니다. |
| sharedAccessPolicyName | *iothubforread* | IoT Hub에 연결할 장치 및 서비스에 대 한 권한을 정의 하는 공유 액세스 정책의 이름입니다. |
| consumerGroup | *$Default* | 이벤트 허브의 소비자 그룹입니다.|
| location | *미국 중부* | 데이터 연결 리소스의 위치입니다.|

[!INCLUDE [data-explorer-data-connection-clean-resources-csharp](../../includes/data-explorer-data-connection-clean-resources-csharp.md)]
