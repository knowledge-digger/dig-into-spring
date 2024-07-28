---
description: 이 페이지에서는Multipart Resolve에 대해 설명합니다.
---

# Multipart

MultipartResolver는 multipart 요청을 처리합니다.

Spring 6.0 이상 Sevlet 5.0 이상에서 SpringWebMVC에서 제공하는 MultipartResolver는 StandardServletMultipartResolver 하나뿐입니다.

StandardServletMultipartResolver은 DispatchServlet의 doDispatch 안에서 작동되며,&#x20;

Handler를 찾기 전 checkMultipart 메서드를 호출해 해당 요청이 Multipart 요청인지 판단하여 Multipart 요청을 StandardMultipartHttpServletRequest로 Wapping 합니다. StandardMultipartHttpServletRequest로 Wapping된 요청은 RequestMappingHandlerAdapter를 통해 호출된 Controller 메서드의 매개변수에 StandardMultipartFile을 인자로 전달해 주게 됩니다.

Multipart 설정이 필요한 경우 애플리케이션에서 MultipartConfigElement를 통해 저장할 디렉터리 경로와 파일 크기 등을 설정할 수 있습니다.
