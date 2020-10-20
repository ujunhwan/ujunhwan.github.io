평소에는 `RowMapper` 를 아래와 같이 override 하여서 사용하였다.

```java
new RowMapper<Product>() {
	@Override
	public Product mapRow(ResultSet rs, int rowNum) throws SQLException {
		Product product = new Product(rs.getInt("displayInfoId"), rs.getString("placeName"),
				rs.getString("productContent"), rs.getString("productDescription"),
				rs.getInt("productId"), rs.getString("productImageUrl"), rs.getInt("categoryId"));

		return product;
	}
}
```

그런데, column들이 많아졌을 때, 이걸 일일히 다 타이핑해야되나 싶어졌다..  
그래서 `BeanPropertyRowMapper` 를 이용하여 간단하게 해보려고 시도하였다.

### Error
DB와의 연동 하는 과정에서, `org.springframework.beans.BeanInstantiationException` 오류가 발생하였다.

### 원인
원인은 쿼리문에 있었다.
```java
public final static String SELECT_PRODUCT_BY_CATEGORY = "SELECT d.id as displayInfoId, 
p.id as productId, p.description as productDescription, 
d.place_name as placeName, p.content as productContent\n" + 
    ...
    ...

```
이런식으로, 내가 보기 편하게 as를 이용하여 카멜표기법으로 바꾸었는데, 
`BeanPropertyRowMapper` 는 스네이크 표기법으로 표현된 컬럼명들을 자동으로 카멜표기법으로 바꾸어, 클래스에 맞게 맵핑 해주는 것이었다.

### 해결
따라서, 쿼리문에 적혀있는 as들을 다 지웠더니 잘 실행이 됐다.

_해결!_

