---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 스프링 테스트(Spring Test)

## **스프링 테스트의 기능과 이점** <a href="#feature-and-benefit" id="feature-and-benefit"></a>

1.  **의존성 주입(Dependency Injection, DI)**:

    * 코드가 컨테이너에 덜 의존적이도록 합니다.
    * POJO(Plain Old Java Objects)를 new 연산자를 사용해 인스턴스화하여 테스트할 수 있습니다.


2.  **모의 객체(Mock Objects)**:

    * 모의 객체를 사용해 코드를 독립적으로 테스트할 수 있습니다.
    * 외부 시스템이나 복잡한 객체의 의존성을 제거하여 테스트 환경을 단순화합니다.


3.  **아키텍처 권장사항 준수**:

    * Spring 아키텍처를 따르면 코드의 계층화와 컴포넌트화가 용이해집니다.
    * 이를 통해 더 깨끗하고 관리하기 쉬운 코드베이스를 유지할 수 있습니다.


4.  **서비스 계층 테스트**:

    * 인터페이스를 스터빙(stubbing)하거나 모킹(mocking)하여 서비스 계층을테스트할 수 있습니다.
    * 영속성 데이터를 접근하지 않고도 단위 테스트를 수행할 수 있습니다.


5.  **런타임 인프라스트럭처 설정 필요 없음**:

    * 추가적인 런타임 인프라를 설정할 필요가 없습니다.
    * 진정한 단위 테스트를 강조하여 개발 생산성을 높일 수 있습니다.



스프링 테스트는 컨테이너 의존성을 줄이고, 테스트 용이성과 유지 보수성을 높이며, 외부 의존성을 제거한 효율적인 테스트가 가능합니다.
