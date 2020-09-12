# Servlet과 jsp 연동

* `servlet` 은 프로그램 로직이 수행되기에 유리하다. 
* jsp는 결과를 출력하기에 유리하다. html 문을 그냥 입력하면 되기 때문
* 따라서 프로그램 로직 수행은 servlet, 결과 출력은 jsp 에서 하는 것이 유리하다.

결국 `servlet`과 `jsp`의 장단점을 해결하기 위해서 `servlet` 에서 프로그램 로직이 수행되고, 그 결과를 jsp 에게 포워딩 하는 방법이 사용되게 되었다.  
이를 `servlet`과 `jsp`의 연동이라고 한다.  
<br>


# 예제
_LogicServlet.java_ 파일에서 랜덤한 숫자를 발생 시킨다음, `request` 객체를 통해 _result.jsp_ 로 보낸 뒤, _result.jsp_ 에서 출력을 담당한다.
 이 때, `jsp` 에서의 출력 방식을 두가지로 사용할 수 있는데, `스크립틀릿`과 `표현식`을 사용하는 방법과 `EL표기법`을 사용하는 방법 이다.
<details>
<summary>Source Code</summary>
<div markdown="1">

```java
// LogicServlet.java
package examples;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/logic")
public class LogicServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public LogicServlet() {
        super();
    }
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int v1 = (int)(Math.random() * 100) + 1;
		int v2 = (int)(Math.random() * 100) + 1;
		int result = v1 + v2;
		
		// jsp forwarding
		request.setAttribute("v1", v1);
		request.setAttribute("v2", v2);
		request.setAttribute("result", result);

		RequestDispatcher rd = request.getRequestDispatcher("/result.jsp");
		rd.forward(request, response);
	}

}
```

```jsp
<!-- result.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
스크립틀릿 + 표현식 <br>
<%
	int v1 = (int)request.getAttribute("v1");
	int v2 = (int)request.getAttribute("v2");
	int result = (int)request.getAttribute("result");
%>
<%=v1 %> + <%=v2 %> = <%=result %><br>
<br>

EL표기 <br>
${v1 } + ${v2 } = ${result } <br>

</body>
</html>
```

</div>
</details>

<details>
<summary>결과</summary>
<img src='http://drive.google.com/uc?export=view&id=1oiLw8m79r-pbi-M5VrWa6Xbgz2-pZnx-' /><br>
</details>