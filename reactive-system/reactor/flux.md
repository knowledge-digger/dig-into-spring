# Flux

Flux 의 주요 메소드를 알아보겠습니다.

1. 생성 메소드

* just : 주어진 데이터로 Flux를 생성.
* range : 시작 값부터 지정된 개수만큼의 정수를 포함하는 Flux를 생성.
* fromIterable : Iterable를 Flux로 변환.
* interval : 지정된 기간마다 값을 방출하는 Flux를 생성.

2. 변환 메소드

* map : 각 요소를 매핑하여 새로운 Flux를 생성.
* flatMap : 각 요소를 비동기적으로 매핑하여 새로운 Flux를 생성.
* filter : 조건에 맞는 요소만 포함하는 Flux 생성.
* take : 처음 N개의 요소만 포함하는 새로운 Flux 생성.

3. 결합 메소드

* mergeWith : 다른 Publisher와 결합 하여 새로운 Flux 를 생성.
* concatWith : 다른 Publisher와 순차적으로 결합.
* zipWith : 두 Flux의 요소를 결합 하여 새로운 Flux를 생성.

4. 소비 메소드

* subscribe : Flux의 각 요소를 소비.
* collectList : Flux의 모든 요소를 List로 수집.
* blockLast : Flux의 마지막 요소를 동기적으로 반환.
