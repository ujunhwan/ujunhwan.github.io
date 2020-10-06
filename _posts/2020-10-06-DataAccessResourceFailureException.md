## Error Message
Exception in thread "main" org.springframework.dao.DataAccessResourceFailureException: Error retrieving database metadata; nested exception is org.springframework.jdbc.support.MetaDataAccessException: Could not get Connection for extracting meta data; nested exception is org.springframework.jdbc.CannotGetJdbcConnectionException: Could not get JDBC Connection; nested exception is java.sql.SQLException: Cannot create PoolableConnectionFactory (No timezone mapping entry for 'UTC8')

문제의 url
```java
private String url = "jdbc:mysql://localhost:3306/dbname?useSSL=false&serverTimezone=UTC8";
```

에러 메시지를 보니, UTC8에 맵핑된 타임존이 없어서 발생하는 오류라고 생각했다.  

그래서 간단하게 UTC로 바꾸었다.

```java
private String url = "jdbc:mysql://localhost:3306/dbname?useSSL=false&serverTimezone=UTC";
```

_해결!_