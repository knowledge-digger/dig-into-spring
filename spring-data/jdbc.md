# JDBC

JDBC가 없던 시절에는 각각의 데이터베이스마다 서버와 연결하는 방법이 달랐습니다.

이때는 개발자가 각각의 DB마다 커넥션 연결, SQL 전달, 결과 응답을 받는 방법들을 DB마다 새로 학습해야 했으며, 데이터베이스를 다른 종류의 데이터베이스로 변경하면 당연히 애플리케이션 서버에 개발된 데이터베이스 사용 코드도 함께 변경해야 했습니다.



하지만,

&#x20;JDBC가 다음 3가지 기능을 표준 인터페이스로 정의해서 제공한 이후로 위의 문제점은 사라졌습니다.

```
java.sql.Connection - 연결
java.sql.Statement - SQL을 담은 내용
java.sql.ResultSet - SQL 요청 응답
```

JDBC 동작 원리

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p><a href="https://steady-coding.tistory.com/564">https://steady-coding.tistory.com/564</a></p></figcaption></figure>



**1. JDBC 드라이버 로드:**

JDBC는 데이터베이스와의 연결을 위해 드라이버를 로드해야 합니다.&#x20;

이 드라이버는 특정 데이터베이스 벤더가 제공하는 JDBC 드라이버여야 합니다.

**2. 데이터베이스 연결:**&#x20;

DriverManager 클래스를 사용하여 데이터베이스와의 연결을 설정합니다.&#x20;

연결 설정에는 JDBC URL, 데이터베이스 사용자 이름, 암호 등이 필요합니다.

**3. Statement 생성:**&#x20;

연결이 설정되면 Statement 또는 PreparedStatement를 사용하여 SQL 쿼리를 실행할 수 있습니다. Statement는 정적인 쿼리에 사용되고, PreparedStatement는 동적인 쿼리에 사용됩니다.

**4.SQL문 전송:**

Statement 나 PreparedStatement의 executeUpdate(), executeQuery() 메서드를 사용하면 됩니다.

**5. 결과 받기:**&#x20;

쿼리 실행 결과로 ResultSet 객체가 생성됩니다.&#x20;

ResultSet을 통해 데이터베이스에서 반환된 결과를 처리하고, 필요에 따라 데이터를 추출하거나 조작할 수 있습니다.

**6. 연결 해제:**&#x20;

모든 작업이 완료되면 연결을 종료해야 합니다.&#x20;

이는 메모리 누수를 방지하고 데이터베이스 리소스를 효율적으로 관리하기 위함입니다.
