# 테스트 컨텍스트 (TestContext)

## **테스트 컨텍스트 프레임워크(TextContext Framework) 란?**

테스트에 필요한 [애플리케이션 컨텍스트](../core-technologies/ioc.md#applicationcontext)를 생성하고 관리를 제공하는 테스트 프레임워크입니다.



## **@DirtiesContext 란?**

스프링 테스트의 `@DirtiesContext`는 테스트 실행 후에 스프링 애플리케이션 컨텍스트가 더럽혀졌다고 표시하여, 다음 테스트에서 새로운 애플리케이션 컨텍스트를 생성하도록 합니다.

왜 필요한가? 보통의 이유는 다음과 같다:

* **테스트 간 간섭 방지**:
  * 하나의 테스트가 애플리케이션 컨텍스트를 변경하여 다른 테스트에 영향을 주는 것을 방지합니다.
* **신뢰성 있는 테스트 결과 보장**:
  * 각 테스트가 독립적으로 실행되도록 하여, 테스트 결과의 신뢰성을 높입니다.
* **상태 초기화**:
  * 테스트 실행 후 상태를 초기화하여, 깨끗한 환경에서 다른 테스트를 실행할 수 있도록 합니다.

`@DirtiesContext`는 특히 데이터베이스 상태를 변경하거나, 애플리케이션 컨텍스트의 빈을 변경하는 테스트를 실행할 때 유용합니다. 이를 통해 테스트 간의 의존성을 줄이고, 신뢰성 있는 테스트 환경을 유지할 수 있습니다.

### **사용 방법** <a href="#dirties-context-usage" id="dirties-context-usage"></a>

{% code title="1) 클래스 레벨에서 사용" %}
```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = AppConfig.class)
@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_CLASS)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testServiceMethod1() {
        // 테스트 로직
    }

    @Test
    @DirtiesContext // 메서드 레벨에서 사용
    public void testServiceMethod2() {
        // 테스트 로직
    }
}
```
{% endcode %}

클래스 레벨에 `@DirtiesContext`를 적용하면, 해당 클래스의 모든 테스트 메서드가 실행된 후 애플리케이션 컨텍스트가 더럽혀졌다고 간주됩니다.\


{% code title="2) 메서드 레벨에서 사용" %}
```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = AppConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    public void testServiceMethod1() {
        // 테스트 로직
    }

    @Test
    @DirtiesContext
    public void testServiceMethod2() {
        // 이 메서드 실행 후 컨텍스트가 더럽혀졌다고 표시
    }
}
```
{% endcode %}

특정 테스트 메서드가 실행된 후에 애플리케이션 컨텍스트가 더럽혀졌다고 표시합니다. 이는 해당 메서드가 컨텍스트를 변경할 가능성이 있을 때 사용됩니다.\


{% hint style="warning" %}
**@DirtiesContext**는 테스트 간의 독립성을 보장하지만 자원 소모가 큰 작업입니다. 이를 최소화하기 위해 사용 타당성이 충분할 경우에만 사용하며  테스트 설계를 신중히 하고, 필요에 따라 **컨텍스트 캐싱, 트랜잭션 관리**등의 성능 최적화를 위해 다양한 전략을 사용해야 합니다.
{% endhint %}



## **테스트 성능 최적화** <a href="#optimization" id="optimization"></a>

스프링 테스트 성능 최적화를 위해 다양한 전략을 사용할 수 있습니다.

### **컨텍스트 캐싱** <a href="#context-cashing" id="context-cashing"></a>

스프링 테스트 프레임워크는 컨텍스트를 캐싱하여 동일한 컨텍스트를 사용하는 테스트끼리 애플리케이션 컨텍스트를 공유합니다.

<img src="../.gitbook/assets/file.excalidraw (1).svg" alt="컨텍스트 캐싱" class="gitbook-drawing">

위 처럼 여러개의 테스트가 캐싱된한 개의 애플리케이션 컨텍스트를 공유하여  테스트를 수행합니다.\


{% hint style="warning" %}
같은 컨텍스트를 상속 받더라도 특정 객체가 Mocking 된 클래스의 경우 Bean 설정이 달라질 수 있음으로 스프링은 테스트가 서로 다른 설정이라고 판단합니다.
{% endhint %}



## **트랜잭션 관리** <a href="#management-transaction" id="management-transaction"></a>

테스트는 독립적으로 실행되어야하고 반복적으로 수행 할 수 있어야합니다.\
스프링에서는 데이터 베이스 초기화, 일관성 유지 등의 목적으로 **@Transactional**을 활용할 수 있습니다.

#### &#x20;**@Transactional** <a href="#transactional-annotation" id="transactional-annotation"></a>

스프링 테스트 컨텍스트 프레임워크에서 `@Transactional`은 AOP 기반이 아닌 방식으로 트랜잭션을 관리합니다. 테스트 클래스에서 `@Transactional`을 사용하면, 트랜잭션 매니저가 자동으로 빈으로 등록되어 있어야 하며, 트랜잭션은 `transactionManager`라는 이름의 빈을 사용합니다. 테스트 메서드에서 `@Transactional`을 사용하면 해당 트랜잭션은 강제로 롤백되도록 설정됩니다.



#### **@Rollback** <a href="#rollback-annotation" id="rollback-annotation"></a>

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = AppConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @Test
    @Transactional
    @Rollback(false) // 이 테스트 메서드는 트랜잭션을 커밋합니다.
    public void testCommit() {
        myService.performService();
        // 트랜잭션이 커밋됩니다.
    }
}
```

`@Rollback` 어노테이션을 사용하면 트랜잭션의 롤백 동작을 제어할 수 있습니다. 기본적으로 테스트 메서드의 트랜잭션은 강제로 롤백되지만, `@Rollback(false)`로 설정하면 테스트 메서드가 끝난 후 트랜잭션이 커밋됩니다.

#### &#x20;**@BeforeTransaction, @AfterTransaction** <a href="#before-after-transaction-annotaion" id="before-after-transaction-annotaion"></a>

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = AppConfig.class)
public class MyServiceTest {

    @Autowired
    private MyService myService;

    @BeforeTransaction
    public void beforeTransaction() {
        // 트랜잭션이 시작되기 전에 실행됩니다.
    }

    @AfterTransaction
    public void afterTransaction() {
        // 트랜잭션이 종료된 후에 실행됩니다.
    }

    @Test
    @Transactional
    public void testService() {
        myService.performService();
        // 테스트 메서드 로직
    }
}
```

`@BeforeTransaction` 및 `@AfterTransaction` 어노테이션은 트랜잭션이 시작되기 전과 종료된 후에 실행되는 메서드를 정의할 때 사용됩니다. 이를 통해 트랜잭션 전후에 필요한 작업을 수행할 수 있습니다.
