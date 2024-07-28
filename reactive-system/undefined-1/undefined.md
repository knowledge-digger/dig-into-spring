# 리액티브 스트림즈의 구성 요소

리액티브 스트림즈는 java 9의 Flow API , 프로젝트 리액터(Project Reactor) 등의 여러 구현체를 통해서

사용됩니다. 모든 구현체를 관통하는 핵심 키워드는 동일하고 다음과 같습니다.

* Publisher :

&#x20;       데이터를 생성하고 발행하는 역할을 합니다.

&#x20;       "리액티브 프로그래밍"에서의 구성요소 **Publisher 와 동일한 개념이고** 아래와 같은&#x20;

&#x20;        인터페이스를 제공합니다.

&#x20;        _public interface Publisher {_&#x20;

&#x20;             _public void subscribe(Subscriber\<? super T> s);_&#x20;

&#x20;        _}_

&#x20;        _- subscribe :_ 주어진 Subscriber를 구독자로 등록.



* Subscriber :&#x20;

&#x20;      Publisher 와 마찬가지로 , 앞서 언급한 프로그래밍의 구성요소와 동일하게&#x20;

&#x20;      데이터를 수신하고 이를 처리하는 역할을 합니다. 주요 인터페이스는 아래와 같습니다.

&#x20;       _public interface Subscriber {_&#x20;

&#x20;           _public void onSubscribe(Subscription s);_&#x20;

&#x20;           _public void onNext(T t);_&#x20;

&#x20;           _public void onError(Throwable t);_&#x20;

&#x20;           _public void onComplete();_&#x20;

&#x20;       _}_

&#x20;     _- onSubscribe :_ Publisher가 Subscriber에게 구독이 시작되었음을 알리고 Subscription을 전달.

&#x20;     \- onNext : 새로운 데이터를 수신할 때 호출됨.

&#x20;     \- _onError :_ 데이터 전송 중 에러가 발생했을 때 호출됨.

&#x20;     \- _onComplete :_ 데이터 전송이 완료되었을 때 호출됨.



* Subscription :&#x20;

&#x20;      Subscriber 와  Publisher 간의 데이터 구독을 관리합니다. 주요 인터페이스는 아래와 같습니다.

&#x20;      _public interface Subscription {_&#x20;

&#x20;          _public void request(long n);_&#x20;

&#x20;          _public void cancel();_&#x20;

&#x20;      _}_

&#x20;     _- request :_ Subscriber 가 Publisher 에게 데이터의 개수를 요청.&#x20;

&#x20;       ( ex : request(1) 은 한 개의 데이터를 요청 )

&#x20;     \- cancel : 구독을 취소 하고 더 이상 데이터를 수신 하지 않음.



* Processor :&#x20;

&#x20;       Publisher 와 Subscriber 의 두 역할을 모두 수행합니다.&#x20;

&#x20;       이는 중간 단계에서 데이터 처리를 가능 하게 합니다.

&#x20;        _※ Processor 는 Publisher 와 Subscriber 의 인터페이스들을 모두 상속 받아 사용 할 수 있습니다._

&#x20;        _두 요소의 기능을 모두 사용 할 수 있지만  , 각 역할의 책임을 명확히 나눔으로써 시스템의_&#x20;

&#x20;        _모듈화와 유연성을 높입니다_

