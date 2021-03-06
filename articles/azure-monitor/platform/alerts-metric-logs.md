---
title: Azure Monitor에서 로그 메트릭 경고 만들기
description: 인기 있는 로그 분석 데이터에 대해 실시간에 가까운 메트릭 경고를 만드는 자습서입니다.
author: yanivlavi
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: d6b65b76138cb180ab105631ebc0f19b7d38f206
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226530"
---
# <a name="create-metric-alerts-for-logs-in-azure-monitor"></a>Azure Monitor에서 로그 메트릭 경고 만들기

## <a name="overview"></a>개요

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor는 [클래식 경고](../../azure-monitor/platform/alerts-metric-near-real-time.md)보다 이점이 있는 [메트릭 경고 유형](../../azure-monitor/platform/alerts-classic-portal.md)을 지원합니다. [다양한 Azure 서비스](../../azure-monitor/platform/metrics-supported.md)에 대해 메트릭을 사용할 수 있습니다. 이 문서에서는 리소스(`Microsoft.OperationalInsights/workspaces`)의 하위 집합을 사용하는 방법에 대해 설명합니다.

Azure 또는 온-프레미스의 리소스를 포함 하 여 로그에서 메트릭의 일부로 추출 된 인기 있는 Log Analytics 로그에서 메트릭 경고를 사용할 수 있습니다. 지원되는 Log Analytics 솔루션은 다음과 같습니다.

- Windows 및 Linux 컴퓨터에 대한 [성능 카운터](../../azure-monitor/platform/data-sources-performance-counters.md)
- [에이전트 상태에 대한 하트비트 레코드](../../azure-monitor/insights/solution-agenthealth.md)
- [업데이트 관리](../../automation/automation-update-management.md) 레코드
- [이벤트 데이터](../../azure-monitor/platform/data-sources-windows-events.md) 로그

Azure에서 **로그 메트릭 경고**를 사용하면 쿼리 기반 [로그 경고](../../azure-monitor/platform/alerts-log.md)에 비해 많은 이점이 있습니다. 그 중 일부는 다음과 같습니다.

- 메트릭 경고는 실시간에 가까운 모니터링 기능을 제공하며, 로그 메트릭 경고는 로그 원본의 데이터를 포크하여 동일하게 보장합니다.
- 메트릭 경고는 상태 저장이며, 경고가 발생할 때와 해결될 때 각각 한 번만 알려줍니다. 상태 비저장이며 경고 조건이 충족되는 경우 매 간격마다 계속 실행되는 로그 경고와는 대조적입니다.
- 로그 메트릭 경고는 여러 차원을 제공하여 분석에서 쿼리를 작성할 필요 없이 컴퓨터, OS 유형 등과 같은 특정 값으로 더 간단하게 필터링할 수 있습니다.

> [!NOTE]
> 특정 메트릭 및/또는 차원은 선택한 기간에 해당 데이터가 존재할 때만 표시됩니다. 이러한 메트릭은 Azure Log Analytics 작업 영역을 사용하는 고객에게 제공됩니다.

## <a name="metrics-and-dimensions-supported-for-logs"></a>로그에 지원되는 메트릭 및 차원

 메트릭 경고는 차원을 사용하는 메트릭에 대한 경고를 지원합니다. 차원을 사용하여 메트릭을 적절한 수준으로 필터링할 수 있습니다. 지원되는 솔루션 전체에서 [Log Analytics 작업 영역](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)의 로그에 지원되는 전체 메트릭 목록이 나열됩니다.

> [!NOTE]
> [Azure Monitor - 메트릭](../../azure-monitor/platform/metrics-charts.md)을 통해 Log Analytics 작업 영역에서 추출할 수 있는 지원되는 메트릭을 보려면 해당 메트릭에 대한 로그 메트릭 경고가 만들어져야 합니다. 로그 메트릭 경고에서 선택된 차원은 [Azure Monitor - 메트릭]을 통한 탐색에만 표시됩니다.

## <a name="creating-metric-alert-for-log-analytics"></a>Log Analytics에 대한 메트릭 경고 만들기

인기 있는 로그의 메트릭 데이터는 Log Analytics에서 [Azure Monitor - 메트릭]으로 처리되기 전에 파이프됩니다. 이렇게 하면 사용자가 메트릭 플랫폼과 메트릭 경고(최대 1분 정도의 빈도가 있는 경고 포함)의 기능을 모두 활용할 수 있습니다.
로그 메트릭 경고를 만드는 방법은 다음과 같습니다.

## <a name="prerequisites-for-metric-alert-for-logs"></a>로그 메트릭 경고의 필수 조건

Log Analytics 데이터에서 수집된 로그에 대한 메트릭이 작동하려면 다음을 설정하여 사용할 수 있어야 합니다.

