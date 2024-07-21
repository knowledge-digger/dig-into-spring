---
description: 테스트 코드가 무엇인가, 왜 필요한가
---

# Testing

## **테스트(Test)란?** <a href="#what-is-test" id="what-is-test"></a>

테스트는 제품(소프트웨어) 또는서비스가 예상하는 대로 동작하는지 확인하는 작업을 말합니다.



## **테스트 코드(Test Code)란?** <a href="#what-is-test-code" id="what-is-test-code"></a>

테스트 코드는 소프트웨어의 기능과 동작을 테스트하기 위한 코드입니다.



## **테스트 코드의 필요성** <a href="#needs-for-test-code" id="needs-for-test-code"></a>

왜 우리는 테스트 코드를 작성하여야 하는가?\
제일중요하다고 생각하는 이유는 다음과 같습니다.

* 리펙토링이 편하다.
* 디버깅 시간이 줄여준다.
* 동작하는 문서 역할을 한다.

이외에도 많은 이점이 존재합니다. \
~~코드 품질 보장, 회기 테스트, 배포 안정성, 품질향상, 자동화, 협업 능력 향상 등등~~ \
\
\
아래 V 모델(V-Model)은 **시스템 개발 과정을 시각화 한 모델**로 범위에 따라 테스트를 구분합니다.

<img src="../.gitbook/assets/file.excalidraw.svg" alt="" class="gitbook-drawing">

우리는 스프링을  사용하여 서비스 개발 시 그림처럼 구분화한 각 테스트를 구현할 수 있습니다.

