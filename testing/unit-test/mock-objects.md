---
description: 모의 객체란?
---

# 모의 객체(Mock Objects)

## **MockObject 란?**

Mock Object는 주로 단위 테스트(Unit Test)를 수행할 때 사용하는 가짜 객체를 의미합니다.\
Mock 객체는 실제 객체의 동작을 흉내 내며, 테스트가 특정 조건이나 동작을 검증할 수 있도록 도와줍니다.

<pre class="language-java" data-title="JUnit5와 Mockito를 사용" data-line-numbers data-full-width="false"><code class="lang-java"><strong>@SpringBootTest
</strong>public class GreetingControllerTest {

    @Mock
    private GreetingService greetingService;

    @InjectMocks
    private GreetingController greetingController;

    @Test
    public void testGreet() {
        // Mock behavior 설정
        when(greetingService.greet()).thenReturn("Hello, Mock!");

        // 컨트롤러 호출 및 검증
        String result = greetingController.greet();
        assertEquals("Hello, Mock!", result);
    }
}

</code></pre>

