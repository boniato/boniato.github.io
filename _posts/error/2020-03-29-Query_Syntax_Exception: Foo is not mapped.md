### "QuerySyntaxException: posts is not mapped…" ###
<img src="/assets/images/posts/error/2020-03-29-Query_Syntax_Exception.png" width="90%" height="90%" title="등록" alt="등록">

<br>
테스트 코드를 돌리니 위와 같은 에러가 났다.
<br><br>
구글링을 하니 JPQL은 대부분 대소문자 구분이 없지만, 이를 구분하는 경우 중 하나는 자바 객체명이라고 한다. 즉, 테이블과 매핑된 도메인의 클래스명과 같아야한다.
<br><br>
"JPQL mostly is case-insensitive. One of the things that is case-sensitive is Java entity names. Change your query to..."
<br><br>
그래서 posts -> **Posts** 로 바꾸니 해결되었다.

```java
@Query("SELECT p FROM Posts p ORDER BY p.id DESC")
```

<br>
참조) https://stackoverflow.com/questions/8230309/jpa-mapping-querysyntaxexception-foobar-is-not-mapped
<br>
