---
description: 이 페이지에서는 DispatcherServlet 대해 설명합니다.
---

# DispatcherServlet

Spring Web MVC 는Front Controller Pattern 중심으로 설계되었습니다. Front Controller Pattern 이란 모든 요청을 가장 먼저 처리하는 Controller 를 하나 생성하는 것입니다. Front Controller Pattern 의 장점이라면 프로세스가 집중되어 공통으로 사용되는 코드의 중복을 방지 할 수 있습니다.

Spring Web MVC 에서 사용된 Front Controller Pattern 은 다음과 같습니다. HttpServlet 를 상속 받은 FrameworkSevlet 이 doGet, doPost, doDelete, doPut 등 HttpServlet 에서 제공하는 메서드들을 상속 받아 구현합니다. 이제 모든 HTTP 요청들이 FrameworkSevlet 에서 구현한 doGet, doPost, doDelete, doPut 메서드를 통해 processRequest 메서드 호출하게 되며, 최종적으로 DispatcherServlet이 요청을 수행하게됩니다.

DispatcherServlet 는 해당 요청을 처리할 수 있는 Handler를 찾고 지정한 응답 형식에 따라 HandlerAdapter 를 검색하여 요청을 처리하게 됩니다. Controller 에 해당 요청을 처리할 수 있는 API 가 등록된경우 RequestMappingHandlerAdapter 가 그 요청을 처리하며, 정적리소스를 사용한다면, ResourceHttpRequestHandler 가 요청을 처리합니다. (ResourceHttpRequestHandler 가 처리할 수 없는 요청이라면 404 상태를 반환합니다.)

DispatcherServlet Resolver 를 통해 다양한 기능을 처리할 수 있습니다.

1. Locale Resolver 를 통해 다국어 처리가 가능합니다.
2. Theme Resolver (Deprecated) 를 통해 view 에 css, image 등 다양한 정적 자원을 설정할 수 있지만, 6.0 부터는 지원되지 않습니다.
3. Multipart Resolver 를 통해 multipart 요청에 대한 처리가 가능합니다. 주로 이미지 업로드 기능에 많이 사용되며 주로 StandardServletMultipartResolver 와 CommonsMultipartResolver 를 사용합니다. 그러나 스프링 6.0 , Selvet 5.0 부터는 CommonsMultipartResolver 를 사용할 수 없습니다.
4. HandlerExceptionResolver 를 통해 요청 처리 중 발생하는 예외에 대한 처리를 할 수 있습니다.
5. View Resolver 를 통해 HandlerAdapter 이 반환한 ModelAndView 를 통해 view 를 찾고 render 를 실행합니다.

