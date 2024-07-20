---
description: Spring Security란?
---

# 🔒 Security

짧게 서술하면 Spring에서 제공하는 애플리케이션의 <mark style="color:green;">**보안**</mark>을 담당하는 프레임워크입니다.\
Spring 공식 문서에는 Security를 어떻게 설명하는지 한번 알아겠습니다.

> "Spring Security is a framework that provides <mark style="color:green;">**authentication**</mark>, <mark style="color:green;">**authorization**</mark>, and protection against <mark style="color:green;">**common attacks**</mark>."
>
> "Spring Security는 <mark style="color:green;">**인증(Authentication)**</mark>과 <mark style="color:green;">**권한 부여(authorization)**</mark> 및 <mark style="color:green;">**일반적인 공격**</mark>에 대한 보호를 제공하는 프레임워크이다."

Spring 공식 문서에는 위와같이 설명하고 있습니다.\
그렇다면 여기서 말하는 인증과 권한 부여는 무엇인지 알아 보겠습니다.

* 인증(Authentication)\
  \- 특정 리소스에 접근하려는 사람의 신원을 확인하는 과정\
  \- 일반적인 방법으로 아이디와 비밀번호를 입력하는 방법이 있습니다.
* 권한 부여(Authorization)\
  \- 신원이 인증된 사용자가 특정 리소스에 접근 가능한지 허용하는 과정\
  \- 사용자의 역할에 따라 사용 가능한 기능을 나누기 위한 과정입니다.

그리고 이러한 인증과 권한부여는 filter를 통해 이루어집니다.\
filter는 DispatcherServlet 앞 단계에서 실행되는 과정이며 요청을 가로채어 보안관련 처리를 합니다.\
이미지를 통해 자세한 과정을 보겠습니다.\


<figure><img src=".gitbook/assets/security architecture.png" alt=""><figcaption></figcaption></figure>

1. 클라이언트에서 로그인 정보와 함께 요청(Http Request)을 합니다.
2. AuthenticationFilter를 통해 요청 정보에서 이름과 비밀번호를 추출하여 인증 토큰을 생성합니다.
3. 생성된 토큰을 AuthenticationManager를 통해 ProviderManager로 전달합니다.
4. ProviderManager가 관리하는 인증 제공자(AuthenticationProviders)를 통해 인증을 시도하고 성공 시 인증된 Authentication을 반환합니다.
5. DB에서 사용자 정보를 가져오는 UserDetailsService에 Authentication을 넘겨줍니다.
6. 넘겨받은 정보를 통해 사용자 정보(UserDetails) 객체를 생성합니다.
7. 생성된 사용자 정보를 인증 제공자에게 넘기고 사용자 정보를 비교합니다.
8. 인증 성공 시 Principal(주체), Credentials(자격 증명), Authorities(권한), Authenticated(인증 여부) 등의 정보를 담은 Authentication 객체를 반환합니다.
9. AuthenticationManager는 Authentication 객체를 AuthenticationFilter로 반환합니다.
10. SecurityContextHolder의 SecurityContext에 Authentication 객체를 저장합니다.

위와 같은 과정으로 처리되며 Filter는 여러가지 유형이 존재합니다.\
아래는 로그인 인증 관련 Filter의 예시입니다.

* UsernamePasswordAuthenticationFilter\
  \- 사용자의 아이디/비밀번호 기반 인증 요청을 처리하는 역할을 합니다.
* FormLoginAuthenticationFilter\
  \- 폼 기반 로그인 인증을 처리하는 필터입니다.\
  \- 사용자가 로그인 폼을 통해 인증하는 경우 이 필터가 처리를 담당합니다.
* OAuth2LoginAuthenticationFilter\
  \- OAuth2 기반 소셜 로그인 인증을 처리하는 필터입니다. (네이버, 카카오톡, 구글)
* JwtAuthenticationFilter\
  \- JWT(JSON Web Token) 기반 인증을 처리하는 필터입니다.\
  \- 클라이언트가 전송한 JWT 토큰을 검증하고, 유효한 토큰인 경우 사용자 정보를 SecurityContext에 저장합니다.
