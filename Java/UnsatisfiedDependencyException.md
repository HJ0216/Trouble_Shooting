## UnsatisfiedDependencyException

### Problem
```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="memberService" class="hello.corehj.member.MemberServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository" />
    </bean>
    <bean id="memberRepository" class="hello.corehj.member.MemoryMemberRepository" />
    <bean id="orderService" class="hello.corehj.order.OrderServiceImpl">
        <constructor-arg name="memberRepository" ref="memberRepository" />
        <constructor-arg name="discountPolicy" ref="discountPolicy" />
    </bean>
    <bean id="discountPolicy" class="hello.corehj.discount.RateDiscountPolicy" />
</beans>

```

```bash
Error creating bean with name 'memberService' defined in class path resource [appConfig.xml]: Unsatisfied dependency expressed through constructor parameter 0: Ambiguous argument values for parameter of type [hello.corehj.member.MemberRepository] - did you specify the correct bean references as arguments?

org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'memberService' defined in class path resource [appConfig.xml]: Unsatisfied dependency expressed through constructor parameter 0: Ambiguous argument values for parameter of type [hello.corehj.member.MemberRepository] - did you specify the correct bean references as arguments?

```

<br/>



### Cause of Problem
memberService 클래스의 생성자 매개변수인 MemberRepository의 객체 생성 후 주입이 이뤄지지 않음

<br/>



### Solution
bean 생성 시, 생성자 매개변수 이름(constructor-arg name)과 memberServiceImpl에서 생성자 매개변수 이름이 같아야 함 <br/>
Spring이 생성자 주입(Constructor Injection)을 수행할 때 어떤 매개변수에 어떤 값을 주입해야 할지를 결정을 도움 <br/>
XML 설정 파일의 'constructor-arg' 태그의 'name' 속성 값과 일치하면 Spring은 'memberRepository' 빈을 'repository' 매개변수에 주입

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="memberService" class="hello.corehj.member.MemberServiceImpl">
        <constructor-arg name="repository" ref="memberRepository" />
    </bean>
    <bean id="memberRepository" class="hello.corehj.member.MemoryMemberRepository" />

</beans>

```

```java
@Component
public class MemberServiceImpl implements MemberService{

    @Autowired
    public MemberServiceImpl(MemberRepository repository) {
        this.repository = repository;
    }
}

```

<br/>



### 📚 참고 자료
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)
