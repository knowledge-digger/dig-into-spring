---
description: TaskletStep에 대해 설명합니다.
---

# TaskletStep

`step`을 다루는 방법 중에는 Chunk-oriented Processing 외에 **taskletStep**을 사용하는 경우도 있습니다.



**Tasklet** 인터페이스에는 `execute`라는 하나의 메서드를 가졌습니다.

이 메서드는 `RepeatStatus.FINISHED`가 반환되거나 실패했다는 의미의 예외를 던질 때까지 TaskletStep에 의해 반복적으로 호출됩니다.

각 Tasklet 호출은 트랜잭션으로 래핑되어있습니다. Tasklet 구현체는 저장 프로시저 호출, 스크립트 실행, SQL 업데이트 문 등을 호출할 수 있습니다.



**TaskletStep**을 생성하려면빌더의 taskler 메소드에 Tasklet 인터페이스를 구현한 빈을 넘겨야 합니다. TaskletStep을 만들 때는 chunk 메서드를 사용하면 안 됩니다.&#x20;



아래 예제는 간단한 tasklet을 만드는 샘플 코드입니다.

```java
@Bean
public Step step1() {
    return this.stepBuilderFactory.get("step1")
    			.tasklet(myTasklet())
    			.build();
}
```



### Chunk-oriented Processing - TaskletStep 비교&#x20;

청크 기반 프로세싱과 TaskletStep은 Spring Batch에서 배치 작업을 수행하는 두 가지 주요 방법입니다. \
아래는 이 두 방식의 사용 시점, 장점, 단점을 비교한 내용입니다.

<table><thead><tr><th width="114">항목</th><th>Chunk-oriented Processing)</th><th>TaskletStep</th></tr></thead><tbody><tr><td><strong>사용시점</strong></td><td><p>- 대량의 데이터를 처리할 때 </p><p>- 데이터를 읽고 변환한 후 저장할 때 </p><p>- 작업이 여러 단계로 나뉠 때</p></td><td><p>- 단일 작업을 수행할 때 <br>- 스크립트 실행, 파일 삭제 등 간단한 작업을 할 때 </p><p>- 배치 작업의 전처리나 후처리를 할 때</p></td></tr><tr><td><strong>장점</strong></td><td><p>- 효율성: 대량 데이터를 청크 단위로 나누어 메모리 사용 최적화 </p><p>- 트랜잭션 관리: 각 청크가 하나의 트랜잭션 단위로 처리됨 </p><p>- 재시작 가능성: 실패한 청크부터 다시 시작 가능</p></td><td>- 간단함: Tasklet 인터페이스만 구현하면 됨 <br>- 유연성: 다양한 작업을 손쉽게 구현 가능 <br>- 반복 실행: 특정 조건이 만족될 때까지 반복 실행 가능</td></tr><tr><td><strong>단점</strong></td><td>- 복잡성: ItemReader, ItemProcessor, ItemWriter 구현 필요<br>- 오버헤드: 각 청크마다 트랜잭션 시작/종료 오버헤드 발생</td><td>- 재사용성 제한: 주로 단일 작업을 수행하므로 재사용성 제한 <br>- 복잡한 데이터 처리에는 부적합</td></tr></tbody></table>

