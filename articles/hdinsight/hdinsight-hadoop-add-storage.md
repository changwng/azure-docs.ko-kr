---
title: HDInsight에 추가 Azure Storage 계정 추가
description: 기존 HDInsight 클러스터에 추가 Azure Storage 계정을 추가하는 방법에 대해 알아봅니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 86b9230dbdca82c5599c1839fd64bd3df4725051
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435584"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>HDInsight에 추가 스토리지 계정 추가

스크립트 작업을 사용 하 여 HDInsight에 추가 Azure storage *계정을* 추가 하는 방법을 알아봅니다. 이 문서의 단계는 기존 Linux 기반 HDInsight 클러스터에 저장소 *계정을* 추가 합니다. 이 문서는 저장소 *계정* (기본 클러스터 저장소 계정이 아님)에 적용 되며, [Azure Data Lake Storage Gen1](hdinsight-hadoop-use-data-lake-store.md) 및 [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md)와 같은 추가 저장소는 적용 되지 않습니다.

> [!IMPORTANT]  
> 이 문서의 정보는 클러스터를 만든 후 클러스터에 추가 저장소 계정을 추가 하는 방법에 대 한 것입니다. 클러스터를 만드는 동안 스토리지 계정을 추가하는 방법에 대한 자세한 내용은 [Apache Hadoop, Apache Spark, Apache Kafka 등으로 HDInsight에서 클러스터 설정](hdinsight-hadoop-provision-linux-clusters.md)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

