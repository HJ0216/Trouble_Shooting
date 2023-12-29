## InvalidDataAccessApiUsageException

### Environment
- Language: Java
- DB: H2 Database
- IDE: IntelliJ

<br/>

### Problem
주문 메서드를 수행할 때, 넘기는 parameter 값 중 하나인 id 값이 load되지 않아 발생

```bash
org.springframework.dao.InvalidDataAccessApiUsageException: id to load is required for loading; nested exception is java.lang.IllegalArgumentException: id to load is required for loading

Caused by: java.lang.IllegalArgumentException: id to load is required for loading

```

<br/>

### Cause of Problem


<br/>

### Solution
* @GeneratedValue에 strategy GenerationType.IDENTITY 속성 추가

```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
}

```

- strategy 유형
    - GenerationType.AUTO
        - 기본값
        - 데이터베이스에 맞는 적절한 전략을 자동으로 선택
    - GenerationType.IDENTITY
        - PK 생성을 DB에 위임(예: MySQL-AUTO_INCREMENT)
        - PK에 null값을 넘기면 DB에서 자동으로 생성해줌
    - GenerationType.SEQUENCE
        - Sequence 객체를 생성해서 생성한 Sequence 객체에서 값을 가져와 PK값에 세팅
        - 주로 Oracle, H2에서 지원
    - GenerationType.TABLE
        - 키 생성 전용 테이블을 하나 만들어서 데이터베이스 시퀀스를 사용하는 것처럼 하는 전략
        - 시퀀스를 지원하지 않을 때 사용

<br/>

cf. `@Id package 확인`

```java
import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    private Long id;
    private String username;
}

```

@Id의 package 경로가 jakarta인지 확인
- `jakarta.persistence.Id`
    - the annotation defined by JPA for all its implementations
    - JPA only applies for management of relational data
- org.springframework.data.annotation.Id
    - currently used by Spring to support mapping for other non relational persistence databases or frameworks
    - it is normally used when dealing with other spring-data projects such as spring-data-mongodb, spring-data-solr, etc

<br/>

### 📚 참고 자료
[id to load is required for loading; nested exception is java.lang.IllegalArgumentException](https://www.inflearn.com/questions/130507/id-to-load-is-required-for-loading-nested-exception-is-java-lang-illegalargumen) <br/>
[테스트 exception 관련 질문 입니다.](https://www.inflearn.com/questions/65714/%ED%85%8C%EC%8A%A4%ED%8A%B8-exception-%EA%B4%80%EB%A0%A8-%EC%A7%88%EB%AC%B8-%EC%9E%85%EB%8B%88%EB%8B%A4)