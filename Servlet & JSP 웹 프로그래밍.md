# 02 Servlet 구현 및 실행

## Servlet객체의 생명주기

- init() -  클라이언트로부터 최초의 요청을 받을 때 실행

- service() - 클라이언트로부터 요청을 받을 때 실행

- destroy() - 웹 애플리케이션 서비스를 중지할 때

  

## Servlet과 CGI

- CGI 프로그램은 요청이 있을 때마다 독립적으로 처리
- Servlet은 클라이언트로부터 최초의 요청을 받을 때 init()을 통해 서블릿 객체를 생성하고 service()를 실행 이후로 받는 요청은 생성된 서블릿 객체를 이용하고 service()만 실행한다
  - Servlet은 한 번 생성된 객체를 재사용하는 방식이므로 CGI프로그램에 비해 처리속도, 메모리 사용 부분에서 효율적이다.



## WEB-INF 폴더 접근

- WEB-INF 폴더는 기본적으로 클라이언트에서 접근이 불가하기 때문에 서버 쪽에서 클라이언트가 접속할 수 있도록 따로 설정을 해주어야 한다.
- 설정 방법은 2가지 이다.

### 1. web.xml설정을 통한 접근

### 2. @WebServlet을 통한 접근

- 서블릿 3.0부터는 @WebServlet 어노테이션을 통해 서블리 접근이 가능하다. 톰캣 7버전 부터 서블릿 3.0버전을 지원한다.

  

- ```java
  import javax.servlet.*;
  import javax.servlet.annotation.WebServlet;	//어노테이션
  import javax.servlet.http.*;
  
  @WebServlet("/hello2")	//@WebServlet
  public class FirstServlet extends HttpServlet{
      //......
  }
  
  ```

### 3. web.xml 방식과 @WebServlet 방식의 장단점

#### web.xml 방식

- 여러 개의 서블릿을 태그로 등록하기 때문에 전체적인 관리가 쉽다.
- URL 값이 변경되어야 할  때 소스를 수정하지 않고 web.xml에서 쉽게 변경이 가능하다.

#### @WebServlet 방식

- 별도의 xml 설정 파일 없이 자바 소스에서 @WebServlet으로 쉽게 URL 패턴이 지정가능
- 하나의 자바 소스에 하나의 서블릿 매핑만 가능하므로 여러 개의 서블릿에 대한 정보를 일목요연하게 볼 수 없다. 따라서, web.xml방식에 비해 유지보수성이 떨어진다.
- URL이 변경되어야 할 때 소스를 수정하여 다시 컴파일 해야 한다.



## service()와 요청 방식에 따른 메소드 호출

- public void service(ServletRequest req, ServletResponse resp)

  - 이 메소드는 클라이언트로부터 서블릿 요청을 받을 때마다 실행되는 메소드로써  같은 객체 내의 아래의 protected void service() 메소드를 호출만 해준다.

- protected void service(HttpServletRequest req, HttpServletResponse resp)

  - public void service()메소드로 부터 호출 받고 서블릿 요청이 GET, POST, PUT, DELETE 요청 들 중 어느 요청인지에 따라 그에 따른 메소드를 호출하는 역할을 한다.

    | 접근자와 타입  | 메소드                                                     |
    | -------------- | ---------------------------------------------------------- |
    | protected void | doGet(HttpServletRequest req, HttpServletResponse resp)    |
    |                | doPost(HttpServletRequest req, HttpServletResponse resp)   |
    |                | doPut(HttpServletRequset req,HttpServletResponse resp)     |
    |                | doDelete(HttpServletRequest req, HttpServletResponse resp) |

    

- ```java
  import java.io.*;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  @WebServlet("/second")
  public class SecondServlet extends HttpServlet {
  
  	@Override
  	protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
  		System.out.println("SecondServlet!!");
  	}
  
  }
  ```

- 실행 순서

  1. init(Servletconfig) &rarr; GenericServlet
  2. service(ServletRequest req, ServletResponse resp) &rarr; HttpServlet
  3. service(HttpServletRequest req, HttpServletResponse resp) &rarr; HttpServlet
  4. doGet(HttpServletRequest req, HttpServletResponse resp) &rarr; SecondServlet



