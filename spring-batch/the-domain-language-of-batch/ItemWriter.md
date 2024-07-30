---
description: 이 페이지에서는 Spring Batch의 ItemWriter에 대해 설명합니다.
---

# ItemWriter

`ItemWriter`는 `Step`에서 배치나 청크 단위로 아이템을 출력하는 작업을 추상화합니다.
보통 `ItemWriter`는 다음에 받을 입력이 무엇인지는 알지 못하며 현재 받은 아이템만 알고 있습니다.

`ItemWriter`는 `ItemReader`과 비슷하지만 하는 일은 정 반대입니다.
자원이 필요하고, 열리고 닫혀야하지만 `ItemWriter`는 읽는 게 아니라 쓴다는 점이 다릅니다. 데이터 베이스나 큐를 사용한다면 이러한 작동과정은 inserts, updates, or sends일 것 입니다.
