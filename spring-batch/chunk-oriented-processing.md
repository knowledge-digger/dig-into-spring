---
description: 이 페이지에서는 청크 지향 프로세싱에 대해 설명합니다
---

# Chunk-oriented Processing

## Chunk-oriented Processing

스프링 배치 구현체의 대부분은 ‘청크 지향'으로 개발되었습니다.

**청크 지향 프로세싱**이란 한 번에 데이터를 하나씩 읽어와서 트랜잭션 경계 내에서 쓰여질 **청크**를 만드는 것입니다. \
\
`ItemReader`가 읽은 항목의 수가 커밋 간격과 같아지면 전체 청크가 `ItemWriter`에 의해 쓰여지고 트랜잭션이 커밋됩니다. \
\
다음 이미지는 프로세스를 보여줍니다.



<figure><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&#x26;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKulqs%2FbtqZoxFyRus%2FrFT6XpFpL2W8MvTi0baDUK%2Fimg.png" alt=""><figcaption><p><strong>프로세스</strong></p></figcaption></figure>

같은 개념을 코드로 나타내면 다음과 같습니다.&#x20;

```java
List items = new Arraylist();
for(int i = 0; i < commitInterval; i++){
    Object item = itemReader.read();
    if (item != null) {
        items.add(item);
    }
}
List processedItems = new Arraylist();
for(Object item: items){
    Object processedItem = itemProcessor.process(item);
    if (processedItem != null) {
        processedItems.add(processedItem);
    }
}
itemWriter.write(processedItems);
```



### Chunk-oriented Processing **장점 및 단점**

\
청크 지향 프로세싱의 장점은 데이터 처리의 **효율성**을 높이고, 트랜잭션 관리를 통해 데이터의 **일관성**을 유지할 수 있다는 점입니다.&#x20;

각 청크 단위로 데이터를 처리함으로써 **대량의 데이터**를 효과적으로 다룰 수 있으며, 트랜잭션 경계를 명확히 하여 실패 시 청크 단위로 롤백할 수 있어 복구가 용이합니다.&#x20;

그러나 각 청크마다 트랜잭션을 시작하고 종료하는 과정에서 **오버헤드**가 발생할 수 있으며, 이는 **시스템 성능**에 영향을 미칠 수 있습니다.

### **청크 기반 프로세싱 예시**

예를 들면, **청크 없이** 한 번에 1000개의 레코드를 처리하는 경우

* 트랜잭션 시작
* 1000개의 레코드 읽기, 변환, 저장
* 트랜잭션 커밋&#x20;

\
여기서는 트랜잭션을 한 번만 시작하고 종료합니다. 그러나 **청크 기반 프로세싱**을 사용해 100개의 레코드씩 청크 단위로 나누어 1000개의 레코드를 처리하는 경우\


* 첫 번째 청크 (100개의 레코드)
  * 트랜잭션 시작
  * 100개의 레코드 읽기, 변환, 저장
  * 트랜잭션 커밋
*   두 번째 청크 (100개의 레코드)

    * 트랜잭션 시작
    * 100개의 레코드 읽기, 변환, 저장
    * 트랜잭션 커밋



&#x20;... (이 과정을 반복)



*   마지막 청크 (100개의 레코드)

    * 트랜잭션 시작
    * 100개의 레코드 읽기, 변환, 저장
    * 트랜잭션 커밋&#x20;



여기서는 10번의 트랜잭션 시작과 커밋 작업이 발생합니다.

\
또한 데이터 처리 단계가 여러 개로 분리되므로, 각 단계 간의 데이터 전달 및 오류 처리 로직이 복잡해질 수 있는 단점도 존재합니다.
