---
title: Azure Storage services의 데이터에 액세스
titleSuffix: Azure Machine Learning
description: 을 사용 하 여 학습 하는 동안 데이터 저장소를 사용 하 여 Azure storage 서비스에 안전 하 게 연결 하는 방법을 알아봅니다 Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: ylxiong
author: YLXiong1125
ms.reviewer: nibaccam
ms.date: 12/10/2019
ms.custom: seodec18
ms.openlocfilehash: ac6ef6341013ca13d5a9f27be8897365c1c2155d
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75540946"
---
# <a name="access-data-in-azure-storage-services"></a>Azure storage 서비스의 데이터에 액세스
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

이 문서에서는 Azure Machine Learning datastores를 통해 Azure Storage 서비스에서 데이터에 쉽게 액세스 하는 방법에 대해 알아봅니다. 데이터 저장소는 구독 ID 및 토큰 권한 부여와 같은 연결 정보를 저장 하는 데 사용 됩니다. Datastores를 사용 하는 경우 스크립트에 연결 정보를 하드 코딩 하지 않고도 저장소에 액세스할 수 있습니다. 

[이러한 Azure Storage 솔루션](#matrix)에서 데이터 저장소를 만들 수 있습니다. 지원 되지 않는 저장소 솔루션의 경우 machine learning 실험 중에 데이터 송신 비용을 절약 하려면 지원 되는 Azure Storage 솔루션으로 [데이터를 이동](#move) 하는 것이 좋습니다. 

## <a name="prerequisites"></a>필수 조건
필요한 사항:
- Azure 구독 Azure 구독이 없는 경우 시작하기 전에 체험 계정을 만듭니다. [Azure Machine Learning의 무료 또는 유료 버전](https://aka.ms/AMLFree)을 사용해 보세요.

- Azure [blob 컨테이너](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) 또는 [azure 파일 공유](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)를 사용 하는 azure storage 계정

- [Python 용 AZURE MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)또는 [Azure Machine Learning studio](https://ml.azure.com/)에 대 한 액세스입니다.

- Azure Machine Learning 작업 영역
  
  [Azure Machine Learning 작업 영역을 만들거나](how-to-manage-workspace.md) Python SDK를 통해 기존 작업 영역을 사용 합니다.

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

<a name="access"></a>

## <a name="create-and-register-datastores"></a>데이터 저장소 만들기 및 등록

Azure Storage 솔루션을 데이터 저장소로 등록 하면 해당 데이터 저장소를 특정 작업 영역에 자동으로 만듭니다. Python SDK 또는 Azure Machine Learning studio를 사용 하 여 데이터 저장소를 만들고 작업 영역에 등록할 수 있습니다.

### <a name="python-sdk"></a>Python SDK

모든 register 메서드는 [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) 클래스에 있으며 폼 `register_azure_*`합니다.

[Azure Portal](https://portal.azure.com)를 사용 하 여 `register()` 메서드를 채우는 데 필요한 정보를 찾을 수 있습니다.

1. 왼쪽 창에서 **저장소 계정** 을 선택 하 고 등록 하려는 저장소 계정을 선택 합니다. 
2. 계정 이름, 컨테이너 및 파일 공유 이름과 같은 정보는 **개요** 페이지로 이동 합니다. 계정 키 또는 SAS 토큰과 같은 인증 정보는 **설정** 창에서 **액세스 키** 로 이동 합니다. 

> [!IMPORTANT]
> 저장소 계정이 가상 네트워크에 있는 경우 Azure blob 데이터 저장소 만들기만 지원 됩니다. 작업 영역에 저장소 계정에 대 한 액세스 권한을 부여 하려면 매개 변수 `grant_workspace_access` `True`로 설정 합니다.

다음 예에서는 azure blob 컨테이너, Azure 파일 공유 및 Azure SQL 데이터를 데이터 저장소로 등록 하는 방법을 보여 줍니다.

#### <a name="blob-container"></a>Blob 컨테이너

Azure blob 컨테이너를 데이터 저장소로 등록 하려면 [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-)을 사용 합니다.

다음 코드는 `blob_datastore_name` 데이터 저장소를 만들어 `ws` 작업 영역에 등록 합니다. 이 데이터 저장소는 제공 된 계정 키를 사용 하 여 `my-account-name` 저장소 계정의 `my-container-name` blob 컨테이너에 액세스 합니다.

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```

#### <a name="file-share"></a>파일 공유

Azure 파일 공유를 데이터 저장소로 등록 하려면 [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-)을 사용 합니다. 

다음 코드는 `file_datastore_name` 데이터 저장소를 만들어 `ws` 작업 영역에 등록 합니다. 이 데이터 저장소는 제공 된 계정 키를 사용 하 여 `my-account-name` 저장소 계정의 `my-fileshare-name` 파일 공유에 액세스 합니다.

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                 datastore_name=file_datastore_name, 
                                                 file_share_name=file_share_name, 
                                                 account_name=account_name,
                                                 account_key=account_key)
```

#### <a name="sql-data"></a>SQL data

Azure SQL 데이터 저장소의 경우 [register_azure_sql_database ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-sql-database-workspace--datastore-name--server-name--database-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--endpoint-none--overwrite-false--username-none--password-none-) 를 사용 하 여 sql 인증 또는 서비스 사용자 권한으로 azure sql 데이터베이스에 연결 된 자격 증명 데이터 저장소를 등록 합니다. 

SQL 인증을 통해 등록 하려면:

```python
sql_datastore_name="azsqlsdksql"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the Azure SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the Azure SQL database
username=os.getenv("SQL_USER_NAME", "<my-sql-user-name>") # Username of the database user to access the database
password=os.getenv("SQL_USER_PASSWORD", "<my-sql-user-password>") # Password of the database user to access the database

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                  datastore_name=sql_datastore_name,
                                                  server_name=server_name,
                                                  database_name=database_name,
                                                  username=username,
                                                  password=password)

```

서비스 주체를 통해 등록 하려면:

```python 
sql_datastore_name="azsqlsdksp"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the SQL database
client_id=os.getenv("SQL_CLIENTNAME", "<my-client-id>") # Client ID of the service principal with permissions to access the database
client_secret=os.getenv("SQL_CLIENTSECRET", "<my-client-secret>") # Secret of the service principal
tenant_id=os.getenv("SQL_TENANTID", "<my-tenant-id>") # Tenant ID of the service principal

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                      datastore_name=sql_datastore_name,
                                                      server_name=server_name,
                                                      database_name=database_name,
                                                      client_id=client_id,
                                                      client_secret=client_secret,
                                                      tenant_id=tenant_id)
```

#### <a name="storage-guidance"></a>스토리지 지침

Azure blob 컨테이너를 권장 합니다. Blob에는 standard 및 premium storage를 모두 사용할 수 있습니다. Premium storage는 비용이 더 많이 들지만 더 빠른 처리량 속도는 특히 큰 데이터 집합에 대해 학습 하는 경우 학습 실행 속도를 향상 시킬 수 있습니다. 저장소 계정 비용에 대 한 자세한 내용은 [Azure 가격 계산기](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service)를 참조 하세요.

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio 

Azure Machine Learning studio에서 몇 가지 단계를 수행 하 여 새 데이터 저장소를 만듭니다.

1. [Azure Machine Learning Studio](https://ml.azure.com/)에 로그인합니다.
1. **관리**아래의 왼쪽 창에서 **datastores** 를 선택 합니다.
1. **+ 새 데이터 저장소**를 선택 합니다.
1. 새 데이터 저장소에 대 한 양식을 작성 합니다. 양식은 Azure Storage 유형 및 인증 유형에 대 한 선택 항목에 따라 지능적으로 업데이트 됩니다.
  
[Azure Portal](https://portal.azure.com)에서 폼을 채우는 데 필요한 정보를 찾을 수 있습니다. 왼쪽 창에서 **저장소 계정** 을 선택 하 고 등록 하려는 저장소 계정을 선택 합니다. **개요** 페이지에서 계정 이름, 컨테이너 및 파일 공유 이름과 같은 정보를 제공 합니다. 계정 키 또는 SAS 토큰과 같은 인증 항목의 경우 **설정** 창의 **계정 키** 로 이동 합니다.

다음 예제에서는 Azure blob 데이터 저장소를 만들 때 양식이 표시 되는 모양을 보여 줍니다. 
    
![새 데이터 저장소의 폼](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>작업 영역에서 데이터 저장소 가져오기

현재 작업 영역에 등록 된 특정 데이터 저장소를 가져오려면 `Datastore` 클래스에서 [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) 정적 메서드를 사용 합니다.

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
지정 된 작업 영역에 등록 된 데이터 저장소 목록을 가져오려면 작업 영역 개체에 대 한 [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores) 속성을 사용 하면 됩니다.

```Python
# List all datastores registered in the current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

작업 영역을 만들면 Azure blob 컨테이너 및 Azure 파일 공유가 작업 영역에 자동으로 등록 됩니다. 각각 `workspaceblobstore` 및 `workspacefilestore`라고 합니다. 작업 영역에 연결 된 저장소 계정에 프로 비전 된 blob 컨테이너 및 파일 공유에 대 한 연결 정보를 저장 합니다. `workspaceblobstore` 컨테이너는 기본 데이터 저장소로 설정 됩니다.

작업 영역의 기본 데이터 저장소를 가져오려면 다음 줄을 사용 합니다.

```Python
datastore = ws.get_default_datastore()
```

현재 작업 영역에 대해 다른 기본 데이터 저장소를 정의 하려면 작업 영역 개체에 대 한 [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) 메서드를 사용 합니다.

```Python
# Define the default datastore for the current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>데이터 업로드 및 다운로드

다음 예제에서 설명 하는 [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-) 및 [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-) 메서드는 [Azureblobdatastore 저장소](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py) 및 [azureblobdatastore 저장소](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py) 클래스에 대해 동일 하 게 작동 합니다.

### <a name="upload"></a>업로드

Python SDK를 사용 하 여 데이터 저장소에 디렉터리 또는 개별 파일을 업로드 합니다.

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

`target_path` 매개 변수는 업로드할 파일 공유 (또는 blob 컨테이너)의 위치를 지정 합니다. 기본값은 `None`이므로 데이터가 루트에 업로드 됩니다. `overwrite=True`경우 `target_path`의 모든 기존 데이터를 덮어씁니다.

`upload_files()` 메서드를 통해 데이터 저장소에 개별 파일 목록을 업로드할 수도 있습니다.

### <a name="download"></a>다운로드

데이터 저장소에서 로컬 파일 시스템으로 데이터를 다운로드 합니다.

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

`target_path` 매개 변수는 데이터를 다운로드할 로컬 디렉터리의 위치입니다. 다운로드할 파일 공유 또는 blob 컨테이너의 폴더 경로를 지정하려면 `prefix`에 해당 경로를 입력합니다. `prefix` `None`되 면 파일 공유 (또는 blob 컨테이너)의 모든 내용이 다운로드 됩니다.

<a name="train"></a>
## <a name="access-your-data-during-training"></a>학습 중 데이터 액세스

> [!IMPORTANT]
> 이제 [Azure Machine Learning 데이터 집합](how-to-create-register-datasets.md) 을 사용 하 여 학습에서 데이터에 액세스 하는 것이 좋습니다. 데이터 집합은 pandas 또는 Spark 데이터 프레임에 테이블 형식 데이터를 로드 하는 함수를 제공 합니다. 또한 데이터 집합은 Azure Blob storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database 및 Azure Database for PostgreSQL에서 모든 형식의 파일을 다운로드 하거나 탑재 하는 기능을 제공 합니다. [데이터 집합을 사용 하 여 학습 하는 방법에 대해 자세히 알아보세요](how-to-train-with-datasets.md).

다음 표에서는 실행 중에 데이터 저장소를 사용 하는 방법을 계산 대상에 지시 하는 방법을 보여 줍니다. 

Way|방법|Description|
----|-----|--------
탑재| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| 를 사용 하 여 계산 대상에 데이터 저장소를 탑재 합니다. 데이터 저장소가 탑재 되 면 계산 대상에서 데이터 저장소의 모든 파일에 액세스할 수 있습니다.
다운로드|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|를 사용 하 여 `path_on_compute`에 지정 된 위치에 데이터 저장소의 콘텐츠를 다운로드 합니다. <br><br> 이 다운로드는 실행 전에 발생 합니다.
업로드|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| `path_on_compute`에 지정 된 위치에서 데이터 저장소에 파일을 업로드 하는 데 사용 합니다. <br><br> 이 업로드는 실행 후에 수행 됩니다.

데이터 저장소의 특정 폴더 또는 파일을 참조 하 여 계산 대상에서 사용할 수 있도록 하려면 데이터 저장소 [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-) 메서드를 사용 합니다.

```Python
# To mount the full contents in your storage to the compute target
datastore.as_mount()

# To download the contents of only the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> 지정 된 `datastore` 또는 `datastore.path` 개체는 `"$AZUREML_DATAREFERENCE_XXXX"`형식의 환경 변수 이름으로 확인 됩니다. 이 이름의 값은 계산 대상의 탑재/다운로드 경로를 나타냅니다. 계산 대상의 데이터 저장소 경로가 학습 스크립트의 실행 경로와 다를 수 있습니다.

### <a name="examples"></a>예시 

학습 중에 데이터에 액세스 하는 데 [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) 클래스를 사용 하는 것이 좋습니다. 

`script_params` 변수는 `entry_script`매개 변수를 포함 하는 사전입니다. 이를 사용 하 여 데이터 저장소를 전달 하 고 계산 대상에서 데이터를 사용할 수 있는 방법을 설명 합니다. [종단 간 자습서](tutorial-train-models-with-aml.md)에서 자세히 알아보세요.

```Python
from azureml.train.estimator import Estimator

# Notice that '/' is in front, which indicates the absolute path
script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

데이터 저장소 목록을 `Estimator` 생성자의 `inputs` 매개 변수에 전달 하 여 데이터 저장소 간에 데이터를 탑재 하거나 복사할 수도 있습니다. 이 코드 예제는 다음과 같습니다.
* `train.py` 학습 스크립트를 실행 하기 전에 `datastore1`의 모든 콘텐츠를 계산 대상으로 다운로드 합니다.
* `train.py` 실행 되기 전에 `datastore2`의 `'./foo'` 폴더를 계산 대상으로 다운로드 합니다.
* 스크립트를 실행 한 후 계산 대상의 `'./bar.pkl'` 파일을 `datastore3`에 업로드 합니다.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
학습에 `RunConfig` 개체를 사용 하려는 경우 [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) 개체를 설정 해야 합니다. 

다음 코드에서는 예측 파이프라인에서 `DataReference` 개체를 사용 하는 방법을 보여 줍니다. 전체 예제는 [이 노트북](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-estimatorstep.ipynb)을 참조 하세요.

```Python
from azureml.core import Datastore
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

def_blob_store = Datastore(ws, "workspaceblobstore")

input_data = DataReference(
       datastore=def_blob_store,
       data_reference_name="input_data",
       path_on_datastore="20newsgroups/20news.pkl")

output = PipelineData("output", datastore=def_blob_store)
```
<a name="matrix"></a>

### <a name="compute-and-datastore-matrix"></a>계산 및 데이터 저장소 행렬

Datastores는 현재 다음 행렬에 나열 된 저장소 서비스에 대 한 연결 정보를 저장 하는 기능을 지원 합니다. 이 행렬은 다양 한 계산 대상 및 데이터 저장소 시나리오에 사용할 수 있는 데이터 액세스 기능을 표시 합니다. [Azure Machine Learning에 대 한 계산 대상에 대해 자세히 알아보세요](how-to-set-up-training-targets.md#compute-targets-for-training).

|컴퓨팅|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| 지방|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|N/A         |N/A                                                                         |
| Azure Machine Learning 컴퓨팅 |[as_mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download (](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [Machine Learning 파이프라인](concept-ml-pipelines.md)|[as_mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download (](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-)), [as_upload ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [Machine Learning 파이프라인](concept-ml-pipelines.md)|N/A         |N/A                                                                         |
| Virtual Machines               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/A         |N/A                                                                         |
| Azure HDInsight                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |N/A         |N/A                                                                         |
| 데이터 전송                  |[파이프라인 Machine Learning](concept-ml-pipelines.md)                                               |N/A                                           |[파이프라인 Machine Learning](concept-ml-pipelines.md)            |[파이프라인 Machine Learning](concept-ml-pipelines.md)                                                                            |
| Azure Databricks                     |[파이프라인 Machine Learning](concept-ml-pipelines.md)                                              |N/A                                           |[파이프라인 Machine Learning](concept-ml-pipelines.md)             |N/A                                                                         |
| Azure Batch                    |[파이프라인 Machine Learning](concept-ml-pipelines.md)                                               |N/A                                           |N/A         |N/A                                                                         |
| Azure 데이터 레이크 분석       |N/A                                           |N/A                                           |[파이프라인 Machine Learning](concept-ml-pipelines.md)             |N/A                                                                         |

> [!NOTE]
> 매우 반복적인 큰 데이터 프로세스가 `as_mount()`대신 `as_download()`를 사용 하 여 더 빠르게 실행 되는 시나리오가 있을 수 있습니다. 이 experimentally의 유효성을 검사할 수 있습니다.

### <a name="accessing-source-code-during-training"></a>학습 중 소스 코드 액세스

Azure Blob storage는 Azure 파일 공유 보다 더 높은 처리량 속도를 가지 며 동시에 시작 된 많은 수의 작업으로 확장 됩니다. 따라서 소스 코드 파일을 전송 하는 데 Blob storage를 사용 하도록 실행을 구성 하는 것이 좋습니다.

다음 코드 예제에서는 실행 구성에서 소스 코드 전송에 사용할 blob 데이터 저장소를 지정 합니다.

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>점수 매기기 중 데이터 액세스

Azure Machine Learning에서는 모델을 사용 하 여 점수를 매기는 여러 가지 방법을 제공 합니다. 이러한 메서드 중 일부는 datastores에 대 한 액세스를 제공 하지 않습니다. 다음 표를 사용 하 여 점수 매기기 중에 데이터 저장소에 액세스할 수 있는 방법을 알아봅니다.

| 방법 | 데이터 저장소 액세스 | Description |
| ----- | :-----: | ----- |
| [일괄 처리 예측](how-to-run-batch-predictions.md) | ✔ | 많은 양의 데이터를 비동기적으로 예측 합니다. |
| [웹 서비스](how-to-deploy-and-where.md) | &nbsp; | 모델을 웹 서비스로 배포 합니다. |
| [Azure IoT Edge 모듈](how-to-deploy-and-where.md) | &nbsp; | IoT Edge 장치에 모델을 배포 합니다. |

SDK가 datastores에 대 한 액세스를 제공 하지 않는 경우에는 관련 Azure SDK를 사용 하 여 데이터에 액세스 하 여 사용자 지정 코드를 만들 수 있습니다. 예를 들어 [Python 용 AZURE STORAGE SDK](https://github.com/Azure/azure-storage-python) 는 blob 또는 파일에 저장 된 데이터에 액세스 하는 데 사용할 수 있는 클라이언트 라이브러리입니다.

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>지원 되는 Azure Storage 솔루션으로 데이터 이동

Azure Machine Learning는 Azure Blob storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database 및 Azure Database for PostgreSQL의 데이터에 액세스 하는 것을 지원 합니다. 지원 되지 않는 저장소를 사용 하는 경우 Azure Data Factory를 사용 하 여 지원 되는 Azure Storage 솔루션으로 데이터를 이동 하는 것이 좋습니다. 지원 되는 저장소로 데이터를 이동 하면 기계 학습 실험 중에 데이터 송신 비용을 절감할 수 있습니다. 

Azure Data Factory는 80 개 이상의 미리 작성 된 커넥터를 추가 비용 없이 효율적이 고 복원 력 있는 데이터 전송을 제공 합니다. 이러한 커넥터에는 Azure 데이터 서비스, 온-프레미스 데이터 원본, Amazon S3 및 Redshift 및 Google 이상 쿼리가 포함 됩니다. 단계별 [가이드에 따라 Azure Data Factory를 사용 하 여 데이터를 이동](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool)합니다.

## <a name="next-steps"></a>다음 단계

* [모델 학습](how-to-train-ml-models.md)
* [모델 배포](how-to-deploy-and-where.md)
