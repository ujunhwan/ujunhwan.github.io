# Spring MVC

<img src='http://drive.google.com/uc?export=view&id=1IoRiAATs0f8_nCVtrrBdJyGSs2OhUzDZ' /><br>

Database라고 되어있는 부분을 제외한 파란색 부분들이 Spring MVC가 제공해주는 부분이고
개발자가 만들어야 하는 부분은 __보라색 부분들__ 이다.

먼저 client가 요청을 보내면 `DispatcherServlet` 이라는 서블릿이 받는다.

`DispatcherServlet`은 요청을 처리할 컨트롤러와 메서드가 무엇인지 `HandlerMapping`에게 물어본다.

`HandlerMapping`혼자 요청을 처리할 컨트롤러와 메서드를 알아낼 순 없다.
따라서 개발자가 어떤 요청에 어떤 컨트롤러가 동작할 것인지 _xml_,_java_ 파일의 어노테이션을 이용해 설정한다.

Spring으로 만들어진 웹어플리케이션이 생성될 때, `HandlerMapping` 객체들이 생성되면서 관리하게 된다.

일련의 과정들을 거쳐, 알맞은 컨트롤러와 메서드를 이용해 `HandlerAdapter`에게 실행을 요청한다.

그 결과를 모델에 받아서 `DispatcherServlet`에게 전달한다. `DispatcherServlet`은 컨트롤러가 리턴한 view name을 알게된다.

컨트롤러가 리턴한 view name을 갖고 적절한 view resolver를 사용하여 view를 출력하게 된다.  

<br>

### 요청 처리를 위해 사용되는 컴포넌트

* DispatcherServlet
  * HandlerMapping
  * HandlerAdapter
  * MultipartResolver
  * LocaleResolver
  * ThemeResolver
  * HandlerExceptionResolver
  * RequestToViewNameTranslator
  * ViewResolver
  * FlashMapManager

<br>

## DispatcherServlet 

___프론트 컨트롤러___  

클라이언트의 _모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고_ 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여준다.  
`DispathcerServlet`은 여러 컴포넌트를 이용해 작업을 처리한다.

<br>  


## DispatcherServlet 내부 동작 흐름

<img src='http://drive.google.com/uc?export=view&id=1RRS6fVLVUBO4vVLNrUuzozLc3BLL5pyF' /><br>

### 요청 선처리 작업

<img src='http://drive.google.com/uc?export=view&id=1RU_Z0FLA6VKUvUr8xVPUkw5g7HmZ3zwM' /><br>


___org.springframework.web.servlet.LocaleResolver___

Spring MVC 는 지역화를 제공한다.

우리가 똑같은 사이트를 들어갔음에도 불구하고, 어떤 사람들은 영어, 어떤 사람들은 독일어 사이트가 보이게 처리할 수 있다.

브라우저가 보내는 헤더 정보로부터 각 사용자의 브라우저의 언어셋팅 정보등을 받아올 수 있다.

___RequestContextHolder에 요청 저장___

이 부분은 스레드 로컬 객체이다. 

요청을 받아서 응답할 때 까지, HttpServletRequest, HttpServletResponse 등을 스프링이 관리하는 객체안에서 사용할 수 있도록 해준다.

___FlashMap 복원___

redirect로 값을 전달할 때 사용된다.
URL이 너무 길어지고 복잡해질 때 FlashMap을 사용하면, redirect될 때 딱 한 번 값을 유지시킬 수 있게 해준다.
FlashMap은 session과 같은 장소에 저장한 뒤, redirect가 된 후 삭제한다. get방식을 사용할 경우, URL이 드러나 보안에 취약하다는 단점이 있어서 사용한다.

사용자가 파일업로드를 했을 때
파일정보를 읽어들이는 특수한 형태의 request 객체가 필요하다.

이 때, 멀티파트라는 요청이 들어오게 되면, multipart resolver가 멀티파트를 결정하게 된다. 핸들러를 결정하고 요청하는 것 까지 선처리작업.


### 선처리작업에 필요한 컴포넌트

<Br>

___org.springframework.web.servlet.LocaleResolver___  

지역 정보를 결정해주는 전략 오브젝트이다.
디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정해준다.

<br>

___org.springframework.web.servlet.FlashMapManager___

FlashMap객체를 조회(retrieve) & 저장을 위한 인터페이스
RedirectAttributes의 addFlashAttribute메소드를 이용해서 저장한다.
리다이렉트 후 조회를 하면 바로 정보는 삭제된다.

<br>

___org.springframework.web.context.request.RequestContextHolder___

일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있도록 한다.
해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.

<Br>

___org.springframework.web.multipart.MultipartResolver___

멀티파트 파일 업로드를 처리하는 전략

<br>

### 요청 전달

<img src='http://drive.google.com/uc?export=view&id=1RW-TzsvVoyejU2lSrutL3HDk8bnCkDfF' /><br>

<Br>

### 요청 처리시 사용된 컴포넌트

___org.springframework.web.servlet.ModelAndView___

`ModelAndView` 는 Controller의 처리 결과를 보여줄 view와 view에서 사용할 값을 전달하는 클래스이다.  
이전에는 servlet에서 모델을 얻고, 그 모델을 jsp로 넘길 때 request 같은 객체를 이용하였었다.  
반면 스프링에서는 종속되지 않게 하기 위해 `ModelAndView` 같은 객체를 제공하고 있다.  
<br>
 따라서 request에 값을 넣어서 사용하기 보다는 `ModelAndView`같은 컴포넌트를 이용하는 것이 바람직하다.


<br>

___org.springframework.web.servlet.RequestToViewNameTranslator___

컨트롤러에서 뷰 이름이나 뷰 오브젝트를 __제공해주지 않았을 경우__ URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는 전략 오브젝트이다. 디폴트는 DefaultRequestToViewNameTranslator이다.