---
description: 이 페이지에서는 Spring Batch의 job에 대해 설명합니다.
---

# Job

스프링 배치(Spring Batch)는 배치 애플리케이션을 설계하고 구현하는 데 필요한 다양한 구성 요소로 이루어져 있습니다.

다음 다이어그램은 스프링 배치의 도메인 언어를 구성하는 핵심 개념을 나타냅니다.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Batch Stereotypes</p></figcaption></figure>



Job 하나는 1\~n개의 step을 가지고 있으며,

각 step은 `ItemReader`, `ItemProcessor`,`ItemWriter`를 1개씩 가지고 있습니다.

각 Job은 `JobLauncher` 가 실행하며,

현재 실행중인 프로세스의 메타정보는 `JobRepository` 에 저장됩니다.

이제 각 구성요소에 대해 더 자세히 살펴보겠습니다.



### 1. Job&#x20;

`job`은  전체적인 배치 프로세스를 캡슐화한 **최상위 엔티티**입니다.

또한 Step 인스턴트의 컨테이너 개념으로 볼 수 있습니다. 한 플로우에 속한 여러 step을 결합하고, 재시작 같은 속성을 **전역**으로 구성할 수 있습니다.



#### **1.1 JobInstance**

`jobInstance` 는 **논리적인 job 실행**을 뜻합니다.

위 다이어그램의 ‘EndOfDay’ `job` 처럼 하루가 끝날 때마다 한 번 실행되야 하는 배치 job을 생각해보겠습니다.

‘EndOfDay’ job은 하나지만, `job`을 각각 실행할 때마다 각각 추적할 수 있어야 합니다. 이 예시에서는 매일 하나의 논리적인 `jobInstance` 가 필요합니다.

예를 들어 1월 1일 실행, 1월 2일 실행, 등등. 1월 1일에 실행한 배치가 실패해서 다음 날 다시 실행하는 경우 1월 1일의 작업과 동일한 작업을 재실행해야합니다.

따라서 각 `JobInstance`는 실행 결과를 **여럿 가질 수** 있습니다.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Job Hierarchy</p></figcaption></figure>

#### 1.2 **JobParameters**

`JobInstance` 와 `job` 은 어떤 **차이점**을 가지고 있을지 의문이 생기실 수 있습니다.

정답은 `JobPatameters` 입니다.



`JobPatameters` 는 배치 job을 시작하는데 사용하는 파라미터 셋을 가지고 있는 객체입니다. 아래 그림처럼 실행 중인 job을 **식별**하거나 **참조 데이터**로도 사용할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Job Parameters</p></figcaption></figure>

앞에 나온 예시에서는 각 1월 1일, 1월 2일 총 두 개의 인스턴스가 있는데, `job`은 하나지만 `JobPatameter` 는 2개 입니다.

따라서 아래의  공식이 도출될 수 있습니다.

`JobInstance` = `job` + 식별용 `JobPatameter`



#### **1.3 JobExecution**

JobExecution의 개념은 `job`을 **한 번 실행하려 했다**는 것입니다.

하나의 실행은 성공하거나 실패하게 되는데, 실행에 상응하는 `JobInstance` 는 실행이 성공적으로 종료되기 전까지는 **완료되지 않은 것으로 간주**합니다.

만약 EndOfDay `job` 을 실행했지만 실패했다고 가정해보겠습니다. 첫 번째 실행과 똑같은 `job` 파라미터로 재실행한다면 새 `JobExecution` 이 생성됩니다.

반면 `JobInstance` 는 여전히 한 개입니다.



#### 1.4 정리

`Job`은 배치 처리의 **하나의 작업 단위**를 의미합니다. 무엇을,해야하는지 어떻게 실행되어야 하는지를.정의합니다.



`JobInstance`는 Job의 특정 실행을 나타내는 **순수한 구조적 객체**입니다. JobInstance는 주로 **올바른 재시작**을 위해 실행을 그룹화하는 역할을 합니다.

예를 들어,  같은 Job이 여러 번 실행될 때 각 실행은 별도의 `JobInstance`로 간주됩니다.



`JobExecution`는 실제 Job이 실행될 때 **필요한 정보를 관리**하는 객체입니다. JobExecution은 실행 상태, 시작 시간, 종료 시간 등 **다양한 속성을 관리하고 저장**합니다.&#x20;

Job이 실행될 때마다 새로운 JobExecution이 생성되어 해당 실행에 대한 모든 정보를 기록합니다.
