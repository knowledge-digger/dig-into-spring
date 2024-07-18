---
description: Spring Security를 파헤쳐보다
---

# 🔒 Security

Spring Security란  무엇인가?\
짧게 서술하면 Spring에서 제공하는 애플리케이션의 <mark style="color:green;">**보안**</mark>을 담당하는 프레임워크이다.\
Spring 공식 문서에는 Security를 뭐라고 설명하는지 한번 알아보자.

> "Spring Security is a framework that provides <mark style="color:green;">**authentication**</mark>, <mark style="color:green;">**authorization**</mark>, and protection against <mark style="color:green;">**common attacks**</mark>."
>
> "Spring Security는 <mark style="color:green;">**인증(Authentication)**</mark>과 <mark style="color:green;">**권한 부여(authorization)**</mark> 및 <mark style="color:green;">**일반적인 공격**</mark>에 대한 보호를 제공하는 프레임워크이다."

그렇다면 인증과 권한 부여는 무엇인지 일반적인 공격에는 어떤 공격들이 있는지 차근차근 알아보겠다.

### <mark style="color:green;">**인증(Authentication)**</mark>

Security에서 말하는 인증은 특정 리소스에 접근하려는 사람의 신원을 확인하는 방법을 말한다.\
우리가 흔하게 접하는 아이디와 비밀번호를 입력하여 로그인하는 방법이 이에 해당다.

### <mark style="color:green;">**권한 부여(Authorization)**</mark>

'인가' 혹은 '허가'와 같은 의미로도 사용이 된다. 권한 부여는 특정 리소스에 접근하는 사람의 이용 권한을  허용할지 결정하는 것이다.\
커뮤니티 게시판을 예로 들자면, 내가 쓴 글을 수정하거나 삭제하는 게 가능하지만, 다른 사람의 글은 수정하거나 삭제하는 게 불가능한 것이 간단한 예라고 볼 수 있다.

