## circular references

### Problem
TimeTraceAop Class를 SpringConfig에 @Bean으로 등록 시 발생

<br/>

### Cause of Problem
TimeTraceAop를 @Bean으로 등록 시, 본인 클래스에도 TimeTraceAop를 적용함으로써 순환 참조 오류 발생

<br/>

### Solution
1. TimeTraceAop를 @Component를 이용해 Bean으로 등록
```java
@Aspect
@Component
public class TimeTraceAop {
    // 생략
}
```

2. SpringConfig 파일에서 @Bean을 아용해 Bean으로 등록 시,
    - TimeTraceAop 적용 대상에서 TimeTraceAop을 제외
    ```java
    @Aspect
    @Component
    public class TimeTraceAop {
        @Around("execution(* hello.hellospring..*(..)) && !target(hello.hellospring.SpringConfig)")
        public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
            // 생략
        }
    }
    ```
* @Component와 @Bean
    * 스프링의 IoC 컨테이너에서 관리하는 객체로 등록되며 전역적으로 의존성 주입에 활용될 수 있음
    * @Component
        * Class Level에 선언
        * Spring이 직접 인스턴스 생성하고 Bean으로 등록
            * Spring은 클래스의 인스턴스를 생성하고 의존성을 주입할 때, 순환 참조 문제를 방지하기 위한 메커니즘이 동작
        * 내부에서 직접 접근 가능한 클래스 사용 시 활용
    * @Bean
        * Method Level에 선언
            * Class Level에서 추가적인 @Configuration 선언 필요
        * Setter나 Builder 등을 통해 사용자가 프로퍼티를 변경해서 생성한 인스턴스를
        Spring Container에 Bean으로 등록
            * AOP 클래스를 @Bean으로 등록하면 Spring이 해당 빈을 생성하려고 하는데, 이때 AOP 프록시 객체가 필요
            * AOP 프록시 객체는 빈의 메서드 호출을 감싸기 위해 생성
            * AOP 빈 자체가 프록시 객체를 필요로 하는 경우, 프록시 객체 생성 시점에서 AOP 빈이 다시 필요로 하게 되면서 순환 참조가 발생
        * 외부 라이브러리가 제공하는 객체 사용 시 활용

* `AOP 관련 클래스는 @Bean으로 직접 등록하면 AOP 프록시 등이 개입되면서 순환 참조 문제가 발생할 가능성이 높아지므로 주로 @Component로 등록하고, 컴포넌트 스캔을 통해 Spring에게 관리되도록 하는 것이 일반적`



<br/>

### 📚 참고 자료
[AOP(TimeTraceAop)를 @Component 로 선언 vs SpringConfig에 @Bean으로 등록](https://www.inflearn.com/questions/48156/aop-timetraceaop-%EB%A5%BC-component-%EB%A1%9C-%EC%84%A0%EC%96%B8-vs-springconfig%EC%97%90-bean%EC%9C%BC%EB%A1%9C-%EB%93%B1%EB%A1%9D) <br/>
[[SPRING/ERROR] AOP 적용 시 순환 참조 에러](https://comibird.tistory.com/41) <br/>
[Bean과 Component 차이](https://medium.com/sjk5766/bean%EA%B3%BC-component-%EC%B0%A8%EC%9D%B4-96a8d0533bfd) <br/>
[@Component와 @Bean의 차이](https://xzio.tistory.com/1826)