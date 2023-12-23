## org.springframework.http.converter.HttpMessageNotWritableException: Could not write JSON: JsonObject

### Problem
JsonObject 데이터를 Response로 응답하고자 함

<br/>

### Cause of Problem
Spring이 내부적으로 사용하는 JSON 라이브러리인 Jackson이 Gson의 JsonObject를 처리할 수 없기 때문에 발생

<br/>

### Solution
- JSONObject 대신 Map 사용
    - 단, Key값의 중복이 허용되지 않으므로 유의

<br/>

* JackSon Library
 * 역직렬화: JSON 문자열 → Java 객체로 변환
 * 직렬화: Java 객체 → JSON 문자열로 변환
 * Spring 프레임워크에서는 기본적으로 Jackson을 사용하여 HTTP 요청 본문의 JSON 데이터를 Java 객체로 변환하거나, Java 객체를 HTTP 응답 본문의 JSON 데이터로 변환
```Json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```
```Java
public class Person {
  private String name;
  private int age;
  private String city;
}
```

<br/>

### 📚 참고 자료