1. **활성 Log Analytics 작업 영역**: 유효한 활성 Log Analytics 작업 영역이 있어야 합니다. 자세한 내용은 [Azure Portal에서 Log Analytics 작업 영역 만들기](../../azure-monitor/learn/quick-create-workspace.md)를 참조하세요.
2. **에이전트가 Log Analytics 작업 영역에 대해 구성 되어**있습니다. Azure vm (및/또는) 온-프레미스 vm에 대해 에이전트를 구성 하 여 이전 단계에서 사용 되는 Log Analytics 작업 영역으로 데이터를 전송 해야 합니다. 자세한 내용은 [Log Analytics - 에이전트 개요](../../azure-monitor/platform/agents-overview.md)를 참조하세요.
3. **지원 되는 Log Analytics 솔루션이 설치 됨**: Log Analytics 솔루션을 구성 하 고 Log Analytics 작업 영역에 데이터를 전송 해야 합니다. 지원 되는 솔루션은 Windows & Linux, [에이전트 상태 하트 비트 레코드](../../azure-monitor/insights/solution-agenthealth.md), [업데이트 관리](../../automation/automation-update-management.md)및 [이벤트 데이터](../../azure-monitor/platform/data-sources-windows-events.md)에 [대 한 성능 카운터](../../azure-monitor/platform/data-sources-performance-counters.md)입니다.
4. **로그를 보내도록 Log Analytics 솔루션 구성**: Log Analytcis 솔루션은 [Log Analytics 작업 영역에 지원되는 메트릭](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)에 해당하는 필수 로그/데이터를 사용하도록 설정해야 합니다. 예를 들어 *% 사용 가능한 메모리* 카운터는 먼저 [성능 카운터](../../azure-monitor/platform/data-sources-performance-counters.md) 솔루션에 구성해야 합니다.

## <a name="configuring-metric-alert-for-logs"></a>로그 메트릭 경고 구성

 메트릭 경고는 Azure Portal, Resource Manager 템플릿, REST API, PowerShell 및 Azure CLI를 사용하여 만들고 관리할 수 있습니다. 로그 메트릭 경고는 메트릭 경고의 변형이므로 필수 조건이 완료되면 지정된 Log Analytics 작업 영역에 대한 로그 메트릭 경고를 만들 수 있습니다. 페이로드 스키마, 적용 가능한 할당량 한도 및 청구 가격을 포함하여 [메트릭 경고](../../azure-monitor/platform/alerts-metric-near-real-time.md)의 모든 특성과 기능은 로그 메트릭 경고에도 적용할 수 있습니다.

