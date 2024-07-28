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

### **2. @Mock** <a href="#annotation-mock" id="annotation-mock"></a>

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

### **3. @Spy**



### **4. @injectMocks**
