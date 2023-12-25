## BeanCreationException

### Problem
Entity에서 PK에 @Id를 설정하고, @GeneratedValue를 통해 자동으로 값을 부여하고자 할 때 발생
```bash
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.MappingException: Could not instantiate id generator [entity-name=jpabook.jpashop.Member]

Caused by: org.hibernate.HibernateException: Could not fetch the SequenceInformation from the database

Caused by: org.h2.jdbc.JdbcSQLSyntaxErrorException: Column "start_value" not found [42122-224]
```

<br/>

### Cause of Problem
strategy 속성을 생략하면 기본적으로 GenerationType.AUTO가 사용됨 <br/>
Hibernate가 데이터베이스에 맞는 적절한 전략을 자동으로 선택 <br/>
H2 DB는 IDENTITY, SEQUENCE, TABLE 전략을 모두 지원하므로 IDENTITY, SEQUENCE, TABLE 중 1개가 선택 <br/>
SEQUENCE없이 GenerationType.SEQUENCE가 실행되어 `Could not instantiate id generator` 오류가 발생한 것으로 추정

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
[What's the difference between javax.persistence.Id and org.springframework.data.annotation.Id?](https://stackoverflow.com/questions/39643960/whats-the-difference-between-javax-persistence-id-and-org-springframework-data) <br/>
[[JPA_Basic] 기본키 매핑](https://hj0216.tistory.com/625) <br/>
[[JPA] H2의 @GeneratedValue 문제](https://hyeonic.tistory.com/196)