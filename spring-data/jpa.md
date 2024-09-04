# JPA

우리가 개발을 할 때는 객체지향을 통해서 개발을 합니다.&#x20;

보통 데이터 베이스는 관계형 DB를 사용합니다.&#x20;

즉, 우리는 객체를 관계형 DB에 저장을 하고 관리를 합니다.&#x20;

따라서 각각 지향하는 목적이 다르기 때문에 패러다임 불일치가 발생합니다

**하지만 개발자가 JPA를 사용하면,**&#x20;

**JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신합니다**

개발자가 Entity를 생성해 JPA에 보내면 JPA는 Entity를 분석하여

그에 맞는 SQL을 생성해 JDBC API를 사용하여 DB와 연결합니다.

이러한 과정으로 쿼리를 JPA가 만들어 주기 때문에&#x20;

Object와 RDB 간의 패러다임 불일치를 해결할 수 있습니다.

즉, 중심적인 개발이 아닌 객체 중심적인 개발이 가능해집니다.

{% hint style="info" %}
Entity란?

* DB Table을 Java Class로 구현한 것입니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption><p><a href="https://yunamom.tistory.com/274">https://yunamom.tistory.com/274</a></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption><p><a href="https://yunamom.tistory.com/274">https://yunamom.tistory.com/274</a></p></figcaption></figure>



JPA의 핵심 내용은 엔티티가 `영속성 컨텍스트`에 포함되어 있냐 아니냐로 갈립니다.\
JPA의 `엔티티 매니저`가 활성화된 상태로 트랜잭션(@Transactional) 안에서 DB에서 데이터를 가져오면 이 데이터는 영속성 컨텍스트가 유지된 상태입니다.



{% hint style="info" %}
### 영속성 컨텍스트란?

* 영속성 컨텍스트는 엔티티를 영구 정정하기 위한 공간입니다.
{% endhint %}

{% hint style="info" %}


### EntityManager 란?

* 엔티티 매니저는 엔티티를 영속성 컨텍스트에 등록하고 생명주기에 따라 관리를 합니다.
{% endhint %}



<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p><a href="https://velog.io/@sang_hyeok/JPA">https://velog.io/@sang_hyeok/JPA</a></p></figcaption></figure>



### 영속성 컨텍스트 특징

* 1차 캐시\
  영속성 컨텍스트는 내부에 1차 캐시를 가지고 있습니다.\
  엔티티를 조회할 때, 1차 캐시를 먼저 조회하고 1차 캐시에 있으면 반환하고, 없으면 DB를 조회해서 가져옵니다.
* 쓰기 지연\
  쿼리문을 실행 될 때 마다 보내는 것이 아니라 쿼리문을 모아 두었다가 트랜잭션이 끝나면 한번에 보냅니다.
* 변경 감지\
  트랜잭션이 일어나면 1차 캐시에 저장이 되어 있는 엔티티 값과 DB의 값을 비교해서 변경된 값이 있다면 데이터베이스에 자동으로 저장이 됩니다.
* 지연로딩\
  지연 로딩은 쿼리로 요청한 데이터를 애플리케이션으로 바로 로딩하는 것이 아니라 필요할 때 쿼리를 보내 데이터를 조회하는 것을 의미합니다.

