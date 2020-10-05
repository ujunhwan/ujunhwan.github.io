# Spring JDBC

## Spring JDBC 패키지

___org.springframework.jdbc.core___  
JdbcTemplate 및 관련 Helper 객체 제공

<br>

___org.springframework.jdbc.datasource___  
DataSource를 쉽게 접근하기 위한 유틸 클래스, 트랜젝션매니져 및 다양한 DataSource 구현을 제공

<br>

___org.springframework.jdbc.object___  
RDBMS 조회, 갱신, 저장등을 안전하고 재사용 가능한 객제 제공

<br>

___org.springframework.jdbc.support___  
jdbc.core 및 jdbc.object를 사용하는 JDBC 프레임워크를 지원

<br>

## JDBC Template select Example

* 열의 수 구하기

<details>
<summary>JDBC</summary>
<div markdown="1">

```java
public int row(Role role) {
    int ret = 0;
    
    try {
        Class.forName("com.mysql.cj.jdbc.Driver");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    
    String sql = "SELECT count(*) FROM t_actor";
    try (Connection conn = DriverManager.getConnection(dburl, dbUser, dbpasswd);
            PreparedStatement ps = conn.prepareStatement(sql)) {
        
        ret = ps.executeUpdate();

    } catch (Exception e) {
        e.printStackTrace();
    }
    return ret;
}
```

</div>
</details>

<details>
<summary>Spring JDBC</summary>
<div markdown="1">

```java
int rowCount = this.jdbcTemplate.queryForInt("select count(*) from t_actor");
```
</div>
</details>

`Spring JDBC` 를 이용하면 간단하게 한줄로 실행가능하다.

* 변수 바인딩 하기

```java
int countOfActorsNamedJoe = this.jdbcTemplate.queryForInt("select count(*) from t_actor where first_name = ?", "Joe"); 
```

* String으로 결과 받기

```java
String lastName = this.jdbcTemplate.queryForObject("select last_name from t_actor where id = ?", new Object[]{1212L}, String.class); 
```

* 한 행 조회하기

```java
Actor actor = this.jdbcTemplate.queryForObject(
  "select first_name, last_name from t_actor where id = ?",
  new Object[]{1212L},
  new RowMapper<Actor>() {
    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
      Actor actor = new Actor();
      actor.setFirstName(rs.getString("first_name"));
      actor.setLastName(rs.getString("last_name"));
      return actor;
    }
  });
```

* 여러 건 조회하기

```java
List<Actor> actors = this.jdbcTemplate.query(
  "select first_name, last_name from t_actor",
  new RowMapper<Actor>() {
    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
      Actor actor = new Actor();
      actor.setFirstName(rs.getString("first_name"));
      actor.setLastName(rs.getString("last_name"));
      return actor;
    }
  });
```



