## SemanticException

- 작성일: 2024-01-11
- 수정일: 

<br/>



### Problem

```java
public List<Order> findAll(OrderSearch orderSearch) {
 
    String jpql = "select o from Order o join Member m";
    boolean isFirstCondition = true;

    if (orderSearch.getOrderStatus() != null) {
        if (isFirstCondition) {
            jpql += " where";
            isFirstCondition = false;
        } else {
            jpql += " and";
        }
        jpql += " o.status = :status";
    }

}
```

```bash
[ERROR] 2024-01-10 20:37:10 [http-nio-8080-exec-3] [dispatcherServlet] - Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed: org.springframework.dao.InvalidDataAccessApiUsageException: org.hibernate.query.SemanticException: Entity join did not specify a join condition [SqmEntityJoin(todowork.todoshop.domain.Member(m))] (specify a join condition with 'on' or use 'cross join')] with root cause
org.hibernate.query.SemanticException: Entity join did not specify a join condition [SqmEntityJoin(todowork.todoshop.domain.Member(m))] (specify a join condition with 'on' or use 'cross join')

```

<br/>



### Cause of Problem
Join에 대한 조건을 지정하지 않음

<br/>



### Solution
JPQL에서는 조인 작업을 수행할 때, 항상 조인 조건을 지정해야 함  
Member 엔티티에 대한 Order 엔티티의 참조 필드를 명시

```java
public List<Order> findAll(OrderSearch orderSearch) {
 
    String jpql = "select o from Order o join o.member m";
    boolean isFirstCondition = true;

    if (orderSearch.getOrderStatus() != null) {
        if (isFirstCondition) {
            jpql += " where";
            isFirstCondition = false;
        } else {
            jpql += " and";
        }
        jpql += " o.status = :status";
    }

}
```

🚨 Order 엔티티와 Member 엔티티 간에 직접적인 관계가 없다면, 두 엔티티를 직접 조인하는 것은 불가능  
만일 두 엔티티를 연결하는 다른 엔티티가 있다면, 그 엔티티를 통해 간접적인 조인을 수행할 수 있지만 비효율적

```java
String jpql = "select o from Order o join OrderMember om on o.id = om.order.id join Member m on om.member.id = m.id";
```

<br/>



### 📚 참고 자료
[실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
