---
layout: post
title: 1학기정리_2주차_Servlet
description: >
    1학기 내용정리 스터디 _ 2주차 _ Servlet
excerpt_separator: <!--more-->
---

<!--more-->

## Web Application


> 로컬에서 독립적으로 수행되는 것이 아니라
> 반드시 웹 브러우저 상에서 HTTP 프로토콜 및 HTML 문서를 근간으로 수행되는 애플리케이션
> 인터넷이나 인트라넷을 통해 웹 브라우저에서 이용할 수 있는 소프트웨어

### 웹 브라우저
클라이언트. 사용자가 접속하는 곳  
사용자의 요청을 웹 서버로 전달

#### 웹 서버
HTTP 프로토콜 기반  
클라이언트 요청을 처리하는 곳  
HTML, 이미지, CSS, JavaScript를 웹 브라우저에 제공  


#### 웹 애플리케이션 서버 (WAS)
페이지의 로직 수행 및 데이터베이스 연동  
웹 서버 / 컨테이너
ex) Apache Tomcat

#### 데이터베이스
웹에서 발생한 데이터를 저장

| |ASP|JSP|PHP
|:---:|:---:|:---:|:---:|
|속도|중간|느림|빠름
|이식성|낮음|높음|중간
장점|COM 객체|객체지향  유지보수 용이|빠른 속도  가볍고 용이


## Servlet (Server Side Applet)
`html`코드가 들어간 `java` 코드


### Servlet 동작 방식
![study-servlet-01](/assets/study-servlet-01.png)

1. 클라이언트가 요청한 `HTTP Request`를 `Servlet Container`로 전송
2. Servlet Container에서 `HttpServletRequest`, `HttpServletResponse` 두 객체를 생성
3. `web.xml`에서 사용자가 요청한 URL을 분석해 각 서블릿을 찾음
4. 해당 서블릿에서 `service` 메소드를 호출한 후 `deGet()` / `doPost()`를 호출
5. `deGet()` / `doPost()`는 동적 페이지 생성 후 `HttpServletResponse`객에체 응답 전송
6. `HttpServletRequest`, `HttpServletResponse` 객체 소멸   




**HelloServlet.java**
``` java
// 사용자의 request와 Servlet를 매핑
@WebServlet("/hello.do")
public class HelloServlet extends HttpServlet {
  private static final long serialVersionUID = 1L;

  @Override // 초기화. 한 번만 일어남
  public void init() throws ServletException {
    super.init();
  }

  @Override
  public void service(ServletRequest request, HttpServletResponse response) throws ServletException{
    // do Service
  }

  @Override // Get 방식
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException {
    request.setCharacterEncoding("utf-8");
    String name = request.getParameter("name");

    response.setContentType("text/html; charset=utf-8");
    PrintWriter out = response.getWriter();

    // logic

    // html 코드!!!!!!
    out.println(
				"<!DOCTYPE html>\r\n"
				+ "<html>\r\n"
				+ "<head>\r\n"
				+ "<meta charset=\"uft-8\">\r\n"
				+ "<title>Insert title here</title>\r\n"
				+ "</head>\r\n"
				+ "<body>\r\n"
				+ "	<h1>Hello " + name + "</h1>\r\n"
				+ "</body>\r\n"
				+ "</html>");
    out.close();

    response.sendRedirect("result.html");

  }

  @Override // Post 방식
  protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException {
		doGet(request, response);
  }
}

```

**web.xml 파일**
``` xml
<servlet>
  <servlet-name>HelloServlet</servlet-name>
  <servlet-class>com.ssafy</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>HelloServlet</servlet-name>
  <url-pattern>/hello</url-pattern>
</servlet-mapping>

```

### Forward vs Include vs Redirect

  1. Forward    
    : `request`객체를 다른 servlet에 `전달`하고 다른 servlet이 응답
  2. Include  
    : `request`객체를 다른 servlet에 `전달`하고 원래의 servlet이 `최종 응답`
  3. Redirect
    : 요청에 따른 응답 `URL`을 클라이언트에게 전달하고 클라이언트는 다시 `요청`


## JSP (Java Server Page)
`java` 코드가 들어간 `html` 코드

* 기존 Servlet에서 html 코드 작성의 불편함
* 개발자가 작성한 JSP 파일은 `Servlet`파일로 변환됨

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%!
	String name;
	String id;
	String msg;
%>
<%
	name = (String)request.getAttribute("name");
	id = (String)request.getAttribute("id");
	msg = (String)request.getAttribute("msg");
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Result Page</h1>
	<h1 style="color:blue;"><%=msg %></h1>
	<div><p>등록하신 이름은 <%=name %>, id는 <%=id %> 입니다.</div><br/>
	<a href="index.html">Main Page</a>
</body>
</html>
```


## MVC


## MVVM
