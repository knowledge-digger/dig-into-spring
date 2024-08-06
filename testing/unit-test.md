# 단위 테스트(Unit Test)

## **단위 테스트(Unit Test) 란?** <a href="#what-is-unit-test" id="what-is-unit-test"></a>

단위 테스트는 하나의특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차를 말합니다.\
스프링에서는 단위 테스트를 위해  [**Mock Object**](test-library/mock-objects.md#mock-object)를 사용하여 코드를 독립적으로 테스트 할 수 있고\
Mock Object를 편하게 활용하기 위해 스프링에서 [**Mockito**](test-library/mock-objects.md#mockito)를 활용할 수 있습니다.



## **스프링 단위 테스트**

스프링에서는 **JUnit**과 **Mockito**를 활용하여 편리한 단위 테스트를 수행 할 수 있습니다.\
\
다음 예제는 JUnit5와 Mockito를 활용한 스프링 단위 테스트 입니다.\
우리는 장바구니에 클라이언트가 선택한 음식을 담는 코드 단위 테스트를 수행할 것 입니다.\
