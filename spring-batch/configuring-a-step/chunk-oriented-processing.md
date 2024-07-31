---
description: 이 페이지에서는 청크 지향 프로세싱에 대해 설명합니다
---

# Chunk-oriented Processing

스프링 배치 구현체의 대부분은 ‘청크 지향'으로 개발되었습니다.

**청크 지향 프로세싱**이란 한 번에 데이터를 하나씩 읽어와서 트랜잭션 경계 내에서 쓰여질 **청크**를 만드는 것입니다.

`ItemReader`가 읽은 항목의 수가 커밋 간격과 같아지면 전체 청크가 `ItemWriter`에 의해 쓰여지고 트랜잭션이 커밋됩니다.
다음 이미지는 프로세스를 보여줍니다.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Chunk-oriented Processing</p></figcaption></figure>

같은 개념을 코드로 나타내면 다음과 같습니다.

```java
List items = new Arraylist();
for(int i = 0; i < commitInterval; i++){
    Object item = itemReader.read();
    if (item != null) {
        items.add(item);
    }
}
itemWriter.write(items);
```


또한 선택사항으로 `ItemProcessor`에 전달하기 청크 지향 단계를 구성할 수 있습니다. 다음 이미지는 `ItemProcessor`가 단계에 등록될 때의 `ItemWriter` 프로세스를 보여줍니다.
절차는 다음과 같습니다.


1.  **ItemReader가 item을 read()한다.**

    ItemReader는 데이터를 하나씩 읽어옵니다. \
    데이터 소스는 파일, 데이터베이스, 큐 등 다양할 수 있습니다.
2.  **데이터가 ItemProcessor에 넘겨져 합쳐진다.**

    읽어온 데이터는 ItemProcessor에 의해 처리됩니다. \
    이 단계에서 데이터 변환, 필터링, 검증 등의 작업이 수행됩니다.
3.  **읽어온 item 수가 커밋 간격과 같아진다.**

    설정된 커밋 간격만큼 데이터가 쌓이면, 한 청크가 완성됩니다.&#x20;

    예를 들어, 커밋 간격이 10으로 설정된 경우, 10개의 아이템이 모일 때까지 데이터를 읽고 처리합니다.
4.  **ItemWriter가 청크 전체를 write하며 트랜잭션이 커밋된다.**

    완성된 청크는 ItemWriter에 의해 데이터베이스나 파일 등 최종 저장소에 기록됩니다. \
    이 과정은 트랜잭션 내에서 이루어지며, 모든 아이템이 성공적으로 기록되면 트랜잭션이 커밋됩니다.

![image](https://github.com/user-attachments/assets/a2829917-cf00-4b3b-8ac3-55a97d65a230)

같은 개념을 코드로 나타내면 다음과 같습니다.

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

### Chunk-oriented Processing 장점 및단점

청크 지향 프로세싱의 장점은 데이터 처리의 **효율성**을 높이고, 트랜잭션 관리를 통해 데이터의 **일관성**을 유지할 수 있다는 점입니다.&#x20;

각 청크 단위로 데이터를 처리함으로써 **대량의 데이터**를 효과적으로 다룰 수 있으며, 트랜잭션 경계를 명확히 하여 실패 시 청크 단위로 롤백할 수 있어 복구가 용이합니다.&#x20;



그러나 각 청크마다 트랜잭션을 시작하고 종료하는 과정에서 **오버헤드**가 발생할 수 있으며, 이는 **시스템 성능**에 영향을 미칠 수 있습니다.&#x20;

{% hint style="info" %}
#### 청크 기반 프로세싱 예시
예를 들면, **청크 없이** 한 번에 1000개의 레코드를 처리하는 경우

* 트랜잭션 시작
* 1000개의 레코드 읽기, 변환, 저장
* 트랜잭션 커밋

여기서는 트랜잭션을 한 번만 시작하고 종료합니다.


그러나 **청크 기반 프로세싱**을 사용해 100개의 레코드씩 청크 단위로 나누어 1000개의 레코드를 처리하는 경우

* 첫 번째 청크 (100개의 레코드)
  * 트랜잭션 시작
  * 100개의 레코드 읽기, 변환, 저장
  * 트랜잭션 커밋
* 두 번째 청크 (100개의 레코드)
  * 트랜잭션 시작
  * 100개의 레코드 읽기, 변환, 저장
  * 트랜잭션 커밋

... (이 과정을 반복)

* 마지막 청크 (100개의 레코드)
  * 트랜잭션 시작
  * 100개의 레코드 읽기, 변환, 저장
  * 트랜잭션 커밋

여기서는 10번의 트랜잭션 시작과 커밋 작업이 발생합니다.
{% endhint %}

또한 데이터 처리 단계가 여러 개로 분리되므로, 각 단계 간의 데이터 전달 및 오류 처리 로직이 복잡해질 수 있는 단점도 존재합니다.
