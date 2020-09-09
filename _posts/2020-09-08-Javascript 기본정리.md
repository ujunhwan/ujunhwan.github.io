# Javascript
## 변수
모든 언어는 어떤 값 또는 어떤 객체를 표현하는 __변수__ 가 있다.
자바스크립트에서도 어떤 변수를 사용하는가에 따라서,    `Scope` 라는 변수의 유효범위가 달라진다.  
<br>

## 연산자
```javascript
const name = "crong";
const result = name || "codesquad";
```
OR연산을 사용해서 name이 존재하면 name이 쓰이고, 없으면 ___codesquad___ 가 쓰인다. 왼쪽 만족시, 오른쪽 확인 X  
여기선 ___crong___ 출력
<br>
```javascript
const name;
const result = name || "codesquad";
```
name이 존재하지 않으므로, ___codesquad___ 출력  
<br>

## 비교연산자
js에서는 ==보다는 `===` 를 많이 사용한다. why? 
```javascript
0 == "0";
true

0 == 0;
true

0 === "0";
false

0 === 0;
true

null == undefined;
true
```
>==은 타입을 비교하지 않는다.

타입까지 비교하기 위해선 `===` 을 사용해야 한다!  
<br>

## js의 type
> 타입은 선언할때가 아니고, 실행타임에 결정된다. 함수안에서의 파라미터나 변수는 실행될 때 그 타입이 결정된다.
> 
간단한 기본타입은 ` typeof `를 이용해 판단할 수 있다.  
<br>

## 문자열 처리
```javascript
typeof "abc"; // string
typeof "A"; // string
typeof 'a'; // string. single quote도 사용가능.
```

문자열에 다양한 메서드가 존재.
```javascript
"ab:cd".split("."); // ["ab", "cd"];
"ab:cd".replace(":", "$"); // "ab$cd"
"  abcde  ".trim(); // "abcde"
```  
<br>

## js의 함수
```javascript
function printName(firstname) {
    return "name is " + firstname;
}
console.log(printName());
```
만약 파라미터인 `firstname` 을 입력하지 않는다면?  
__'name is undefined'__ 출력  
아무것도 입력하지 않으면, undefined 가 입력된다.  
<br>

## hoisting
함수 안에 있는 변수들을 모두 끌어올려서 선언한다고 하여 `hoisting` 이라고 한다.
```javascript
function printName(firstname) {
    console.log(inner);

    var inner = function() {
        return "inner value";
    }

}

printName();    
```
__undefined__ 출력.

```javascript
function printName(firstname) {

    var result = inner();
    console.log("name is " + result);

    function inner() {
        return "inner value";
    }

}

printName();
```
__name is inner value__ 출력. `javascript` 의 `parser`가 함수와 변수들을 먼저 선언한다. 이 때, 위에 작성된 코드의 경우 
```javascript
var inner = functino() {
    ...
}
```
으로 작성되어 있으므로, `parser`에 의해 ___inner___ 라는 변수만 먼저 선언 되었고, 아래에 작성된 코드의 경우
```javascript
function inner() {
    ...
}
```
으로 작성되어 있으므로, ___inner___ 함수가 먼저 선언이 되었으므로, 결과에서 차이가 난다.  
<br>

## arguments
함수가 실행되면 그 안에서 `arguments` 라는 특별한 지역변수가 존재한다. js 함수는 선언한 파라미터보다 더 많은 인자를 보낼 수도 있다. 이 때 넘어온 인자를 `arguments` 로 배열의 형태로 하나씩 접근할 수가 있다. `arguments` 는 배열타입은 아니다. 따라서 배열의 메서드를 사용할 수 없다.

```javascript
function a() {
    console.log(arguments);
}
a(1, 2, 3);
```
__{ '0': 1, '1': 2, '2': 3 }__ 출력.

```javascript
function a() {
    for(var i=0; i<arguments.length; i++) {
        console.log(arguments[i]);
    }
}
a(4, 2, 3);
```
__4 2 3__ 출력.  
함수의 선언에서 매개변수를 굳이 지정하지 않아도, `arguments` 를 이용하여 사용할 수 있다. 그러나, `arguments` 는 의도를 알기 힘드므로, 조심히 사용해야 한다.  
<br>

## arrow function
`ES2015` 에는 `arrow function` 이 추가됐다. 간단하게 함수를 선언할 수 있는 문법이다.
```javascript
function getName(name) {
    return "Yu " + name;
}
// getName 함수와 아래의 arrow 함수는 동일하다.
var getName = (name) => "Yu " + name;
```