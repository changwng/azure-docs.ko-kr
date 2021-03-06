---
title: Azure HDInsight의 Apache Ambari 하트 비트 문제
description: Azure HDInsight에서 Apache Ambari 하트 비트 문제에 대 한 다양 한 이유 검토
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 09/11/2019
ms.openlocfilehash: ae5cfcfcd394aab644b35ac66aafa213dc49dd42
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895376"
---
# <a name="apache-ambari-heartbeat-issues-in-azure-hdinsight"></a>Azure HDInsight의 Apache Ambari 하트 비트 문제

이 문서에서는 Azure HDInsight 클러스터와 상호 작용할 때 문제에 대 한 문제 해결 단계 및 가능한 해결 방법을 설명 합니다.

## <a name="scenario-high-cpu-utilization"></a>시나리오: 높은 CPU 사용률

### <a name="issue"></a>문제

Ambari 에이전트의 CPU 사용률이 높아,이로 인해 일부 노드에 대해 Ambari agent 하트 비트가 손실 된 Ambari UI의 경고가 발생 합니다. 하트 비트 손실 경고는 일반적으로 일시적입니다. 

### <a name="cause"></a>원인

드문 경우 지만 다양 한 ambari-에이전트 버그로 인해 ambari (100)의 CPU 사용률이 높을 수 있습니다.

### <a name="resolution"></a>해상도

1. Ambari-에이전트의 pid (프로세스 ID)를 식별 합니다.

    ```bash
    ps -ef | grep ambari_agent
    ```

1. 그 후 다음 명령을 실행하여 CPU 사용률을 표시합니다.

    ```bash
    top -p <ambari-agent-pid>
    ```

1. 문제를 완화 하기 위해 ambari-에이전트를 다시 시작 합니다.

    ```bash
    service ambari-agent restart
    ```

1. 다시 시작이 작동 하지 않으면 ambari 프로세스를 중지 하 고 시작 합니다.

    ```bash
    kill -9 <ambari-agent-pid>
    service ambari-agent start
    ```

---

## <a name="scenario-ambari-agent-not-started"></a>시나리오: Ambari 에이전트가 시작 되지 않음

### <a name="issue"></a>문제

Ambari 에이전트가 시작 되지 않았기 때문에 Ambari UI에서 Ambari 에이전트 하트 비트가 손실 된 일부 노드에 대 한 경고가 발생 했습니다.

### <a name="cause"></a>원인

Ambari 에이전트가 실행 되 고 있지 않기 때문에 경고가 발생 합니다.

### <a name="resolution"></a>해상도

1. Ambari: 에이전트의 상태를 확인 합니다.

    ```bash
    service ambari-agent status
    ```

1. 장애 조치 (failover) 컨트롤러 서비스가 실행 중인지 확인 합니다.

    ```bash
    ps -ef | grep failover
    ```

    장애 조치 (failover) 컨트롤러 서비스가 실행 되 고 있지 않으면 hdinsight 에이전트가 장애 조치 (failover) 컨트롤러를 시작 하지 못하도록 하는 문제가 원인일 수 있습니다. `/var/log/hdinsight-agent/hdinsight-agent.out` 파일에서 hdinsight 에이전트 로그를 확인 합니다.

---

## <a name="next-steps"></a>다음 단계

문제가 표시되지 않거나 문제를 해결할 수 없는 경우 다음 채널 중 하나를 방문하여 추가 지원을 받으세요.

* Azure [커뮤니티 지원을](https://azure.microsoft.com/support/community/)통해 azure 전문가 로부터 답변을 받으세요.

* [@AzureSupport](https://twitter.com/azuresupport) 연결-Azure 커뮤니티를 적절 한 리소스 (답변, 지원 및 전문가)에 연결 하 여 고객 환경을 개선 하기 위한 공식 Microsoft Azure 계정입니다.

* 도움이 더 필요한 경우 [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)에서 지원 요청을 제출할 수 있습니다. 메뉴 모음에서 **지원** 을 선택 하거나 **도움말 + 지원** 허브를 엽니다. 자세한 내용은 [Azure 지원 요청을 만드는 방법](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)을 참조 하세요. 구독 관리 및 청구 지원에 대 한 액세스는 Microsoft Azure 구독에 포함 되며, [Azure 지원 계획](https://azure.microsoft.com/support/plans/)중 하나를 통해 기술 지원이 제공 됩니다.
