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



{% hint style="info" %}
공통의 목적이 있는 데이터와 동작을 묶어 **하나의 객체로 정의**하는 프로그래밍 방식입니다.&#x20;

어플리케이션을 구성하는 요소들을 객체로 인식하고, 객체들을 유기적으로 연결하여 프로그래밍 하는 것을 말합니다.
{% endhint %}

#### OOP의 한계점

횡단관심사의 발생 : 로깅, 성능검사, 권한 체크 등과 같은 인프라 로직을 어플리케이션 전반적으로 적용하는 방식은 OOP의 방식으로 해결이 어렵습니다.&#x20;

즉, 어떤 프로그램 구조에 대해서 다른 생각의 방향을 제시함으로써 **AOP가 OOP를 보완**하는 것입니다.



### AOP의 용어

<table><thead><tr><th width="118">구분</th><th>내용</th></tr></thead><tbody><tr><td>Target</td><td>어떤 대상에 부가기능을 부여할 것인지 의미<br>즉, 부가기능이 부여되는 대상</td></tr><tr><td>Advice </td><td><p>어떤 부가기능을 부가적으로 부여할 것인가를 의미</p><p>실질적인 부가기능을 담은 구현체를 의미</p><p>BEFORE, AROUND, AFTER의 실행 위치를 지정할 수 있다.</p></td></tr><tr><td>Join Point</td><td><p>어디에 적용할 것인가 의미<br>관심사를 구현한 <strong>코드를 끼워 넣을 수 있는 프로그램의 이벤트</strong>를 의미.</p><p>Ex. Call Events, Execution Events, Initialization Events</p></td></tr><tr><td>Point Cut</td><td><p>실제 Advice가 적용될 지점을 의미<br>Join Point의 상세한 스펙을 정의한것이다.</p><p>관심사가 주 프로그램의 <strong>어디에 횡단될 것인지를 지정</strong>하는 문장이다.</p><p>Ex. Before call</p></td></tr></tbody></table>



#### Advice Annotation

Advice는 특정 어노테이션으로 횡단관심사의 실행 시점에 대해 5가지로 정의할 수 있습니다.

<table><thead><tr><th width="191">정의</th><th>실행시점</th></tr></thead><tbody><tr><td>Before</td><td>메서드 호출되기 전</td></tr><tr><td>AfterReturning</td><td>메서드 실행 후 성공적인 결과값 리턴시</td></tr><tr><td>AfterThrowing</td><td>실행시점에 Exception이 발생한 경우</td></tr><tr><td>After</td><td>Before, AfterReturning에 상관없이 메서드 종료 시</td></tr><tr><td>Around</td><td>메서드 실행 전 후로 실행 가능</td></tr></tbody></table>



<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>AOP의 적용 원리</p></figcaption></figure>





### AOP의 구현 방법

AOP의 적용 시점에 따라 세 가지로 구분 할 수 있습니다.

**AspectJ**는 컴파일 시와 클래스 로드 시 적용을 지원하고 있습니다.

**Spring AOP**에서는 프록시 패턴 방식을 지원하고 있습니다다.

> **AspectJ란?**\
> AspectJ 는 자바에서 완벽한 AOP 솔루션 제공을 목표로 하는 기술이다

#### 1. 컴파일

Ex. J.java -> J.class와 같이 파일이 컴파일 되는 시점에 AOP를 적용하는 것입니다.

#### 2. 클래스 로드시

컴파일 방식과 달리 클래스 파일이 컴파일 되는 시점이 아닌 클래스 로더가 메모리 상에 클래스 파일을 업로드 시키는 시점에 AOP를 적용할 수 있습니다.

#### 3. 프록시 패턴

Spring AOP에서 사용하는 방식입니다.

타겟 클래스를 부가기능을 제공하는 프록시로 감싸 실행하는 방식입니다.

해당 프록시 패턴은 Spring에서 IoC와 DI를 사용하기에 가능한 방식입니다.





### Spring AOP vs AspectJ



<table><thead><tr><th width="111">비고</th><th>Spring AOP</th><th>AspectJ</th></tr></thead><tbody><tr><td>목표</td><td>간단한 AOP기능 제공</td><td>완벽한 AOP기능 제공</td></tr><tr><td>Join Point</td><td>메서드 레벨만 지원</td><td>생성자, 필드, 메서드 등 다양하게 지원</td></tr><tr><td>Weaving</td><td>런타임 시에만 가능</td><td>complete-time, post-complete, load-time제공</td></tr><tr><td>대상</td><td>Spring Container가 관리하는 Bean에만 적용가능</td><td>모든 Java Object적용 가능</td></tr></tbody></table>



**레퍼런스**

[**https://amaran-th.github.io/Spring/\[Spring\]%20AOP/**](https://amaran-th.github.io/Spring/\[Spring]%20AOP/)\
[**https://naveen-metta.medium.com/a-deep-dive-into-aop-in-spring-unveiling-the-power-of-aspect-oriented-programming-c6619aeb2dc4**](https://naveen-metta.medium.com/a-deep-dive-into-aop-in-spring-unveiling-the-power-of-aspect-oriented-programming-c6619aeb2dc4)