단계별 세부 정보 및 샘플은 [메트릭 경고 만들기 및 관리](https://aka.ms/createmetricalert)를 참조하세요. 특히 로그 메트릭 경고의 경우 메트릭 경고 관리 지침을 따르고 다음 사항을 확인합니다.

- 메트릭 경고의 대상은 유효한 *Log Analytics 작업 영역*입니다.
- 선택한 *Log Analytics 작업 영역*에 대한 메트릭 경고로 선택한 신호는 **메트릭** 유형입니다.
- 차원 필터를 사용하여 특정 조건 또는 리소스를 필터링합니다. 로그에 대한 메트릭은 다차원입니다.
- *신호 논리*를 구성할 때 단일 경고를 만들어 여러 차원 값(예: 컴퓨터)에 걸쳐 있을 수 있습니다.
- Azure Portal을 사용하여 선택한 **Log Analytics 작업 영역**에 대한 메트릭 경고를 만들지 *않는* 경우, 사용자는 먼저 [Azure Monitor - 예약된 쿼리 규칙](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)을 사용하여 로그 데이터를 메트릭으로 변환하는 명시적 규칙을 수동으로 만들어야 합니다.

> [!NOTE]
> Azure Portal을 통해 Log Analytics 작업 영역에 대한 메트릭 경고를 만드는 경우, [사용자가 개입하거나 작업할 필요 없이](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) *Azure Monitor - 예약된 쿼리 규칙*을 통해 로그 데이터를 메트릭으로 변환하는 해당 규칙이 백그라운드에서 자동으로 만들어집니다. Azure Portal 이외의 수단을 사용하여 로그 메트릭 경고를 만드는 경우, 메트릭 경고를 만들기 전에 ScheduledQueryRule 기반 로그를 메트릭 변환 규칙으로 만드는 샘플 수단에 대한 [로그 메트릭 경고에 대한 리소스 템플릿](#resource-template-for-metric-alerts-for-logs) 섹션을 참조하세요. 그렇지 않으면 만들어진 로그 메트릭 경고에 대한 데이터가 없습니다.

## <a name="resource-template-for-metric-alerts-for-logs"></a>로그 메트릭 경고에 대한 리소스 템플릿

앞에서 설명한 대로 로그에서 메트릭 경고를 만드는 프로세스는 다음 두 가지 단계로 구분됩니다.

1. scheduledQueryRule API를 사용하여 지원되는 로그에서 메트릭을 추출하는 규칙을 만듭니다.
2. 로그에서 추출한 메트릭(1단계)과 Log Analytics 작업 영역에 대한 메트릭 경고를 대상 리소스로 만듭니다.

### <a name="metric-alerts-for-logs-with-static-threshold"></a>정적 임계값을 사용한 로그 메트릭 경고

이를 위해 아래의 Azure Resource Manager 템플릿 샘플을 사용할 수 있습니다. 여기서 정적 임계값 메트릭 경고 만들기는 scheduledQueryRule을 통해 로그에서 메트릭을 추출하는 규칙의 성공적인 생성 여부에 달려 있습니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

위의 JSON이 metricfromLogsAlertStatic.json으로 저장되었다고 가정하면 리소스 템플릿 기반 만들기에 대한 매개 변수 JSON 파일과 결합할 수 있습니다. 매개 변수 JSON 파일 샘플은 다음과 같습니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule" 
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterThan"
        },
        "threshold":{
            "value": "1"
        },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

위의 매개 변수 파일이 metricfromLogsAlertStatic.parameters.json으로 저장되었다고 가정하면 [Azure Portal에서 만들기 위한 리소스 템플릿](../../azure-resource-manager/resource-group-template-deploy-portal.md)을 사용하여 로그 메트릭 경고를 만들 수 있습니다.

또는 아래의 Azure Powershell 명령을 사용할 수도 있습니다.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertStatic.json TemplateParameterFile metricfromLogsAlertStatic.parameters.json
```

또는 Azure CLI를 통해 리소스 템플릿 배포를 사용합니다.

```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlertStatic.json --parameters @metricfromLogsAlertStatic.parameters.json
```

### <a name="metric-alerts-for-logs-with-dynamic-thresholds"></a>동적 임계값을 사용하는 로그 트릭 경고

이를 위해 아래의 Azure Resource Manager 템플릿 샘플을 사용할 수 있습니다. 여기서 동적 임계값 메트릭 경고 만들기는 scheduledQueryRule을 통해 로그에서 메트릭을 추출하는 규칙의 성공적인 생성 여부에 달려 있습니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterOrLessThan",
            "allowedValues": [
                "GreaterThan",
                "LessThan",
                "GreaterOrLessThan"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "alertSensitivity": {
            "type": "string",
            "defaultValue": "Medium",
            "allowedValues": [
                "High",
                "Medium",
                "Low"
            ],
            "metadata": {
                "description": "Tunes how 'noisy' the Dynamic Thresholds alerts will be: 'High' will result in more alerts while 'Low' will result in fewer alerts."
            }
        },
        "numberOfEvaluationPeriods": {
            "type": "string",
            "defaultValue": "4",
            "metadata": {
                "description": "The number of periods to check in the alert evaluation."
            }
        },
        "minFailingPeriodsToAlert": {
            "type": "string",
            "defaultValue": "3",
            "metadata": {
                "description": "The number of unhealthy periods to alert on (must be lower or equal to numberOfEvaluationPeriods)."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.MultipleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "criterionType": "DynamicThresholdCriterion",
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "alertSensitivity": "[parameters('alertSensitivity')]",
                            "failingPeriods": {
                                "numberOfEvaluationPeriods": "[parameters('numberOfEvaluationPeriods')]",
                                "minFailingPeriodsToAlert": "[parameters('minFailingPeriodsToAlert')]"
                            },
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

위의 JSON이 metricfromLogsAlertDynamic.json으로 저장되었다고 가정하면 리소스 템플릿 기반 만들기에 대한 매개 변수 JSON 파일과 결합할 수 있습니다. 매개 변수 JSON 파일 샘플은 다음과 같습니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule"
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterOrLessThan"
          },
          "alertSensitivity": {
              "value": "Medium"
          },
          "numberOfEvaluationPeriods": {
              "value": "4"
          },
          "minFailingPeriodsToAlert": {
              "value": "3"
          },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

위의 매개 변수 파일이 metricfromLogsAlertDynamic.parameters.json으로 저장되었다고 가정하면 [Azure Portal에서 만들기 위한 리소스 템플릿](../../azure-resource-manager/resource-group-template-deploy-portal.md)을 사용하여 로그 메트릭 경고를 만들 수 있습니다.

또는 아래의 Azure Powershell 명령을 사용할 수도 있습니다.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertDynamic.json TemplateParameterFile metricfromLogsAlertDynamic.parameters.json
```

또는 Azure CLI를 통해 리소스 템플릿 배포를 사용합니다.

```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlertDynamic.json --parameters @metricfromLogsAlertDynamic.parameters.json
```

## <a name="next-steps"></a>다음 단계

- [메트릭 경고](alerts-metric.md)에 대해 자세히 알아봅니다.
- [Azure의 로그 경고](../../azure-monitor/platform/alerts-unified-log.md)에 대해 알아봅니다.
- [Azure의 경고](alerts-overview.md)에 대해 알아봅니다.
