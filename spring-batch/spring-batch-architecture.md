# Spring Batch Architecture

스프링 배치는 확장성과 다양한 유형의 사용자를 고려해 설계했습니다.

다음 그림은 확장성과 편의성을 지원하기 위한 계층 구조를 보여줍니다.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Spring Batch Layered Architecture</p></figcaption></figure>

구조에는 크게 Application, Batch Core, Batch Infrastructure가 있습니다.

#### Application

Spring Batch를 사용하는 개발자가 작성한 모든 배치 작업과 커스텀 코드를 포함합니다.

#### Batch Core

Batch job을 실행하고 제어하는 데 필요한 핵심 런타임 클래스를 포함합니다. 여기에는`JobLauncher`, `Job`, 그리고 `Step` 이 포함됩니다.

#### Batch Infrastructure

애플리케이션과 코어 모두 공통 인프라 위에 구축됩니다.

공통 reader와 writer, 서비스(RetryTemplate같은)을 포함하는데, 어플리케이션 개발자도 사용하고 코어 프레임워크 자체에서 활용하기도 합니다.
