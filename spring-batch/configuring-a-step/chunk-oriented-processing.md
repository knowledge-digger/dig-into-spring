---
description: 이 페이지에서는 청크 지향 프로세싱에 대해 설명합니다
---

# Chunk-oriented Processing

스프링 배치 구현체의 대부분은 ‘청크 지향'으로 개발되었습니다.

**청크 지향 프로세싱**이란 한 번에 데이터를 하나씩 읽어와서 트랜잭션 경계 내에서 쓰여질 **청크**를 만드는 것입니다.

절차는 다음과 같습니다.

1. ItemReader가 item을 read()한다.
2. 데이터가 ItemProcessor에 넘겨져 합쳐진다.
3. 읽어온 item 수가 커밋 간격과 같아진다.
4. ItemWriter가 청크 전체를 write하며 트랜잭션이 커밋된다.

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
