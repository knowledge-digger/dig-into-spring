---
description: 이 페이지에서는 HandlerExceptionResolver에 대해 설명합니다.
---

# Exception

HandlerExceptionResolver는 요청을 처리하는 중 발생한 예외를 처리합니다.&#x20;

클라이언트의 요청을 처리하는중 예외가 발생하면 DispatcherSevlet은 예외를 해결하기 위해 HandlerExceptionResolver Chain에게 책임을 위임합니다. HandlerExceptionResolver는 DispatcherServlet의 processHandlerException 내부에서 동작하며 예외를 처리할 수 있는 Resolver를 찾아 해당 예외를 처리하게 됩니다.

일반적으로 SpringWebMVC는 4가지의 ExceptionResolver를 제공합니다. (SimpleMappingExceptionResolver를 제외한 3가지의 ExceptionResolver은 기본적으로 HandlerExceptionResolver Chain에 등록되어 있습니다.)

1.  SimpleMappingExceptionResolver \
    SimpleMappingExceptionResolver을 Spring Bean으로 등록하여 요청 중 발생한 예외 종류별로 화면을 설정하여 사용자에게 원하는 화면을 보여줄 수 있습니다.\


    ```java
    @Bean
    SimpleMappingExceptionResolver simpleMappingExceptionResolver() {
        SimpleMappingExceptionResolver resolver = new SimpleMappingExceptionResolver();
        Properties properties = new Properties();
        // FileNotFoundException 발생시 노출될 화면 정의
        properties.setProperty(FileNotFoundException.class.getCanonicalName(), "fileError");
        // BadRequestException 발생시 노출될 화면 정의
        properties.setProperty(BadRequestException.class.getCanonicalName(), "badRequest");
        resolver.setExceptionMappings(properties);
        return resolver;
    }
    ```
2. DefaultHandlerExceptionResolver \
   표준 SrpingWebMVC에 정의되어 있는 예외를 해결하고 이를 해당 HTTP 상태 코드로 변환합니다. (404, 500 등..)
3.  ResponseStatusExceptionResolver \
    @ResponseStatus annotation 을 사용하여 클라이언트에게 정의된 상태 메시지를 보낼 수 있습니다.\


    ```java
    @GetMapping("/index")
    @ResponseStatus(HttpStatus.OK)
    public String index() {
        return "index";
    }
    ```
4.  ExceptionHandlerExceptionResolver\
    &#x20;@Controller 또는 @ControllerAdvice 클래스에서 @ExceptionHandler의 값으로 정의되어 있는 예외에 해당하는 메서드를 호출하여 예외를 해결합니다.\


    ```java
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<String> handleRuntimeException() {
        return ResponseEntity.status(500).body("RuntimeException handled");
    }
    ```
