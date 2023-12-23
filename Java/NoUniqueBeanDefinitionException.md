## NoUniqueBeanDefinitionException

### Problem
@ComponentScan에 excludeFilters를 사용하여 @Configuration을 빈 등록에서 제외한 후, 스프링 컨테이너에 Bean을 등록할 때 발생

<br/>

### Cause of Problem
@ComponenetScan과 @Configuration에서 동일한 빈이 중복 등록됨
@Configuration 클래스에서 @Bean으로 빈을 등록하고 있기 때문에 @ComponentScan의 excludeFilters 설정이 @Configuation 클래스에는 적용되지 않음

<br/>

### Solution
* @Configuration 주석 처리
```java
@Configuration
@ComponentScan(
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class))
public class AutoAppConfig { ... }

// @Configuration
public class AppConfig {
    @Bean
    public MemberService memberService(){ ... }

}

```
@Configuration 클래스는 스프링이 해당 클래스를 설정 정보로 사용하고, 빈을 정의하는 곳으로 간주하여 그 안의 모든 @Bean 메소드를 통해 반환되는 객체를 스프링 컨테이너에 빈으로 등록
@Configuration이 없는 클래스 내부에서 @Bean을 사용할 경우, 단순한 자바 클래스로 인식되어 @Bean 메서드는 자동으로 빈 등록이 되지 않음
