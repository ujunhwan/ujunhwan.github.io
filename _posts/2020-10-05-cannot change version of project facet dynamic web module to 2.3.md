## cannot change version of project facet dynamic web module to 2.3 error

<br>

__pom.xml__ 에서 `servlet` 버전을 바꿔도, __org.eclipse.wst.common.project.facet.core.xml__ 에서 jst.web의 버전을 변경하여도 에러가 뜬다면 __web.xml__ 에서 문제가 발생한 것이다.

__web.xml__ 이 다음과 같이 적혀져 있을 것이다.

```xml
<!DOCTYPE web-app PUBLIC
"-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
"http://java.sun.com/dtd/web-app_2_3.dtd" >
```

이것을

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1" />
```

다음과 같이 바꿔준다.

_해결!_