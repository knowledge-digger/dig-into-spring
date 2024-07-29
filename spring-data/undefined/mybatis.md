# MyBatis



자바에서 데이터를 다루는 기술 중 하나로,

&#x20;JDBC 작업을 간편하게 도와주는 DB 프로그래밍 프레임 워크입니다. (Persistence Framework)



### MyBatis  동작원리

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p><a href="https://velog.io/@jonghne/MyBatis-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC">https://velog.io/@jonghne/MyBatis-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC</a></p></figcaption></figure>

* 프로그램 시작 시 수행되는 프로세스
  * SqlSessionFactoryBuilder가 설정파일을 읽어와서 SqlSession을 생성하기위한 SqlSessionFactory를 생성합니다.
  * 이렇게 생성된 SqlSessionFactory는 Spring DI 컨테이너에 저장됩니다.
* 클라이언트 요청 시 수행
  * 특정 쿼리 수행요청이 들어옵니다
  * SqlSessionFactory가 SqlSession을 생성하고 애플리케이션에 반환합니다.
  * 애플리케이션이 매퍼 인터페이스 객체를 가져옵니다. (@Autowired)
  * 매퍼 인터페이스 객체가 SqlSession 메소드를 통해 XML 파일에 있는 SQL을 실행합니다.
