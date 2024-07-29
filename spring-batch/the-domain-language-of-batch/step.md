---
description: 이 페이지에서는 Spring Batch의 step에 대해 설명합니다.
---

# Step

`Step`은 배치 `job`의 독립적이고 순차적인 단계를 **캡슐화한 도메인 객체**입니다.

즉, 모든 `job`은 하나 이상의 `step`으로 구성됩니다.



### 2. Step

`step`은 실제 배치 처리를 정의하고 컨트롤하는 데 필요한 모든 정보를 가지고 있습니다.

아래 그림처럼 `job`이 고유 `JobExecution`이 있듯, `step`도 각자의 `stepExecution`이 있습니다.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p><a href="https://docs.spring.io/spring-batch/reference/domain.html">https://docs.spring.io/spring-batch/reference/domain.html</a></p></figcaption></figure>

2.1 **StepExecution**

`StepExecution`은 **한 번의 `step` 실행 시도**를 의미합니다.&#x20;

`StepExecution`은 `JobExecution`과 유사하게 `step`을 실행할 때마다 생성합니다.



하지만 이전 단계 `step`이 실패해서 `step`을 실행하지 않았다면 `execution`을 저장하지 않습니다.

`step`이 **실제로 시작됐을 때만** `StepExcution`을 생성합니다.
