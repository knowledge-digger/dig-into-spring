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

# 웹 클라이언트(WebClient)

Spring Reactor는 리액티브 프로그래밍 모델을 지원하는 라이브러리입니다. \
이를 사용하면 비동기 이벤트 기반의 애플리케이션을 쉽게 개발할 수 있습니다.

## **WebClient**

**Spring WebFlux**는 **Spring Reactor**를 기반으로 한 **리액티브 웹 프레임워크**이며, 그 중 **WebClient**는 **비동기 및 논블로킹 방식으로 HTTP 요청을 수행하기 위한 클라이언트**입니다.

### **WebClient의 주요 기능**

* **비동기 및 논블로킹:**\
  요청을 보내고 응답을 받을 때까지 기다리지 않고, 그 사이에 다른 작업을 수행할 수 있도록 설계되어 있습니다. 이는 애플리케이션의 성능을 높이고, 특히 네트워크 호출이나 I/O 작업이 많은 환경에서 효율적으로 작동합니다.
* **유연한 API:**\
  다양한 HTTP 메서드(GET, POST, PUT, DELETE 등)를 지원하며, 요청 헤더, URL 매개변수, 폼 데이터, 바디 등도 손쉽게 설정할 수 있습니다.
* **반응형 스트림 처리:**\
  WebClient는 Mono와 Flux를 사용하여 단일값(Mono) 또는 다중값(Flux)을 비동기적으로 처리합니다. 이는 Spring Reactor의 핵심 개념으로, 데이터 스트림을 표현하고 처리하는 데 사용됩니다.
* **디코딩 및 인코딩 지원:**\
  JSON, XML 등의 다양한 데이터 포맷을 인코딩하거나 디코딩할 수 있습니다. 이를 통해 API 응답을 쉽게 처리하고, 요청에 데이터를 쉽게 첨부할 수 있습니다.
* **에러 처리:**\
  WebClient는 다양한 HTTP 상태 코드나 네트워크 에러를 처리할 수 있는 메커니즘을 제공합니다. 이를 통해 안정적인 API 호출을 구현할 수 있습니다.

### blocking, non-blocking

#### blocking

블로킹 동작에서는 함수 호출이 작업이 완료될 때까지 현재 스레드를 차단합니다. 이 방식은 일반적으로 직관적이며, 코드가 순차적으로 실행됩니다.

* **동기적 처리:**\
  호출된 함수는 결과를 반환하거나 작업이 완료될 때까지 호출자 스레드를 점유합니다. 예를 들어, 파일을 읽거나 네트워크 요청을 보낸 경우, 데이터가 준비되기 전까지 스레드가 블로킹됩니다.
* **자원 점유:**\
  블로킹 작업은 해당 작업을 처리하는 동안 스레드를 점유하므로, 동시 작업의 수가 증가하면 그에 비례하여 많은 스레드가 필요할 수 있습니다. 이는 메모리 사용량 증가와 스레드 간 컨텍스트 스위칭 오버헤드로 이어질 수 있습니다.

```java
public void blockingOperation() {
    try {
        String result = new BufferedReader(new InputStreamReader(new URL("https://example.com").openStream())).readLine();
        System.out.println(result);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

위의 코드에서는 URL에서 데이터를 읽을 때까지 현재 스레드가 블로킹됩니다. 이 동안 스레드는 다른 작업을 수행하지 못합니다.

#### non-blocking

논블로킹 동작에서는 함수 호출이 즉시 반환되며, 작업이 완료될 때까지 스레드가 차단되지 않습니다. 작업의 완료 여부를 확인하거나 처리 결과를 받을 때는 콜백, Future, 리액티브 스트림 등을 사용합니다.

* **비동기적 처리:**\
  함수는 작업의 완료 여부와 관계없이 즉시 반환되며, 작업이 완료되었을 때 콜백 함수가 호출되거나, Future나 Promise 등의 비동기 객체를 통해 결과를 받을 수 있습니다.
* **효율적 자원 사용:**\
  논블로킹 방식은 스레드 수를 최소화하면서도 높은 동시성을 제공할 수 있습니다. 스레드는 I/O 작업을 기다리면서 블로킹되지 않기 때문에 다른 작업을 처리할 수 있습니다.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption><p>블로킹/논-블로킹, sync/async</p></figcaption></figure>

{% hint style="info" %}
**`@Async`** 애너테이션을 사용하면 해당 메서드를 비동기적으로 실행하며, 이를 위해 **별도의 스레드를 사용**합니다. **`@Async`**가 적용된 메서드는 호출 즉시 비동기적으로 실행되며, 호출한 스레드는 해당 메서드의 결과를 기다리지 않고 즉시 다음 작업을 진행합니다.
{% endhint %}

### **사용 예제**

#### **비동기 요청** <a href="#request-async" id="request-async"></a>

{% code title=" non-blocking Get 요청" %}
```java
WebClient client = WebClient.create("https://api.example.com");

Mono<String> response = client.get()
    .uri("/endpoint")
    .retrieve()
    .bodyToMono(String.class);

response.subscribe(System.out::println);
```
{% endcode %}

이 예제에서는 `WebClient`를 사용하여 특정 URL로 GET 요청을 보내고, 응답 본문을 문자열로 받아옵니다. `Mono<String>` 타입의 응답을 subscribe하여 실제 결과를 처리합니다.

{% hint style="info" %}
**subscribe는** 응답이 도착할 때 비동기적으로 호출될 콜백을 등록하는 메소드 입니다.
{% endhint %}

#### **동기 요청** <a href="#request-sync" id="request-sync"></a>

```java
WebClient client = WebClient.create("https://api.example.com");

String response = client.get()
    .uri("/endpoint")
    .retrieve()
    .bodyToMono(String.class)
    .block();  // 블로킹 방식으로 결과를 기다림

System.out.println(response);
```

`block()` 메서드는 Mono가 발행하는 단일 결과를 기다립니다. 이 메서드가 호출되면, HTTP 요청이 완료되고 응답이 반환될 때까지 현재 스레드는 블로킹됩니다. 결과적으로, `response` 변수는 HTTP 응답 본문을 포함하게 되며, 이후의 코드 실행은 응답이 도착한 후에만 계속됩니다.
