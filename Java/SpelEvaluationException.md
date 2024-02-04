## SpelEvaluationException

- 작성일: 2024-01-10
- 수정일: 

<br/>



### Problem
```html
<div class="form-group mx-sm-1 mb-2">
    <select th:field="*{orderStatus}" class="form-control">
        <option value="">주문상태</option>
        <option th:each=
                        "status : ${T(jpabook.jpashop.domain.OrderStatus).values()}"
                th:value="${status}"
                th:text="${status}">option
        </option>
    </select>
</div>
```
  
```bash
Caused by: org.springframework.expression.spel.SpelEvaluationException: EL1005E: Type cannot be found 'jpabook.jpashop.domain.OrderStatus'
```

<br/>



### Cause of Problem
`jpabook.jpashop.domain.OrderStatus` 타입을 찾을 수 없음

<br/>



### Solution
`OrderStatus`의 package 경로 확인

```html
<div class="form-group mx-sm-1 mb-2">
    <select th:field="*{orderStatus}" class="form-control">
        <option value="">주문상태</option>
        <option th:each=
                        "status : ${T(todowork.todoshop.domain.OrderStatus).values()}"
                th:value="${status}"
                th:text="${status}">option
        </option>
    </select>
</div>
```

<br/>



### 📚 참고 자료
[실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
[orderstatus 문제로 질문 드립니다.](https://www.inflearn.com/questions/702157/orderstatus-%EB%AC%B8%EC%A0%9C%EB%A1%9C-%EC%A7%88%EB%AC%B8-%EB%93%9C%EB%A6%BD%EB%8B%88%EB%8B%A4)
