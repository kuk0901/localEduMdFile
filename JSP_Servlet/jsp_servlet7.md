# JSP & Servlet 7일차

## 7일차

- redirect와 forward, include 차이

  - 서블릿끼리 작업을 위임하는 방법에 대해 알아야 함

  - 작업을 위임하는 방법: 포워딩, 인클루딩

  1. forward 방식: 작업을 한 번 위임하면 다시 이전 서블릿으로 돌아오지 않음

     > 검색, 이동에 사용 => 기본값 <br /> 새로고침 시에 모두 수행됨(버그 발생 가능)

  2. include 방식: 작업을 한 번 위임하고 다 수행되면 다시 이전 서블릿으로 돌아옴

  ```
  - JSP 환경에서, 현재 작업 중인 페이지에서 다른 페이지로 이동하는 페이지 전환 방법 중 forward는 "Web Container 차원에서 페이지 이동만 있음"

  - 실제로 웹 브라우저는 다른 페이지로 이동했음을 알 수 없기 때문에 웹 브라우저에는 최초에 호출한 URL이 표시되고 이동한 페이지의 URL 정보는 볼 수 없음

  - 현재 실행 중인 페이지와 forward에 의해 호출된 페이지는 request와 response 객체를 공유함
  ```

  ```
  - Redirect는 Web Container에 Redirect 명령이 들어오면 웹 브라우저에 다른 페이지로 이동하라고 명령을 내림

  - 웹 브라우저는 URL을 지시된 주소로 변경하고 그 주소로 이동함

  - 새로운 페이지에서는 기존의 request와 response 객체를 사용할 수 없고 새로운 페이지의 새로운 request와 response 객체가 생성됨


  => 자료를 데이터베이스에 꼭 넣어야 하는 등 DB와 관련 있는 CRUD에서
     R를 제외한 행위를 취할 때 사용 === 보완(새로고침 시에도 버그 발생 X)
  ```

<br />

