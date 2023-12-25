## JdbcTypeRecommendationException

### Environment
- Language: Java
- DB: H2 Database
- IDE: IntelliJ

<br/>

### Problem
@Entity를 선언하고 table을 create할 때 발생

```bash
Caused by: org.hibernate.type.descriptor.java.spi.JdbcTypeRecommendationException: Could not determine recommended JdbcType for Java type 'jpabook.jpashop.domain.Delivery'
```

<br/>

### Cause of Problem
Hibernate가 엔티티의 필드에 대한 JDBC 타입을 결정하지 못할 때 발생 <br/>
엔티티 필드와 데이터베이스 컬럼 간의 매핑이 충분히 명시되지 않았거나 잘못되었을 때 발생 <br/>
관계 설정을 하지 않는다면, Hibernate는 해당 관계를 어떻게 매핑해야 하는지 알 수 없게 되어, Java의 데이터 타입을 SQL 데이터베이스의 데이터 타입에 매핑하지 못하고 JdbcTypeRecommendationException이 발생

<br/>

### Solution
* Delivery Entity가 사용된 Entity에서 @OneToMany 등의 관계 설정이 제대로 이뤄졌는지 확인

```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "member_id")
    private Long id;

    private String name;

    @Embedded
    private Address address;

    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();

    private Delivery delivery;

}

```

<br/>

### 📚 참고 자료
