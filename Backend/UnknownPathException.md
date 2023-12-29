## UnknownPathException

### Problem
```java
public List<Order> findAll(OrderSearch orderSearch) {

    String jpql = "select o from Order o join o.member m";
    boolean isFirstCondition = true;

    if (StringUtils.hasText(orderSearch.getMemberName())) {
        if (isFirstCondition) {
            jpql += " where";
        } else {
            jpql += " and";
        }
        jpql += " o.name like :name";
    }

    TypedQuery<Order> query = em.createQuery(jpql, Order.class)
            .setMaxResults(1000);

    if (orderSearch.getOrderStatus() != null) {
        query = query.setParameter("status", orderSearch.getOrderStatus());
    }
    if (orderSearch.getMemberName() != null) {
        query = query.setParameter("name", orderSearch.getMemberName());
    }

    return query.getResultList();
}

```

```bash
here was an unexpected error (type=Internal Server Error, status=500).
org.hibernate.query.sqm.UnknownPathException: Could not resolve attribute 'name' of 'jpabook.jpashop.domain.Order' [select o from Order o join o.member m where o.name = :name]

```

<br/>



### Cause of Problem
Order Entity에서 name 속성을 찾을 수 없음

<br/>



### Solution
1. JPQL 구문 확인
2. Order 객체에 name 필드가 존재하는지 확인

```java
public List<Order> findAll(OrderSearch orderSearch) {

    String jpql = "select o from Order o join o.member m";
    boolean isFirstCondition = true;

    if (StringUtils.hasText(orderSearch.getMemberName())) {
        if (isFirstCondition) {
            jpql += " where";
        } else {
            jpql += " and";
        }
        jpql += " m.name like :name"; // 수정
    }

    TypedQuery<Order> query = em.createQuery(jpql, Order.class)
            .setMaxResults(1000);

    if (orderSearch.getOrderStatus() != null) {
        query = query.setParameter("status", orderSearch.getOrderStatus());
    }
    if (orderSearch.getMemberName() != null) {
        query = query.setParameter("name", orderSearch.getMemberName());
    }

    return query.getResultList();
}

```

<br/>



### 📚 참고 자료
[실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
