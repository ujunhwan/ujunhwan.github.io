# DispatcherServlet 설정



`DispatcherServlet` 에 대한 설정은 web.xml에서 하고, `DispatcherServlet` 이 읽어들여야할 내용은 @Configuration에서 설정한다.

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = { "junhwan.webmvc.controller" })
public class WebMvcContextConfiguration extends WebMvcConfigurerAdapter {
    ...
}
``` 

`DispatcherServlet` 은 해당 파일 설정을 읽어들여서 내부적으로 스프링 컨테이너인 `ApplicationContext` 를 생성하게 된다. 

<br>

## @Configuration  

이 어노테이션으로 인해서, 자바 config 파일이다 라고 알려준다.

<br>

## @EnableWebMvc  

DispatcherServlet의 RequestMappingHandlerMapping, RequestMappingHandlerAdapter, ExceptionHandlerExceptionResolver, MessageConverter 등 Web에 필요한 Bean들을 대부분 자동으로 설정해준다.

java config가 아닌 xml으로 설정을 했다면, `<mvc:annotation-driven />` 이 해줄 것이다.

_기본 설정 이외의 설정_ 이 필요하다면 `WebMvcConfigurerAdapter` 를 상속받아 Java config class를 작성한 후, 필요한 메소드를 오버라이딩 하도록 한다.

<br>

## @ComponentScan

Spring MVC에서는 컨트롤러를 찾아야 한다. 이것을 찾기 위해서는 `ComponentScan` 이라고 하는 어노테이션이 사용이 된다.

`ComponentScan` 어노테이션을 이용하면 _Controller, Service, Repository, Component_ 어노테이션이 붙은 클래스를 찾아 스프링 컨테이너가 관리하게 된다.  

우리가 작성할 컨트롤러에는 URL Mapping 정보가 어노테이션으로 설정이 되어있을 것이다.
이러한 URL Mapping 정보는 `DispatcherServlet`이 관리하는 `RequestMapping` 객체들로 설정이 될 것이다. 이것을 위해, 

`DefaultAnnotationHandlerMapping` 과 `RequestMappingHandlerMapping` 구현체는 다른 핸드러 매핑보다 훨씬 더 정교한 작업을 수행한다. 

이 두 개의 구현체는 애노테이션을 사용해 매핑 관계를 찾는 매우 강력한 기능을 가지고 있다. 이들 구현체는 스프링 컨테이너 즉 애플리케이션 컨텍스트에 있는 요청 처리 빈에서 `RequestMapping` 어노테이션을 클래스나 메소드에서 찾아 `HandlerMapping` 객체를 생성하게 된다.

`HandlerMapping` 은 서버로 들어온 요청을 어느 핸들러로 전달할지 결정하는 역할을 수행한다.  

`DefaultAnnotationHandlerMapping` 은 `DispatcherServlet`이 기본으로 등록하는 기본 핸들러 맵핑 객체이고, `RequestMappingHandlerMapping`은 더 강력하고 유연하지만 사용하려면 _명시적으로_ 설정해야 한다.

<br>

## WebMvcConfigurerAdapter

`@EnableWebMvc` 를 이용하면 기본적인 설정이 모두 자동으로 되지만, 기본 설정 이외의 설정이 필요할 경우 `WebMvcConfigurerAdapter` 클래스를 상속 받은 후, 메서드를 오버라이딩 하여 구현한다. 

<br>

## Controller(Handler) 클래스 작성하기

@Contoller 어노테이션을 클래스 위에 붙인다.

요청이 들어왔을 때 어떤 url로 들어온 요청인지 알아내서 실제로 처리해야 되는 컨트롤러가 뭔지, 그 컨트롤러에서 구현하고 있는 메서드가 뭔지 알아내야 한다. 
따라서 맵핑을 위해 @RequestMapping 어노테이션을 클래스나 메서드에서 사용한다. 

<br>

## @RequestMapping

Http Method와 연결하는 방법  
```java
@RequestMapping("/users", method=RequestMethod.POST)
```
/users라는 url로 요청이 post방식이면 이거 수행하세요~ 라는 뜻  

4.3 이후로는 @GetMapping @PostMapping 등등도 사용 가능하다.

<details>
<summary>예시</summary>
<div markdown="1">

* Http 특정 헤더와 연결하는 방법  
```java
@RequestMapping(method = RequestMethod.GET, headers="content-type=application.json")
```

* Http 특정 parameter와 연결하는 방법  
```java
@RequestMapping(method = RequestMethod.GET, params="type=raw")
```

* Content-Type Header 와 연결하는 방법  
```java
@RequestMapping(method = RequestMethod.GET, consumes = "application/json")
```

* Accept Header 와 연결하는 방법  
```java
@RequestMapping(method = RequestMethod.GET, produces = "application/json")
```

</div>
</details>

<details>
<summary>WebMvcContextConfiguration.java</summary>
<div markdown="1">

```java

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = { "kr.or.connect.mvcexam.controller" })
// 패키지에서 찾을 곳을 설정
public class WebMvcContextConfiguration extends WebMvcConfigurerAdapter {
	@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/assets/**").addResourceLocations("classpath:/META-INF/resources/webjars/").setCachePeriod(31556926);
        registry.addResourceHandler("/css/**").addResourceLocations("/css/").setCachePeriod(31556926);
        registry.addResourceHandler("/img/**").addResourceLocations("/img/").setCachePeriod(31556926);
        registry.addResourceHandler("/js/**").addResourceLocations("/js/").setCachePeriod(31556926);
        // css 폴더 밑에 application 루트 디렉토리 아래에 있는 이러한 디렉토리를 만들어놓고
        // /css/** 으로 들어오는 요청은, /css/에서 찾는다.
        // 이 부분이 없다면, Controller가 가진 RequestMapping 에서 찾을 것이므로 필요하다
    }
 
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
      // default servlet handler를 사용하게 한다.
      configurer.enable();
    }
   
    @Override
    public void addViewControllers(final ViewControllerRegistry registry) {
    		System.out.println("addViewControllers가 호출됩니다. ");
        registry.addViewController("/").setViewName("main");
        registry.addViewController("asdf").setViewName("userForm");
    }
    
    @Bean
    public InternalResourceViewResolver getInternalResourceViewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        // /WEB-INF/view/**.jsp 를 실행한다.
        return resolver;
    }
```

</div>
</details>

<details>
<summary>web.xml</summary>
<div markdown="1">

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app>

  <display-name>Spring JavaConfig Sample</display-name>

  <servlet>
    <servlet-name>mvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextClass</param-name>
      <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
    </init-param>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>kr.or.connect.mvcexam.config.WebMvcContextConfiguration</param-value>
      <!-- DispatcherServlet이 java config파일을 이용하는 부분 -->
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>mvc</servlet-name>
    <url-pattern>/</url-pattern>
    <!-- 모든 요청을 DispatcherServlet이 받도록 함 -->
    <!-- 위에 있는 org.springframework.web.servlet.DispatcherServlet이 실행됨 -->
  </servlet-mapping>

</web-app>
```

</div>
</details>

__web.xml__ 으로 인해, `DispatcherServlet` 이 _FrontController_ 로 사용된다.