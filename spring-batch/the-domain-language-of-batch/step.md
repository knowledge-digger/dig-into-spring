---
description: 이 페이지에서는 Spring Batch의 step에 대해 설명합니다.
---

# Step

Step은 배치 job의 독립적이고 순차적인 단계를 캡슐화한 도메인 객체입니다.

즉, 모든 job은 하나 이상의 step으로 구성됩니다

step은 실제 배치 처리를 정의하고 컨트롤하는 데 필요한 모든 정보를 가지고 있습니다.

아래 그림처럼 `job`이 고유 `JobExecution` 이 있듯, step도 각자의 `stepExecution` 이 있습니다.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Job Hierarchy With Steps</p></figcaption></figure>

2.1 **StepExecution**

StepExecution은 한 번의 step 실행 시도를 의미합니다. StepExecution은 JobExecution과 유사하게 Step을 실행할 때마다 생성합니다.

하지만 이전 단계 step이 실패해서 step을 실행하지 않았다면 execution을 저장하지 않는다.

step이 실제로 시작됐을 때만 StepExcution을 생성합니다.
