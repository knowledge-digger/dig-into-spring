---
description: 이 페이지에서는 Spring Batch의 ItemProcessor에 대해 설명합니다.
---

# ItemProcessor

## ItemProcessor

`ItemProcessor`는 아이템을 처리하는 비즈니스 로직을 나타내는 추상화 개념입니다. `ItemReader`가 아이템을 하나 read하고 `ItemWriter`가 아이템 묶음을 write한다면, `ItemProcessor`는 데이터 변환이나 다른 비즈니스 처리를 담당합니다.

데이터를 처리하던 중 아이템이 유효하지 않다고 판단하면 null을 리턴하는데, 이 아이템은 write되면 안된다는 것을 의미합니다.
