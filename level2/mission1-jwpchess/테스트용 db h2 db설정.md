# 테스트용 db h2 db설정

### 프로덕션 DB와 테스트용 DB를 분리하는 이유?

**프로덕션 DB를 테스트용 DB와 동일하게 사용하는 경우 어떤 문제가 발생할까?**

실제 서비스를 제공하는 프로덕션 DB로 테스트 하는 경우, 데이터를 롤백하더라도 프로덕션 데이터에 영향을 끼칠 위험이 있다.

**FakeDao와 같이 Mock 객체를 사용하는 경우?**

테이블 관계가 복잡해지면 테스트하기 어려워진다.

실제 쿼리가 잘 동작하는지 파악하기 어렵다.

위과 같은 이유로 테스트용 인메모리 DB인 h2를 사용하면 좋다. 

### 테스트용 h2 DB 설정하는 방법

src/test/resources/application.properties 파일에 다음 내용을 추가한다.

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=false
```

이렇게 설정해두면 테스트 시에 test 하위 폴더의 [application.properties](http://application.properties)를 읽어서 설정파일을 읽어온다.