---
description: 이 페이지에서는 Spring AOP(Aspect-Oriented Programming)에 대해 설명합니다.
---

# ☘️ Spring AOP (Aspect-Oriented Programming)

## Spring AOP란?

{% hint style="info" %}
**Spring AOP**는 스프링 프레임워크에서 제공하는 기능으로 "**관점지향 프로그래밍**" 을 의미합니다.

**AOP**는 객체 지향 프로그래밍을 보완하는 패러다임 기술로서 비즈니스 로직으로부터 **공통의 관심사를 분리**할 수 있습니다.
{% endhint %}

> **패러다임이란**\
> 통상적으로 어떤 한 시대 사람들의 견해나 사고를 지배하고 있는
>
> 이론적 틀이나 개념의 집합체를 의미합니다.

다음은 AOP개념의 존재 의의를 나타내는 그림입니다.&#x20;

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>cross-cutting concerns : 횡단  관심사</p></figcaption></figure>





### Cross-cutting Concerns, 횡단 관심사란?

AOP는 <mark style="color:blue;">횡단 관심사의 분리를 허용함으로써 모듈성을 증가</mark> 시키는 것이 목적인 프로그래밍 패러다임입니다.

횡단 관심사, 로깅이나  권한 체크,  성능 검사 등,  여러 비즈니스 로직의 **전반적으로 걸쳐있는 공통된 관심사**를 뜻합니다.

로깅이나 권한 체크 등의 로직은 **인프라 로직**으로도 불리우는데, 이 인프라로직은 보통 어플리케이션 전 영역에 걸쳐있는 경우가 많습니다.

이 횡단 관심사를 비즈니스 로직에서 분리 시키지 않는다면, 다음과 같은 불편한  점이 발생할 수 있습니다.

1. 메소드의 복잡도가 증가하여 비즈니스 코드 파악이 어려움
2. 중복코드로 유지보수가 어려움.

공식문서에서 AOP는 OOP와 함께 다음과 같이 표현하고 있습니다.

{% hint style="info" %}
**AOP**는 프로그램 구조에 대한 또 다른 사고 방식을 제공함으로써 객체 지향 프로그래밍(**OOP**)을 보완합니다.
{% endhint %}

### OOP(Object Oriented Programming), 객체 지향 프로그래밍이란?



