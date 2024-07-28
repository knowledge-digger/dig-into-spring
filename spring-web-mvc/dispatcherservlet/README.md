---
description: 이 페이지에서는 DispatcherServlet 대해 설명합니다.
---

# DispatcherServlet

Spring Web _MVC는 Front_ Controller Pattern 중심으로 설계되었습니다. Front Controller Pattern 이란 모든 요청을 가장 먼저 처리하는 _Controller를_ 하나 생성하는 것입니다. Front Controller _Pattern의_ 장점이라면 프로세스가 한  곳으로 집중되어 공통으로 사용되는 코드의 중복을 _방지할_ 수 있습니다.

Spring Web _MVC에서_ 사용된 Front Controller Pattern 은 다음과 같습니다. _HttpServlet를_ _상속받은_ FrameworkSevlet이라는 클래스가 doGet, doPost, doDelete, doPut 등 _HttpServlet에서_ 제공하는 메서드들을 _상속받아_ 구현합니다. 모든 HTTP 요청들이 _FrameworkSevlet에서_ 구현한 doGet, doPost, doDelete, doPut 메서드를 통해 공통  처리  로직이있는 processRequest 메서드 호출하게 되며, 최종적으로 DispatcherServlet이 요청을 _수행하게 됩니다._\


_FrameworkSevlet 클래스에 구현되어 있는 doGet, doPost (_processRequest  에서는 DispatcherServlet 클래스의 doService 메서드를 호출하고 있습니다.)

```java
@Override
protected final void doGet(HttpServletRequest request, HttpServletResponse response)
       throws ServletException, IOException {

    processRequest(request, response);
}
```

```java
@Override
protected final void doPost(HttpServletRequest request, HttpServletResponse response)
       throws ServletException, IOException {

    processRequest(request, response);
}
```



_DispatcherServlet는_ 해당 요청을 처리할 수 있는 Handler를 찾고 지정한 응답 형식에 따라 _HandlerAdapter를_ 검색하여 요청을 처리하게 됩니다. _Controller에_ 해당 요청을 처리할 수 있는 API 가 _등록된 경우_ RequestMappingHandlerAdapter 가 그 요청을 처리하며, _정적 리소스를_ 사용한다면, ResourceHttpRequestHandler 가 요청을 처리합니다. (ResourceHttpRequestHandler 가 처리할 수 없는 요청이라면 404 상태를 반환합니다.)

_DispatcherServlet의 내부 구현코드_

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    //...more code
    // 핸드러를 찾는다.
    mappedHandler = getHandler(processedRequest);
    //...more code
    // 핸들러 어뎁터를 찾는다.
    HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler()); 
    //...more code
    // 핸들러 어뎁터가 반환한 ModelAndView 를 통해 view 를 render 한다.
    mv = ha.handle(processedRequest, response, mappedHandler.getHandler()); 


```

