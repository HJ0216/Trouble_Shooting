## cvc-elt.1.a: Cannot find the declaration of element 'project'

### Problem
STS4에서 Maven Project import 후, 발생

<br/>

### Cause of Problem
Maven Project의 Library가 제대로 설치되지 않음

<br/>

### Solution
1. project tag의 link값 수정
    - https://maven.apache.org/xsd/maven-4.0.0.xsd
        - https -> http 로 변경
2. Error 관련 창에서
    - Force download of 'https://maven.apache.org/xsd/maven-4.0.0.xsd' 클릭

<br/>

* Maven project에사 pom.xml에서 오류가 발생할 경우,
    * project tag
    * dependency tag

* 일반적으로
    * Run -> Run As -> Maven Clean
    * Project 우클릭 -> Maven -> Update Project
        * Force Update of Snapshots/Releases
        * Update Project configuration from pom.xml
        * Refresh workspace resources from local filesystem
        * Clean project
    * 상위 4가지 Option check 후, Maven Project 재 업데이트 진행

<br/>

### 📚 참고 자료
[[Spring] cvc-elt.1.a: Cannot find the declaration of element  'project'.](https://til-make-it.tistory.com/23) <br/>
[스프링 STS 메이븐 pom.xml 에러날 때](https://parkpurong.tistory.com/133)
