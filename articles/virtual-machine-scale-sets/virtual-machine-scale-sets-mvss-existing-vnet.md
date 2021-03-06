---
title: Azure 확장 집합 템플릿에서 기존 가상 네트워크 참조 | Microsoft Docs
description: 기존 Azure Virtual Machine Scale Set 템플릿에 가상 네트워크를 추가하는 방법 알아보기
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 8b75b9898eb767866c0843594a82570cfb65d122
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64868947"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Azure 확장 집합 템플릿에 기존 가상 네트워크에 대한 참조 추가

이 문서를 수정 하는 방법을 보여 줍니다 합니다 [기본 확장 집합 템플릿](virtual-machine-scale-sets-mvss-start.md) 새로 만드는 대신 기존 가상 네트워크에 배포 합니다.

## <a name="change-the-template-definition"></a>템플릿 정의 변경

에 [이전 문서](virtual-machine-scale-sets-mvss-start.md) 기본 확장 집합 템플릿을 작성 합니다. 이제 이전 템플릿을 사용 하 여 하 고 수정 하 여 기존 가상 네트워크에 크기 집합을 배포 하는 템플릿을 만듭니다. 

먼저 `subnetId` 매개 변수를 추가합니다. 이 문자열은 확장 집합 구성에 전달되어 확장 집합에서 미리 만든 서브넷을 식별하여 가상 머신을 배포할 수 있게 합니다. 이 문자열 형식 이어야 합니다. `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

예를 들어 `myvnet` 이름, `mysubnet` 서브넷, `myrg` 리소스 그룹 및 `00000000-0000-0000-0000-000000000000` 구독을 사용하여 기존 가상 네트워크에 확장 집합을 배포하려면 subnetId가 `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`이 됩니다.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

다음으로 기존 가상 네트워크를 사용하고 있고 새 가상 네트워크를 배포할 필요가 없기 때문에 `resources` 배열에서 가상 네트워크 리소스를 삭제합니다.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2018-11-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

템플릿을 배포하기 전에 가상 네트워크가 이미 있으므로 확장 집합에서 가상 네트워크로 dependsOn 절을 지정할 필요가 없습니다. 다음 줄을 삭제합니다.

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

마지막으로 전달 합니다 `subnetId` 사용자가 설정한 매개 변수 (사용 하는 대신 `resourceId` 동일한 배포에서 vnet의 ID를 가져오려면는 어떤 기본 실행 가능한 확장 집합 템플릿에서 수행).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>다음 단계

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
