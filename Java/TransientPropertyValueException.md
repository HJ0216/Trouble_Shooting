## TransientPropertyValueException

### Problem
```java
@Transactional
public Long order(Long memberId, Long itemId, int count) {

    Member member = memberRepository.findOne(memberId);
    Item item = itemRepository.findOne(itemId);

    Delivery delivery = new Delivery();
    delivery.setAddress(member.getAddress());
    delivery.setStatus(DeliveryStatus.READY);

    OrderItem orderItem = OrderItem.createOrderItem(item, item.getPrice(), count);

    Order order = Order.createOrder(member, delivery, orderItem);

    orderRepository.save(order);

    return order.getId();
}

```

```bash
org.hibernate.TransientPropertyValueException: object references an unsaved transient instance - save the transient instance before flushing : jpabook.jpashop.domain.Order.delivery -> jpabook.jpashop.domain.Delivery

```

<br/>



### Cause of Problem
객체가 저장되기 전에 저장되지 않은(transient) 연관된 객체를 참조하려고 할 때 발생

```java
@OneToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "delivery_id")
private Delivery delivery;

```

Order 객체가 Delivery 객체를 참고하고 있음 <br/>
Delivery 객체가 아직 영속화되지 않은 상태에서 Order 객체를 저장하려고 했기 때문에 Hibernate 예외 발생

<br/>



### Solution
1. Delivery 객체를 먼저 저장 후, Order 저장

```java
@Transactional
public Long order(Long memberId, Long itemId, int count) {

    Member member = memberRepository.findOne(memberId);
    Item item = itemRepository.findOne(itemId);

    Delivery delivery = new Delivery();
    delivery.setAddress(member.getAddress());
    delivery.setStatus(DeliveryStatus.READY);

    em.persist(delivery); // 추가

    OrderItem orderItem = OrderItem.createOrderItem(item, item.getPrice(), count);

    Order order = Order.createOrder(member, delivery, orderItem);

    orderRepository.save(order);

    return order.getId();
}

```

2. CascadeType 사용

```java
@OneToOne(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
@JoinColumn(name = "delivery_id")
private Delivery delivery;

```

기본은 각각의 객체는 각각 persist 해야 함 <br/>
cascade 사용 시, 값만 세팅해놓으면 1번의 persist로 관련된 객체까지 한 번에 저장할 수 있음

<br/>



### 📚 참고 자료
[실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
