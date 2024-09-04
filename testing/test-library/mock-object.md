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

# 모의 객체(Mock Objects)

## **Mock Object 란?** <a href="#mock-object" id="mock-object"></a>

모의 객체(Mock Object)는 주로 단위 테스트(Unit Test)를 수행할 때 사용하는 가짜 객체를 의미합니다.\
모의객체는 실제 객체의 동작을 흉내내며, 테스트가 특정 조건이나 동작을 검증할 수 있도록 도와줍니다.



## **Mockito 란?** <a href="#mockito" id="mockito"></a>

Mockito는 개발자가 동작을 직접적으로 제어할 수 있는 모의  객체를 지원하는 JAVA 오픈소스 테스트 프레임워크입니다. Mockito는의존성을 단절 시킴으로 단위 테스트를 쉽게 작성하는 것을 도와줍니다.

\
**스터빙(Stubbing) 란?**
--------------------

Mockito에 대해서 알아보기 이전에 스터빙에 대해서 얘기해보겠습니다.\
\
**Stubbing**은 Mock 객체의 특정 메소드 호출에 대해 미리 정의된 동작을 지정하는 과정입니다.\
이를 통해 테스트 중에 메소드가 호출될 때 원하는 값을 반환하거나 특정 행동을 수행하도록 할 수 있습니다. 스터빙을 사용하면 실제 객체의 메소드 호출을 모방하고 테스트 결과를 예측 가능하게 만들 수 있습니다.

```java
import static org.mockito.Mockito.*;

List<String> mockedList = mock(List.class);

// 스터빙
when(mockedList.get(0)).thenReturn("first element");
when(mockedList.get(1)).thenThrow(new RuntimeException());

System.out.println(mockedList.get(0)); // "first element"
System.out.println(mockedList.get(1)); // RuntimeException 발생
```

## **Mockito 사용 예제** <a href="#mockito-usage" id="mockito-usage"></a>

SpringBoot와 Gradle을 사용한 간단한 테스트를 통하여 Mockito 라이브러리 사용 방법을 확인해봅시다.

### **1. Mockito dependency 추가**

스프링 프레임워크 사용 시, 사용환경에 맞는 적절한 Mockito 라이브러리를 추가합니다.

**mockito-core:** Mockito의 기본 기능을 제공하는 라이브러리\
**mockito-junit-jupiter:** JUnit 5와의 통합을 제공하여 Mockito를 더 쉽게 사용할 수 있도록 하는 라이브러리

{% code title="build.gradle" %}
```gradle
dependencies {
    testImplementation 'org.mockito:mockito-core:5.4.0'
    testImplementation 'org.mockito:mockito-junit-jupiter:5.4.0'
}
```
{% endcode %}

{% hint style="info" %}
SpringBoot 2.2 버전 이상 사용 시, spring-boot-start-test 의존성을 통하여 자동으로 \
mockito-core, junit 라이브러를 추가해줍니다.
{% endhint %}

### **1. @Mock** <a href="#annotation-mock" id="annotation-mock"></a>

&#x20;Mock 객체를 생성하기 위해서 **@Mock** 어노테이션과 **mock()** 메서드를 사용할 수 있습니다.

{% code title="어노테이션 사용" %}
```java
@Mock
private UserInterface myInterface;
```
{% endcode %}

{% code title="메소드를 사용" %}
```java
MyService myService= mock(MyService.class);
```
{% endcode %}

### &#x20;2. **@Spy**

`@Spy` 어노테이션은 실제 객체를 스파이 객체로 감싸서, 실제 객체의 메소드 호출을 모니터링하고 스터빙할 수 있게 합니다.

```java
import org.mockito.Spy;

public class ExampleTest {
    @Spy
    private List<String> spyList = new ArrayList<>();

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
    }

    @Test
    public void testSpy() {
        spyList.add("first element");
        verify(spyList).add("first element");
        assertEquals("first element", spyList.get(0));
    }
}
```

### **3. @InjectMocks**

`@InjectMocks` 어노테이션은 목 객체를 테스트 대상 객체에 자동으로 주입합니다. 이는 주입 가능한 생성자, 세터, 필드를 통해 이루어집니다.

```java
import org.mockito.InjectMocks;
import org.mockito.Mock;

public class ExampleTest {
    @Mock
    private List<String> mockedList;

    @InjectMocks
    private SomeService someService;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
    }

    @Test
    public void testService() {
        when(mockedList.get(0)).thenReturn("first element");
        String result = someService.process();
        assertEquals("first element processed", result);
    }
}
```

### **Mockito의 메소드**

Mockito의 메소드는 스터빙, 목 객체 검증, 동작 확인 등을 위해 사용됩니다.

#### when

`when` 메소드는 특정 메소드 호출에 대한 스터빙을 설정합니다.

```java
when(mockedList.get(0)).thenReturn("first element");
```

#### thenReturn

`thenReturn` 메소드는 특정 메소드 호출 시 반환 값을 설정합니다.

```java
when(mockedList.get(0)).thenReturn("first element");
```

#### thenThrow

`thenThrow` 메소드는 특정 메소드 호출 시 예외를 던지도록 설정합니다.

```java
when(mockedList.get(1)).thenThrow(new RuntimeException());
```

#### verify

`verify` 메소드는 특정 메소드가 호출되었는지 여부를 검증합니다.

```java
verify(mockedList).get(0);
```

#### any

`any` 메소드는 메소드 호출 시 임의의 인자를 허용합니다.

```java
when(mockedList.get(anyInt())).thenReturn("element");
```

#### ArgumentCaptor

`ArgumentCaptor`는 메소드 호출 시 전달된 인자를 캡처하여 검증할 수 있게 합니다.

```java
ArgumentCaptor<String> captor = ArgumentCaptor.forClass(String.class);
verify(mockedList).add(captor.capture());
assertEquals("first element", captor.getValue());
```
