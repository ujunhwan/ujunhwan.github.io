## Error

org.springframework.dao.EmptyResultDataAccessException

<br>

### 문제의 코드

```java
public CommentImage selectCommentImage(Integer commentId) {
		Map<String, Integer> params = Collections.singletonMap("commentId", commentId);
		RowMapper<CommentImage> rowMapper = BeanPropertyRowMapper.newInstance(CommentImage.class);
        CommentImage commentImage = jdbc.queryForObject(SELECT_COMMENTS_IMAGE, params, rowMapper);
```  
<br>


### 원인

queryForObject 를 실행했을 때, DB에서 아무값도 리턴하지 않는 경우가 있는데 그때 오류가 발생하였다.  
<br>

### 해결 

try-catch 문을 이용하였더니 잘 실행되었다.  
<br>

```java
public CommentImage selectCommentImage(Integer commentId) {
		Map<String, Integer> params = Collections.singletonMap("commentId", commentId);
		RowMapper<CommentImage> rowMapper = BeanPropertyRowMapper.newInstance(CommentImage.class);
		try {
			CommentImage commentImage = jdbc.queryForObject(SELECT_COMMENTS_IMAGE, params, rowMapper);
			return commentImage;
		} catch(DataAccessException e) {
			return new CommentImage();
		}
}
```

<br>

_해결!_