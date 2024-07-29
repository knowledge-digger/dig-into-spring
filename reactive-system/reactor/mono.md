# Mono

Mono 의 주요 메소드입니다.

1. 생성 메소드

* just : 단일 요소로 Mono를 생성.
* empty : 빈 Mono를 생성.
* error : 에러를 방출하는 Mono를 생성.
* fromCallable : Callable을 실행 하여 그 결과를 포함하는 Mono를 생성.
* fromSupplier : Supplier를 실행 하여 그 결과를 포함하는 Mono를 생성.
* defer : Mono 생성을 지연.

2. 변환 메소드

* map : Mono의 값을 다른 값으로 매핑.
* flatMap : Mono의 값을 비동기적으로 매핑하여 새로운 Mono를 생성.
* flatMapMany : Mono의 값을 Flux로 변환.
* switchIfEmpty : Mono가 빈 경우 대체 Mono를 반환.

3. 결합 메소드

* then : 현재 Mono가 완료된 후 , 실행할 Mono를 반환.
* thenReturn : 현재 Mono가 완료된 후 , 지정된 값을 반환하는 Mono를 생성.
* zipWith : 두 Mono를 결합하여 튜플을 포함 하는 새로운 Mono를 생성.

4. 소비 메소드

* subscribe : Mono를 구독하여 실행 및 소비 , 혹은 에러를 처리.
* block : Mono가 완료될 때까지 대기하고 값을 반환.
* blockOptional : Mono가 완료될 때까지 대기하고 Optional로 값을 반환.
