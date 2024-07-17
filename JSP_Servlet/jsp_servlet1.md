# JSP & Servlet 1일차

## 1일차

- 서블릿 생성 및 실행

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
    id="WebApp_ID" version="6.0">
    <display-file-list>webMiniBoardBasic</display-file-list>
    <welcome-file-list>
      <!-- 하단은 main page에 해당 -->
      <welcome-file>index.html</welcome-file>
      <welcome-file>index.jsp</welcome-file>
      <welcome-file>index.htm</welcome-file>
      <welcome-file>default.html</welcome-file>
      <welcome-file>default.jsp</welcome-file>
      <welcome-file>default.htm</welcome-file>
    </welcome-file-list>

    <servlet>
      <!-- 클래스 이름 -->
      <servlet-name>HelloWorld</servlet-name>
      <!-- 자바 파일의 풀네임 -->
      <servlet-class>stu.edu.servlet.HelloWorld</servlet-class>
      <!-- 매개변수 초기값 -->
      <init-param>
        <param-name>email</param-name>
        <param-value>tj@stu.com</param-value>
      </init-param>
    </servlet>

    <servlet-mapping>
    <!-- 클래스 이름 -->
      <servlet-name>HelloWorld</servlet-name>
      <!-- 주소 경로 (서블릿 자겅된 클래스 명과 다르게 작성 가능) -->
      <url-pattern>HelloWorld</url-pattern>
    </servlet-mapping>
  </web-app>
  ```

  ```java
  // java
  package stu.edu.servlet;

  import java.io.IOException;

  import jakarta.servlet.Servlet;
  import jakarta.servlet.ServletConfig;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.ServletRequest;
  import jakarta.servlet.ServletResponse;

  public class HelloWorld implements Servlet {
    ServletConfig config;

    @Override
    public void destroy() {
      System.out.println("destroy 호출");
    }

    @Override
    public ServletConfig getServletConfig() {
      System.out.println("getServletConfig 호출");
      return this.config;
    }

    @Override
    public String getServletInfo() {
      System.out.println("getServletInfo 호출");
      return "";
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
      System.out.println("init 호출");

      this.config = config;

      String emailStr = this.config.get
    }

    @Override
    public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
      System.out.println("service 호출");

  }
  ```

<br />

- 서블릿의 생명주기와 관련된 메서드

  - init()

    - 서블릿 컨테이너가 서블릿을 생성한 후 초기화 작업을 수행하기 위해 호출

    - 서블릿이 클라이언트의 요청을 처리하기 전에 준비할 작업이 있다면 init 메서드에 작성

    - ex) DB 연동, 프로퍼티 로딩 등 클라이언트 요청을 처리하는데 필요한 자원을 미리 준비

  <br />

  - service()

    - 클라이언트가 요청할 때마다 호출되는 메서드

    - 실질적으로 서비스 작업을 수행하는 메서드

    - service 메서드에 서블릿이 해야 할 일을 작성

    - ex) 비즈니스 로직 등

  <br />

  - destroy()

    - 서블릿 컨테이너가 종료되거나 웹 애플리케이션이 멈출 때, 또는 해당 서블릿을 비활성화시킬 때 호출됨

    - destroy 메서드에는 서비스 수행을 위해 확보했던 자원을 해제하거나 데이터를 저장하는 등의 마무리 작업 작성

    - ex) DB 객체 해제, 메모리 회수 등

  > Servlet 인터페이스에 정의된 5개의 메서드 중에서 서블릿의 생성과 실행, 소멸, 즉 생명주기와 관련된 메서드

<br />

- Servlet 인터페이스에 정의된 5개의 메서드 중 생명주기와 관련되지 않은 이외의 메서드

- getServletConfig()

  - 서블릿 설정 정보를 다루는 ServletConfig 객체를 반환

  - 반환하는 객체를 통해 서블릿 이름과 서블릿 초기 매개변숫값, 서블릿 환경정보를 얻을 수 있음

  <br />

- getServletInfo()

  - 서블릿을 작성한 사람에 대한 정보나 서블릿 버전, 권리 등을 담은 문자열을 반환함

<br />

- web.xml

  - 배치 기술서(Deployment Descriptor) 또는 약어로 DD 파일이라 부름

  - 웹 애플리케이션의 배치 정보를 담고 있는 파일로 서블릿 생성 시 DD 파일에 배치 정보를 등록해야 함

  - DD 파일에 배치 정보를 등록해야 클라이언트에서 해당 서블릿의 실행 요청이 가능함

  - DD 파일에 등록되지 않은 서블릿은 컨테이너(웹 컨테이너)가 찾을 수 없음
