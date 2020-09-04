# CSS Layout block 가운데로 배치하기  

<img src='http://drive.google.com/uc?export=view&id=1n8oIz2i-1r_ZonFqExbzLixvlzk4vnna' /><br>
<p>

## float 이용
111, 222 2개의 블럭들을 정중앙에 위치시키고 싶었다. 따라서 ``float: left;``을 이용해서 왼쪽으로 흐르게 한 뒤, `margin`을 이용하여 중앙에 위치시키려고 하였다. 하지만 이 방법은 굉장히 불편하고, 시간도 오래걸리는 방법이기 때문에 다른 방법을 찾아보았다.  
<br>

## margin 이용

```css
margin: 0 auto;
text-align: center;
```
이 방법은 글을 중앙으로 정렬하는 방법이므로, 현재 상황에선 적용되지 않는 방법이다.  
<br>
</p>

<p>

## inline 이용
``display: inline`` 을 하는 방법의 경우, 블락이 다음 그림과 같이 크기가 변하게 되므로 알맞지 않다.  
<br>
<img src='http://drive.google.com/uc?export=view&id=1n6fTEwT23QZkj9LdEuDT_NcuT-zvt-pN' /><br>

</p>
<p>

## inline-block 이용
``display: inline-block`` 을 적용하였더니, 정확하게 정중앙에 위치하였다.  
<br>
<img src='http://drive.google.com/uc?export=view&id=1n2AlHWudtqIuI9sY20UDkXozHFXZBSW8' /><br>

<p>

## 결론
`inline`의 경우 전후 줄바꿈 없이 한 줄에 다른 엘리먼트들과 나란히 배치가 된다. 하지만 `width`, `height`, `padding` 등의 속성이 반영되지 않으므로 블락이 크기가 변하는 현상이 발생하였던 것이다. `inline` 대신 `inline-block`을 사용하게 되면, `inline`에선 반영되지 않았던 `width`, `height`, `padding` 등의 속성들이 반영된다.  
<br>

</p>

## Code
___
### CSS
```css
section {
    text-align: center;
    overflow: hidden;
}
section ul {
    overflow: hidden;
}
section ul li {
    text-align: center;
    display: inline-block;
    position: relative;
    background-color:darkseagreen;
    width : 100px;
    height : 19px;
    padding : 3%;
    margin : 4px;
}
section ul li a {
    text-decoration: none;
    
}
```
### HTML
```html
<!DOCTYPE html>
<html>
    <body>
        <section>
        <ul>
            <li><a href="">111</a></li>
            <li><a href="">222</a></li>
        </ul>
        </section>
    </body>
</html>
```