- load-on-startup

  - AppInitServlet.java

  ```java
  package spms.servlets;

  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.SQLException;

  import jakarta.servlet.ServletConfig;
  import jakarta.servlet.ServletContext;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.http.HttpServlet;

  public class AppInitServlet extends HttpServlet {
    @Override
    public void init(ServletConfig config) throws ServletException {
      System.out.println("AppInitServlet 준비");

      super.init(config);

      try {
        ServletContext sc = ths.getServletContext();

        String driver = "";
        String url = "";
        String user = "";
        String password = "";

        Connection conn = null;

        driver = sc.getInitParameter("driver");
        url = sc.getInitParameter("url");
        user = sc.getInitParameter("user");
        password = sc.getInitParameter("password");

        Class.forName(driver);

        conn = DriverManager.getConnection(url, user, password);

        sc.setAttribute("conn", conn);

        System.out.println("DB 연결 성공");
      } catch (ClassNotFoundException e) {
        e.printStackTrace();
      } catch (SQLException e) {
        e.printStackTrace();
      }
    }

    @Override
    public void destroy() {
      System.out.println("AppInitServlet 마무리");

      super.destroy();

      ServletContext sc = this.getServletContext();

      Connection conn = (Connection) sc.getAttribute("conn");

      try {
        if (conn != null) {
          conn.close();
          System.out.println("DB 연결 해제");
        }
      } catch (SQLException e) {
        e.printStackTrace();
      }
    }
  }
  ```

  <br />

  - web.xml

  ```xml
  <!-- 추가 -->
  <servlet>
    <servlet-name>AppInitServlet</servlet-name>
    <servlet-class>spms.servlets.AppInitServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet-class>
  ```

  <br />

  - LoginServlet.java

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;

  import jakarta.servlet.RequestDispatcher;
  import jakarta.servlet.ServletContext;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.annotation.WebServlet;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;
  import jakarta.servlet.http.HttpSession;
  import spms.dto.MemberDto;

  @WebServlet(value = "/auth/login")
  public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      RequestDispatcher rd = req.getRequestDispatcher("./LoginForm.jsp");
      rd.forward(req, res);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // JDBC 실행 순서
      // DB 객체 준비
      Connection conn = null;
      PreparedStatement pstmt = null;
      ResultSet rs = null;

      try {
        ServletContext sc = this.getServletContext();
        // 미리 준비된 DB 객체 불러오기
        conn = (Connection) sc.getAttribute("conn");

        String email = req.getParameter("email");
        String pwd = req.getParameter("password");
        String memberName = "";

        String sql = "";
        int colIndex = 1;

        sql += "SELECT MEMBER_NO, EMAIL, MEMBER_NAME";
        sql += " FROM MEMBER";
        sql += " WHERE EMAIL = ? AND PWD = ?";

        pstmt = conn.prepareStatement(sql);
        pstmt.setString(colIndex++, email);
        pstmt.setString(colIndex, pwd);

        // db에 sql문 전달, 실행
        rs = pstmt.executeQuery();

        if (rs.next()) {
          email = rs.getString("EMAIL");
          memberName = rs.getString("MEMBER_NAME");
          MemberDto memberDto = new MemberDto(email, memberName);
          HttpSession session = req.getSession(); // session
          session.setAttribute("memberDto", memberDto);
        }

        res.sendRedirect("../member/list");
      } catch (SQLException e) {
        e.printStackTrace();
      } catch (Exception e) {

        e.printStackTrace();
        // throw new ServletException(e);
        req.setAttribute("error", e);
        req.setAttribute("caseByCase", "상황에 맞는 처리 부탁");

        RequestDispatcher dispatcher = req.getRequestDispatcher("/Error.jsp");

        dispatcher.forward(req, res);
      } finally {

        // db 객체 메모리 해제
        if (rs != null) {
          try {
            rs.close();
          } catch (SQLException e) {
            e.printStackTrace();
          }
        }

        if (pstmt != null) {
          try {
            pstmt.close();
          } catch (SQLException e) {
            e.printStackTrace();
          }
        }
      } // finally 종료
    }
  }
  ```

  <br />

  - LoginForm.jsp

  ```html
  <!-- LoginForm.jsp -->
  <%@ page language="java" contentType="text.html; charset=UTF-8"
  pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>로그인</title>
    </head>
    <body>
      <h2>사용자 로그인</h2>
      <form action="./login" method="post">
        <label>이메일</label>
        <input name="email" value="" placeholder="ex:hong@test.com" />
        <br />
        <label>암호</label> <input type="password" name="password" value="" />
        <br />
        <input type="submit" value="로그인" />
      </form>
    </body>
  </html>
  ```

<br />

- 데이터 보관소

  - 서블릿들이 서로 작업을 수행할 때, 데이터를 공유하는 방법

  - 서블릿에서는 4가지 종류의 데이터 보관소 제공(공유 범위를 기준으로 구분)

  ```
  1. ServletContext 보관소

    - 웹 애플리케이션이 시작될 때 생성되어, 웹 애플리케이션이 종료될 때까지 유지

    - 데이터 보관 시 웹 애플리케이션이 실행되는 동안에는 모든 서블릿이 사용할 수 있음

    - JSP에서는 application 변수를 통해 보관소 이용 가능


  2. HttpSession 보관소

    - 클라이언트의 최초 요청 시 생성되어, 브라우저를 닫을 때까지 유지

    - 보통 로그인할 때 이 보관소를 초기화하고, 로그아웃하면 이 보관소에 저장된 값들을 비움

    - 데이터 보관 시 서블릿이나 JSP 페이지에 상관없이 로그아웃 전까지 계속 데이터 유지 가능


  3. ServletRequest 보관소

    - 클라이언트의 요청이 들어올 때 생성되어, 클라이언트에게 응답할 때까지 유지

    - 포워딩이나 인클루딩 하는 서블릿들 사이에서 값을 공유할 때 유용

    - JSP에서는 request 변수를 통해 보관소 참조 가능


  4. JspContext 보관소

    - JSP 페이지를 실행하는 동안만 유지

    - JSP에서는 pageContext 변수를 통해 보관소 이용 가능
  ```

<br />

- session

  - 클라이언트와 서버의 연결을 유지시켜 주는 방법 중 하나

  - 웹 애플리케이션이 시작될 때 생성되어, 웹 애플리케이션이 종료될 때까지 유지되는 객체

  - 일정 시간 동안 같은 사용자(브라우저 및 클라이언트)로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 일정하게 유지하는 기술

  ```
  - 클라이언트 요청에 따른 정보를 클라이언트 메모리에 저장하는 것이 아닌 웹 서버가 세션 아이디 파일을 만들어 서비스가 돌아가고 있는 "서버에 저장"하는 것

  => 서버에 저장되기 때문에 사용자 정보가 노출되지 않아 보안성이 높음
  ```

<br />

- Login, Logout, Login Fail

  ```java
  // LoginServlet.java
  package spms.servlets;

  import java.io.IOException;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;

  import jakarta.servlet.RequestDispatcher;
  import jakarta.servlet.ServletContext;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.annotation.WebServlet;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;
  import jakarta.servlet.http.HttpSession;
  import spms.dto.MemberDto;

  @WebServlet(value = "/auth/login")
  public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      RequestDispatcher rd = req.getRequestDispatcher("./LoginForm.jsp");
      rd.forward(req, res);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // JDBC 실행 순서
      // DB 객체 준비
      Connection conn = null;
      PreparedStatement pstmt = null;
      ResultSet rs = null;

      try {
        ServletContext sc = this.getServletContext();
        // 미리 준비된 DB 객체 불러오기
        conn = (Connection) sc.getAttribute("conn");

        String email = req.getParameter("email");
        String pwd = req.getParameter("password");
        String memberName = "";

        String sql = "";
        int colIndex = 1;

        sql += "SELECT MEMBER_NO, EMAIL, MEMBER_NAME";
        sql += " FROM MEMBER";
        sql += " WHERE EMAIL = ? AND PWD = ?";

        pstmt = conn.prepareStatement(sql);
        pstmt.setString(colIndex++, email);
        pstmt.setString(colIndex, pwd);

        // db에 sql문 전달, 실행
        rs = pstmt.executeQuery();

        if (rs.next()) {
          email = rs.getString("EMAIL");
          memberName = rs.getString("MEMBER_NAME");
          MemberDto memberDto = new MemberDto(email, memberName);
          HttpSession session = req.getSession(); // session
          session.setAttribute("memberDto", memberDto);
        }

        res.sendRedirect("../member/list");
      } catch (SQLException e) {
        e.printStackTrace();
      } catch (Exception e) {

        e.printStackTrace();
        // throw new ServletException(e);
        req.setAttribute("error", e);
        req.setAttribute("caseByCase", "상황에 맞는 처리 부탁");

        RequestDispatcher dispatcher = req.getRequestDispatcher("/Error.jsp");

        dispatcher.forward(req, res);
      } finally {

        // db 객체 메모리 해제
        if (rs != null) {
          try {
            rs.close();
          } catch (SQLException e) {
            e.printStackTrace();
          }
        }

        if (pstmt != null) {
          try {
            pstmt.close();
          } catch (SQLException e) {
            e.printStackTrace();
          }
        }
      } // finally 종료
    }
  }
  ```

  - LoginForm.jsp

  ```html
  <!-- LoginForm.jsp -->
  <%@ page language="java" contentType="text.html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>로그인</title>
    </head>
    <body>
      <h2>사용자 로그인</h2>
      <form action="./login" method="post">
        <label>이메일</label>
        <input name="email" value="" placeholder="ex:hong@test.com" />
        <br />
        <label>암호</label> <input type="password" name="password" value="" />
        <br />
        <input type="submit" value="로그인" />
      </form>
    </body>
  </html>
  ```

  <br />

  ```java
  // LogoutServlet
  package spms.servlets;

  import java.io.IOException;

  import jakarta.servlet.ServletException;
  import jakarta.servlet.annotation.WebServlet;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;
  import jakarta.servlet.http.HttpSession;

  @WebServlet(value = "/auth/logout")
  public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      doPost(req, res);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      HttpSession session = req.getSession();

      session.invalidate();

      res.sendRedirect("./login");
    }
  }
  ```

  <br />

  - LoginFail.jsp

  ```html
  <!-- LoginFail.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" http-equiv="Refresh" content="5;url=./login" />
      <title>로그인 실패</title>
    </head>
    <body>
      <h1>로그인 실패</h1>
      <pre>
        아이디(로그인 전용 아이디) 또는 비밀번호가 잘못 되었습니다.
        아이디와 비밀번호를 정확히 입력해 주세요.
        다시 시도하거나 비밀번호 찾기를 클릭하여 재설정하세요.
  
        잠시 후에 다시 로그인 화면으로 전환합니다.
      </pre>
      <div>
        <span>입력하신 이메일</span>
        <span><%= request.getParameter("email") %></span>
      </div>

      <button
        onclick="location.href='<%= request.getContextPath() %>/auth/login'"
      >
        로그인 페이지로 이동
      </button>
    </body>
  </html>
  ```

  <br />

  - Header.jsp

  ```html
  <!-- Header.jsp -->
  <%@ page import="spms.dto.MemberDto"%> <%@ page language="java"
  contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%> <% MemberDto
  sessionMemberDto = (MemberDto) session.getAttribute("memberDto");%>

  <div
    style="background-color: #663399; color: #fff; height: 35px; padding: 5px; line-height: 35px; font-weight: 700;"
  >
    SPMS(Simple Project Management System)
    <span style="float: right;">
      <%=sessionMemberDto.getMemberName()%>
      <!-- 공통단의 코드들은 가능한 절대 경로로 설정해야 어떤 구조에서든 동작 -->
      <a
        style="color: #86eb86;"
        href="<%=request.getContextPath()%>/auth/logout"
        >(로그아웃)</a
      >
    </span>
  </div>
  ```