* HDInsight의 Hadoop 클러스터 [Linux에서 HDInsight 시작](./hadoop/apache-hadoop-linux-tutorial-get-started.md)을 참조하세요.
* 저장소 계정 이름 및 키입니다. [저장소 계정 액세스 키 관리](../storage/common/storage-account-keys-manage.md)를 참조 하세요.
* 적절 한 대/ [소문자 클러스터 이름](hdinsight-hadoop-manage-ambari-rest-api.md#identify-correctly-cased-cluster-name)입니다.
* PowerShell을 사용 하는 경우 AZ module이 필요 합니다.  [Azure PowerShell 개요를](https://docs.microsoft.com/powershell/azure/overview)참조 하세요.
* Azure CLI를 설치 하지 않은 경우 [Azure 명령줄 인터페이스 (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)를 참조 하세요.
* Bash 또는 windows 명령 프롬프트를 사용 하는 경우 명령줄 JSON 프로세서인 **jq**도 필요 합니다.  [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)을 참조하세요. Windows 10의 Ubuntu에 있는 bash의 경우 windows [10 용 Windows 하위 시스템 설치 가이드](https://docs.microsoft.com/windows/wsl/install-win10)를 참조 하세요.

## <a name="how-it-works"></a>작동 원리

이 스크립트는 다음 매개 변수를 사용합니다.

* __Azure Storage 계정 이름__: HDInsight 클러스터에 추가할 스토리지 계정의 이름입니다. 스크립트를 실행한 후 HDInsight에서 이 스토리지 계정에 저장된 데이터를 읽고 쓸 수 있습니다.

* __Azure Storage 계정 키__: 스토리지 계정에 대한 액세스 권한을 부여하는 키입니다.

* __-p__ (옵션): 지정 된 경우 키가 암호화 되지 않고 core-site.xml 파일에 일반 텍스트로 저장 됩니다.

처리 중에 스크립트는 다음 작업을 수행합니다.

* 클러스터의 core-site.xml 구성에 스토리지 계정이 이미 있는 경우 스크립트가 종료되고 더 이상의 추가 동작이 수행되지 않습니다.

* 스토리지 계정이 있고 키를 사용하여 액세스할 수 있는지 확인합니다.

* 클러스터 자격 증명을 사용하여 키를 암호화합니다.

* core-site.xml 파일에 스토리지 계정을 추가합니다.

* [Apache Oozie](https://oozie.apache.org/), [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) 및 [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) 서비스를 중지하고 다시 시작합니다. 이러한 서비스를 중지하고 시작하면 서비스에서 새 스토리지 계정을 사용할 수 있습니다.

> [!WARNING]  
> HDInsight 클러스터와 다른 위치에서는 스토리지 계정을 사용할 수 없습니다.

## <a name="the-script"></a>스크립트

__스크립트 위치__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__요구 사항__: __헤드 노드에__스크립트를 적용 해야 합니다. 클러스터에 대 한 Ambari 구성을 직접 업데이트 하므로이 스크립트를 __지속형__으로 표시할 필요가 없습니다.

## <a name="to-use-the-script"></a>스크립트 사용

이 스크립트는 Azure PowerShell, Azure CLI 또는 Azure Portal에서 사용할 수 있습니다.

### <a name="powershell"></a>PowerShell

[AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/submit-azhdinsightscriptaction)사용. `CLUSTERNAME`, `ACCOUNTNAME`및 `ACCOUNTKEY`를 적절 한 값으로 바꿉니다.

```powershell
# Update these parameters
$clusterName = "CLUSTERNAME"
$parameters = "ACCOUNTNAME ACCOUNTKEY"

$scriptActionName = "addStorage"
$scriptActionUri = "https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh"

# Execute script
Submit-AzHDInsightScriptAction `
    -ClusterName $clusterName `
    -Name $scriptActionName `
    -Uri $scriptActionUri `
    -NodeTypes "headnode" `
    -Parameters $parameters
```

### <a name="azure-cli"></a>Azure CLI

[Az hdinsight script-action execute](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-execute)를 사용 합니다.  `CLUSTERNAME`, `RESOURCEGROUP`, `ACCOUNTNAME`및 `ACCOUNTKEY`을 적절 한 값으로 바꿉니다.

```cli
az hdinsight script-action execute ^
    --name CLUSTERNAME ^
    --resource-group RESOURCEGROUP ^
    --roles headnode ^
    --script-action-name addStorage ^
    --script-uri "https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh" ^
    --script-parameters "ACCOUNTNAME ACCOUNTKEY"
```

### <a name="azure-portal"></a>Azure Portal

[실행 중인 클러스터에 스크립트 작업 적용](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)을 참조 하세요.

## <a name="known-issues"></a>알려진 문제

### <a name="storage-firewall"></a>저장소 방화벽

**선택한 네트워크**에 대 한 **방화벽 및 가상 네트워크** 제한 사항을 사용 하 여 저장소 계정을 보호 하도록 선택 하는 경우 HDInsight에서 저장소 계정에 액세스할 수 있도록 예외를 신뢰할 수 있는 **Microsoft 서비스 허용** 으로 설정 해야 합니다.

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Azure Portal 또는 도구에 Storage 계정이 표시되지 않음

Azure Portal에서 HDInsight 클러스터를 볼 때 __속성__ 에서 __저장소 계정__ 항목을 선택 하면이 스크립트 동작을 통해 추가 된 저장소 계정이 표시 되지 않습니다. Azure PowerShell 및 Azure CLI에는 추가 저장소 계정이 표시 되지 않습니다.

이 스토리지 정보는 스크립트에서 클러스터의 core-site.xml 구성만 수정하기 때문에 표시되지 않습니다. 이 정보는 Azure 관리 Api를 사용 하 여 클러스터 정보를 검색할 때 사용 되지 않습니다.

이 스크립트를 사용하여 클러스터에 추가된 스토리지 계정 정보를 보려면 Ambari REST API를 사용합니다. 다음 명령을 사용하여 클러스터에 대한 이 정보를 검색합니다.

### <a name="powershell"></a>PowerShell

`CLUSTERNAME`를 적절 한 대/소문자 클러스터 이름으로 바꿉니다. `ACCOUNTNAME`를 실제 이름으로 바꿉니다. 메시지가 표시 되 면 클러스터 로그인 암호를 입력 합니다.

```powershell
# Update values
$clusterName = "CLUSTERNAME"
$accountName = "ACCOUNTNAME"

$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"

# getting service_config_version
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_service_config_versions/HDFS" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content

$configVersion=$respObj.Clusters.desired_service_config_versions.HDFS.service_config_version

$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=$configVersion" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$accountName.blob.core.windows.net"
```

### <a name="bash"></a>bash

`CLUSTERNAME`를 적절 한 대/소문자 클러스터 이름으로 바꿉니다. `PASSWORD`를 클러스터 관리자 암호로 바꿉니다. `STORAGEACCOUNT`를 실제 저장소 계정 이름으로 바꿉니다.

```bash
export clusterName="CLUSTERNAME"
export password='PASSWORD'
export storageAccount="STORAGEACCOUNT"

export ACCOUNTNAME='"'fs.azure.account.key.$storageAccount.blob.core.windows.net'"'

export configVersion=$(curl --silent -u admin:$password -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName?fields=Clusters/desired_service_config_versions/HDFS" \
| jq ".Clusters.desired_service_config_versions.HDFS[].service_config_version")

curl --silent -u admin:$password -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=$configVersion" \
| jq ".items[].configurations[].properties[$ACCOUNTNAME] | select(. != null)"
```

### <a name="cmd"></a>cmd

`CLUSTERNAME`를 두 스크립트의 적절 한 대/소문자 클러스터 이름으로 바꿉니다. 먼저 아래 명령을 입력 하 여 사용 중인 서비스 구성 버전을 식별 합니다.

```cmd
curl --silent -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_service_config_versions/HDFS" | ^
jq-win64 ".Clusters.desired_service_config_versions.HDFS[].service_config_version"
```

`ACCOUNTNAME`를 실제 저장소 계정 이름으로 바꿉니다. 그런 다음 `4`를 실제 서비스 구성 버전으로 바꾸고 명령을 입력 합니다.

```cmd
curl --silent -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=4" | ^
jq-win64 ".items[].configurations[].properties["""fs.azure.account.key.ACCOUNTNAME.blob.core.windows.net"""] | select(. != null)"
```

---

이 명령에서 반환되는 정보는 다음 텍스트와 비슷합니다.

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

이 텍스트는 스토리지 계정에 액세스하는 데 사용되는 암호화된 키의 예입니다.

### <a name="unable-to-access-storage-after-changing-key"></a>키를 변경한 후 스토리지에 액세스할 수 없음

스토리지 계정의 키를 변경하면 HDInsight에서 더 이상 스토리지 계정에 액세스할 수 없습니다. HDInsight에서는 클러스터에 대한 core-site.xml에서 키의 캐시된 복사본을 사용합니다. 이 캐시된 복사본은 새 키와 일치하도록 업데이트해야 합니다.

스크립트 동작을 다시 실행하면 스크립트에서 스토리지 계정에 대한 항목이 이미 있는지 확인하기 때문에 키를 업데이트하지 __않습니다__. 항목이 이미 있는 경우 아무 것도 변경 되지 않습니다.

이 문제를 해결하려면 스토리지 계정에 대한 기존 항목을 제거해야 합니다. 기존 항목을 제거하려면 다음 단계를 수행합니다.

> [!IMPORTANT]  
> 클러스터에 연결 된 기본 저장소 계정에 대 한 저장소 키 회전은 지원 되지 않습니다.

1. 웹 브라우저에서 HDInsight 클러스터에 대한 Ambari 웹 UI를 엽니다. URI는 `https://CLUSTERNAME.azurehdinsight.net`입니다. `CLUSTERNAME`을 클러스터의 이름으로 바꿉니다.

    메시지가 표시되면 클러스터에 대한 HTTP 로그인 사용자 및 암호를 입력합니다.

2. 페이지 왼쪽의 서비스 목록에서 __HDFS__를 선택합니다. 그런 다음 페이지 중앙의 __구성__ 탭을 선택합니다.

3. __필터 ...__ 필드에서 __fs.azure.account__의 값을 입력합니다. 이렇게 하면 클러스터에 추가된 추가 스토리지 계정에 대한 항목이 반환됩니다. 항목에는 두 가지 유형, __keyprovider__와 __key__가 있습니다. 둘 다 키 이름의 일부로 스토리지 계정의 이름을 포함합니다.

    다음은 __mystorage__라는 스토리지 계정의 예제 항목입니다.

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. 제거 해야 하는 저장소 계정에 대 한 키를 확인 한 후 항목 오른쪽에 있는 빨간색 '-' 아이콘을 사용 하 여 삭제 합니다. 그런 다음 __저장__ 단추를 사용하여 변경 내용을 저장합니다.

5. 변경 사항을 저장한 후 스크립트 동작을 사용하여 스토리지 계정과 새 키 값을 클러스터에 추가합니다.

### <a name="poor-performance"></a>성능 저하

스토리지 계정이 HDInsight 클러스터와 다른 하위 지역에 있는 경우 성능이 저하될 수 있습니다. 다른 하위 지역의 데이터에 액세스하면 하위 지역 Azure 데이터 센터 외부와 공용 인터넷을 통해 네트워크 트래픽이 전송되어 대기 시간이 발생할 수 있습니다.

### <a name="additional-charges"></a>추가 요금

스토리지 계정이 HDInsight 클러스터와 다른 하위 지역에 있는 경우 Azure 청구에서 추가 송신 요금이 발생할 수 있습니다. 데이터가 지역 데이터 센터를 떠날 때 송신 요금이 부과됩니다. 트래픽이 다른 하위 지역의 또 다른 Azure 데이터 센터로 전송되는 경우에도 이 요금이 적용됩니다.

## <a name="next-steps"></a>다음 단계

기존 HDInsight 클러스터에 추가 저장소 계정을 추가 하는 방법을 알아보았습니다. 스크립트 동작에 대한 자세한 내용은 [스크립트 동작을 사용하여 Linux 기반 HDInsight 클러스터 사용자 지정](hdinsight-hadoop-customize-cluster-linux.md)를 참조하세요.
