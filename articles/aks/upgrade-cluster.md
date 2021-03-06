---
title: AKS(Azure Kubernetes Service) 클러스터 업그레이드
description: AKS(Azure Kubernetes Service) 클러스터를 업그레이드하는 방법 알아보기
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: e1ef87471c81fbf0d242837bd94fdc17f686387f
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75889397"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>AKS(Azure Kubernetes Service) 클러스터 업그레이드

AKS 클러스터의 수명 주기의 일환으로, 최신 Kubernetes 버전으로 업그레이드해야 하는 경우가 많습니다. 최신 Kubernetes 보안 릴리스를 적용하거나 최신 기능으로 업그레이드하는 것은 중요합니다. 이 문서에서는 AKS 클러스터에서 마스터 구성 요소 또는 단일 기본 노드 풀을 업그레이드 하는 방법을 보여 줍니다.

여러 노드 풀 또는 Windows Server 노드를 사용 하는 AKS 클러스터 (현재 AKS에서 미리 보기 상태)의 경우 [AKS에서 노드 풀 업그레이드][nodepool-upgrade]를 참조 하세요.

## <a name="before-you-begin"></a>시작하기 전에

이 문서에서는 Azure CLI 버전 2.0.65 이상을 실행 해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치][azure-cli-install]를 참조하세요.

> [!WARNING]
> AKS 클러스터 업그레이드는 노드의 cordon 드레이닝을 트리거합니다. 사용 가능한 계산 할당량이 낮은 경우 업그레이드가 실패할 수 있습니다.  자세한 내용은 [할당량 늘리기](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request?branch=pr-en-us-83289) 를 참조 하세요.
> 사용자 고유의 클러스터 autoscaler 배포를 실행 하는 경우 업그레이드 프로세스를 방해할 가능성이 있으므로 업그레이드 하는 동안이를 사용 하지 않도록 설정 하세요 (0 개의 복제본으로 크기를 조정할 수 있음). 관리 되는 autoscaler이이를 자동으로 처리 합니다. 

## <a name="check-for-available-aks-cluster-upgrades"></a>사용할 수 있는 AKS 클러스터 업그레이드를 위한 확인

클러스터에 사용할 수 있는 Kubernetes 릴리스를 확인 하려면 [az aks][az-aks-get-upgrades] 명령을 사용 합니다. 다음 예제에서는 *myResourceGroup*이라는 리소스 그룹에서 *myAKSCluster*라는 클러스터로 가능한 업그레이드를 확인합니다.

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> AKS 클러스터를 업그레이드하는 경우 Kubernetes 부 버전은 건너뛸 수 없습니다. 예를 들어 ->  -> *1.12* x x x *x x* x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x. x *는 -> * *1.14*
>
>  -> x x x x x x *x x x*x x x x x x. *x에서 업그레이드* *하려면 -> 먼저* *1.14*에서 *1.14*를 업그레이드 한 다음, *1.13 -> .* x에서 업그레이드 합니다.

다음 예제 출력에서는 *1.13.9* 및 *1.13.10*버전으로 클러스터를 업그레이드할 수 있음을 보여 줍니다.

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
업그레이드를 사용할 수 없는 경우 다음을 얻게 됩니다.
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="upgrade-an-aks-cluster"></a>AKS 클러스터 업그레이드

AKS 클러스터에 사용할 수 있는 버전의 목록을 사용 하 여 업그레이드 하려면 [az AKS upgrade][az-aks-upgrade] 명령을 사용 합니다. 업그레이드 프로세스가 진행 되는 동안 AKS는 지정 된 Kubernetes 버전을 실행 하는 클러스터에 새 노드를 추가한 다음 실행 중인 응용 프로그램의 중단을 최소화 하기 위해 이전 노드 중 하나를 중단 [하 고 드레이닝][kubernetes-drain] 합니다. 새 노드가 application pod를 실행 하는 것으로 확인 되 면 이전 노드가 삭제 됩니다. 이 프로세스는 클러스터의 모든 노드가 업그레이드 될 때까지 반복 됩니다.

다음 예제에서는 클러스터를 *1.13.10*버전으로 업그레이드 합니다.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.13.10
```

노드 수에 따라 클러스터를 업그레이드하는 데 몇 분 정도 걸립니다. 

> [!NOTE]
> 클러스터 업그레이드를 완료 하는 데 허용 되는 총 시간이 있습니다. 이 시간은 `10 minutes * total number of nodes in the cluster`제품을 활용 하 여 계산 됩니다. 예를 들어 20 개 노드 클러스터에서 업그레이드 작업은 200 분 내에 성공 해야 합니다. 그렇지 않으면 AKS는 복구 불가능 한 클러스터 상태를 방지 하기 위해 작업에 실패 합니다. 업그레이드 실패 시 복구 하려면 시간 제한이 적중 된 후 업그레이드 작업을 다시 시도 하세요.

업그레이드에 성공 했는지 확인 하려면 [az aks show][az-aks-show] 명령을 사용 합니다.

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

다음 예제 출력에서는 클러스터가 이제 *1.13.10*를 실행 하는 방법을 보여 줍니다.

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 기존 AKS 클러스터를 업그레이드하는 방법을 설명했습니다. AKS 클러스터의 배포 및 관리에 대해 더 자세히 알아보려면 자습서 세트를 참조하세요.

> [!div class="nextstepaction"]
> [AKS 자습서][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
