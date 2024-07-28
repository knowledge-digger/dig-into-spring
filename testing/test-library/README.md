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

# 테스트 라이브러리(Test Library)

## **스프링 테스트 라이브러리** <a href="#spring-test-library" id="spring-test-library"></a>

스프링 부트를 사용하여 개발 시 `spring-boot-starter-test` 의존성을 통해 양한 테스트 라이브러리를  사용할 수 있습니다.

{% code title="build.gradle" %}
```gradle
dependencies {
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

```
{% endcode %}

`spring-boot-starter-test`에는 다음과 같은 테스트 관련 라이브러리들이 포함되어 있습니다:

* [Mockito (mockito-core)](mock-objects.md)
* JUnit 5 (JUnit Jupiter)
* AssertJ
* Hamcrest
* JSONassert
* JsonPath

{% hint style="info" %}
스프링 부트를 사용하지 않는다면 필요한 라이브러리는 의존성을 추가하면 사용할 수 있다.
{% endhint %}
