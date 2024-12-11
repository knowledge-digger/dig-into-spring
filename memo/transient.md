# transient

## **transient**

**Java 기본 제공 키워드**로, **직렬화와 관련된 제어**를 위해 사용된다.

```java
private transient String password;
```

## **transient 특징**

1. Java의 **`Serializable`** 인터페이스를 구현한 객체의 필드
2. **Java 직렬화** (`ObjectOutputStream`) 시 해당 필드는 무시한다.
3. Jackson과 같은 JSON 직렬화 라이브러리에서는 기본적으로 무시되지만, Jackson을 커스터마이징하면 이를 포함하도록 설정할 수도 있다.
4. **역직렬화** 시점에는 무시된 필드가 기본값(`null`, `0`, `false` 등)으로 초기화된다.

## **@JsonIgnore와 차이**

<table data-full-width="false"><thead><tr><th>특징</th><th>transient</th><th>@JsonIgnore</th></tr></thead><tbody><tr><td><strong>적용 범위</strong></td><td>Java 직렬화 (<code>Serializable</code>)</td><td>JSON 직렬화/역직렬화</td></tr><tr><td><strong>직렬화 대상에서의 동작</strong></td><td>해당 필드 무시</td><td>해당 필드 무시</td></tr><tr><td><strong>역직렬화 대상에서의 동작</strong></td><td>기본값(<code>null</code>, <code>0</code>, <code>false</code>)으로 설정</td><td>JSON 데이터에 필드가 있어도 무시</td></tr><tr><td><strong>JSON 데이터 반영 여부</strong></td><td>Jackson 기본 설정에서는 무시됨</td><td>명시적으로 JSON에 포함되지 않음</td></tr><tr><td><strong>유연성</strong></td><td>Java 직렬화에 국한됨</td><td>Jackson 기반으로 JSON 직렬화만 제한</td></tr></tbody></table>
