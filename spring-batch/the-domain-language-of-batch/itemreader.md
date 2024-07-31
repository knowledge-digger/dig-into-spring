---
description: 이 페이지에서는 Spring Batch의 ItemReader에 대해 설명합니다.
---

# ItemReader

## ItemReader

스프링 배치는 벌크 read와 write를 위한 세 가지 핵심 인터페이스인 `ItemReader`, `ItemProcessor`,`ItemWriter`를 제공합니다.

`ItemReader`는 `Step`에서 아이템을 한 번에 하나씩 읽어오는 작업을 추상화한 개념입니다. 더 이상 읽을 아이템이 없으면 `ItemReader`는 null을 리턴합니다.

간단한 개념이지만, 매우 다양한 입력으로부터 데이터를 읽는 수단입니다. 대부분의 예제는 아래 예시를 포함합니다.

* Flat File : 플랫 파일 아이템 reader는 일반적으로 필드가 고정된 위치에 있거나 특정한 특수문자(쉼표 같은)로 필드를 구분하는 파일을 읽습니다.
* XML : XML `ItemReaders`는 파밍, 매핑, 검증에 사용되는 기술과는 독립적으로 XML을 처리합니다.
* Database : 데이터베이스에 접근해 처리할 객체에 매핑되는 resultSet을 얻어옵니다. default SQL `ItemReader` 구현체는 `RowMapper`를 호출해서 오브젝트를 리턴하고, 재시작을 대비해 현재 로우(row)를 추적하고, 기본적인 통계를 저장하며, 뒤에서 설명할 개선된 트랜잭션을 제공합니다.
