# JSP & Servlet 5일차

## 5일차

- filter

  - 서블릿 실행 전후에 어떤 작업을 하고자 할 때 사용하는 기술

    - ex) 클라이언트가 보낸 데이터의 암호 해제, 서블릿 실행 전 필요한 자원을 미리 준비, 서블릿 실행마다 로그를 남김 => 필터를 통해 처리 가능

  - 기본적으로 특정 요청에만 동작하며 여러 개의 필터가 정해진 순서에 따라 배치될 수 있는데 클라이언트 요청 처리 이전에 먼저 실행됨

  - 기존 코드의 변경 없이 애플리케이션에서 공통으로 사용할 수 있는 기능 구현에 널리 사용됨

  - 대표적으로 활용되는 분야: 인증, 로깅/검사, 한글 인코딩 처리 등

  <br />

  - doFilter()

  ```xml
  <!-- xml  -->
  <?xml version="1.0" encoding="UTF-8"?>

  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
    id="WebApp_ID" version="6.0">

    <display-name>webMiniBoardBasic</display-name>

    <context-param>
      <param-name>driver</param-name>
      <param-value>oracle.jdbc.driver.OracleDriver</param-value>
    </context-param>

    <context-param>
      <param-name>url</param-name>
      <param-value>jdbc:oracle:thin:@localhost:1521:xe</param-value>
    </context-param>

    <context-param>
      <param-name>user</param-name>
      <param-value>edu</param-value>
    </context-param>

    <context-param>
      <param-name>password</param-name>
      <param-value>edu12</param-value>
    </context-param>

    <filter>
      <filter-name>CharacterEncodingFilter</filter-name>
      <filter-class>spms/filters/CharacterEncodingFilter</filter-class>
      <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
      </init-param>
    </filter>

    <filter-mapping>
      <filter-name>CharacterEncodingFilter</filter-name>
      <!-- url 패턴(정규표현식)으로 filter 적용 -->
      <!-- 전체 적용 -->
      <url-pattern>/*</url-pattern>
    </filter-mapping>

    <welcome-file-list>
      <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

  </web-app>
  ```

  ```java
  // Filter class
  package spms.filters;

  import java.io.IOException;

  import jakarta.servlet.Filter;
  import jakarta.servlet.FilterChain;
  import jakarta.servlet.FilterConfig;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.ServletRequest;
  import jakarta.servlet.ServletResponse;

  public class CharacterEncodingFilter implements Filter {

    FilterConfig filterConfig = null;

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
      // TODO Auto-generated method stub
      this.filterConfig = filterConfig;
    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain fc)
        throws IOException, ServletException {
      // TODO Auto-generated method stub
      String encoding = this.filterConfig.getInitParameter("encoding");

      req.setCharacterEncoding(encoding);

      fc.doFilter(req, res);

    }

    @Override
    public void destroy() {
      // TODO Auto-generated method stub
      System.out.println("문자열 인코딩 destroy() 수행");

    }
  }
  ```
