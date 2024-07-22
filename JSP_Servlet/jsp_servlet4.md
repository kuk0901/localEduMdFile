# JSP & Servlet 4일차

## 4일차

- Statement VS PreparedStatement

  - Statement

  - 실행 속도: 질의할 때마다 SQL문을 컴파일

  - 바이너리 데이터 전송: 불가능

  - 프로그래밍 편의성: sql문 안에 입력 매개변수 값이 포함되어 있어서 sql문이 복잡하고 매개변수가 여러 개인 경우 코드 관리가 힘듦

  - 보안성; 해킹당할 우려 존재

  - Statement는 개행을 위해 띄어쓰기 필수

  ```java
  Connection conn = null;
  Statement stmt = null;
  ResultSet rs = null;

  // ... 내용 생략

  stmt = conn.createStatement();

  String sql = "";

  sql += "SELECT MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE";
  sql += " FROM MEMBER";
  sql += " ORDER BY MEMBER_NO";

  rs = stmt.executeQuery(sql);
  ```

  <br />

  - PreparedStatement

  - 실행속도: sql문을 미리 준비하여 컴파일 해둠, 입력 매개변수 값만 추가하여 서버에 전송함, 특히 여러 번 반복하여 질의하는 경우 실행속도가 빠름

  - 바이너리 데이터 전송: 가능

  - 프로그래밍 편의성: sql문과 입력 매개변수가 분리되어 있어서 코드 작성이 편리함

  - 보안성: 해킹 불가능

  - PreparedStatement는 띄어쓰기 하지 않아도 문법적 오류 아님

  ```java
  Connection conn = null;
  PreparedStatement pstmt = null;

  // ... 내용 생략

  pstmt = conn.createStatement();

  String sql = "";

  sql += "SELECT MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE";
  sql += "FROM MEMBER";
  sql += "ORDER BY MEMBER_NO";

  pstmt = conn.prepareStatement(sql);
  ```

<br />

- 메서드로 문자열 대체하기

  ```java
  String htmlStr = "";
  htmlStr = "<meta charset='UTF-8' http-equiv='Refresh' content='1; url=list'/>";

  out.println(htmlStr);

  res.addHeader("Refresh", "2; url=./list");
  ```

<br />

- WebServlet annotation

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
    id="WebApp_ID" version="6.0">
    <display-file-list>webMiniBoardBasic</display-file-list>
    <welcome-file-list>
      <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

    <!-- <servlet>
      <servlet-name>MemberList</servlet-name>
      <servlet-class>spms.servlets.MemberListServlet</servlet-class>
    </servlet>
    <servlet-mapping>
      <servlet-name>MemberList</servlet-name>
      <url-pattern>/member/list</url-pattern>
    </servlet-mapping> -->
  ```

  ```java
  // MemberListServlet

  import jakarta.servlet.annotation.WebServlet;

  @WebServlet(value = "/member/list")
  public class MemberListServlet extends GenericServlet {

  }
  ```

<br />

- MemberUpdateServlet

  - web.xml

  ```xml
  <servlet>
    <servlet-name>MemberUpdateServlet</servlet-name>
    <servlet-class>spms.servlets.MemberUpdateServlet</servlet-class>
    <init-param>
      <param-name>driver</param-name>
      <param-value>oracle.jdbc.driver.OracleDriver</param-value>
    </init-param>
    <init-param>
      <param-name>url</param-name>
      <param-value>jdbc:oracle:thin:@localhost:1521:xe</param-value>
    </init-param>
    <init-param>
      <param-name>user</param-name>
      <param-value>eud</param-value>
    </init-param>
    <init-param>
      <param-name>password</param-name>
      <param-value>eud12</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>MemberUpdateServlet</servlet-name>
    <url-pattern>/member/update</url-pattern>
  </servlet-mapping>
  ```

  <br />

  - MemberUpdateServlet

    - 사용자의 모든 정보는 req에 들어있음

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.Date;

  import jakarta.servlet.ServletException;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;

  public class MemberUpdateServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // TODO Auto-generated method stub
      Connection conn = null;
      PreparedStatement pstmt = null;
      ResultSet rs = null;

      String driver = this.getInitParameter("driver");
      String url = this.getInitParameter("url");
      String user = this.getInitParameter("user");
      String password = this.getInitParameter("password");

      // DB 조회를 위해, 화면을 데이터 기반으로 구성하기 위해 필요한 데이터
      // Cannot parse null string
      int memberNo = Integer.parseInt(req.getParameter("memberNo"));

      String sql = "";

      try {

        Class.forName(driver);
        System.out.println("오라클 드라이버 로드");
        conn = DriverManager.getConnection(url, user, password);

        sql = "SELECT MEMBER_NO, EMAIL, MEMBER_NAME, CRE_DATE";
        sql += " FROM MEMBER";
        sql += " WHERE MEMBER_NO = ?";

        pstmt = conn.prepareStatement(sql);

        pstmt.setInt(1, memberNo);

        rs = pstmt.executeQuery();

        String memberName = "";
        String email = "";
        Date creDate = null;

        while (rs.next()) {
          memberName = rs.getString("MEMBER_NAME");
          email = rs.getString("EMAIL");
          creDate = rs.getDate("CRE_DATE");
        }

        // 사용자에게 백단에서 무슨 일이 벌어진 건지 알려주는 화면을 제작해야 함
        res.setContentType("text/html");
        res.setCharacterEncoding("UTF-8");

        PrintWriter out = res.getWriter();

        String htmlStr = "";

        htmlStr += "<html>";
        htmlStr += "<head>";
        htmlStr += "<title>회원 등록</title>";
        htmlStr += "</head>";
        htmlStr += "<body>";
        htmlStr += "<h1>회원등록</h1>";
        htmlStr += "<form action='./update' method='post'>";
        htmlStr += "번호: <input type='text' name='memberNo' value='" + memberNo + "' readonly='readonly'/><br />";
        htmlStr += "이름: <input type='text' name='memberName' value='" + memberName + "' /><br />";
        htmlStr += "이메일: <input type='text' name='email' value='" + email + "' /><br />";
        htmlStr += "가입일: " + creDate + "<br />";
        htmlStr += "<input type='submit' value='정보 수정' />";
        htmlStr += "<input type='button' value='취소' onclick='location.href=\"./list\"'/>";
        htmlStr += "</form>";
        htmlStr += "</body>";
        htmlStr += "</html>";

        out.println(htmlStr);

        System.out.println("회원 상세 정보 수정 페이지 체크");

      } catch (ClassNotFoundException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      } catch (SQLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      } finally {
        if (rs != null) {
          try {
            rs.close();
          } catch (SQLException e) {
            // TODO: handle exception
            e.printStackTrace();
          }
        }

        if (pstmt != null) {
          try {
            pstmt.close();
          } catch (SQLException e) {
            // TODO: handle exception
            e.printStackTrace();
          }
        }

        if (conn != null) {
          try {
            conn.close();
          } catch (SQLException e) {
            // TODO: handle exception
            e.printStackTrace();
          }
        }
      } // finally end

      res.setContentType("text/html");
      res.setCharacterEncoding("UTF-8");
      PrintWriter out = res.getWriter();

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // TODO Auto-generated method stub
      Connection conn = null;
      PreparedStatement pstmt = null;

      String driver = this.getInitParameter("driver");
      String url = this.getInitParameter("url");
      String user = this.getInitParameter("user");
      String password = this.getInitParameter("password");

      try {
        String emailStr = req.getParameter("email");
        String memberNameStr = req.getParameter("memberName");
        int memberNo = Integer.parseInt(req.getParameter("memberNo"));

        Class.forName(driver);
        conn = DriverManager.getConnection(url, user, password);

        String sql = "";

        sql += "UPDATE MEMBER";
        sql += " SET EMAIL = ?, MEMBER_NAME = ?";
        sql += " WHERE MEMBER_NO = ?";

        pstmt = conn.prepareStatement(sql);

        pstmt.setString(1, emailStr);
        pstmt.setString(2, memberNameStr);
        pstmt.setInt(3, memberNo);

        pstmt.executeUpdate();

        res.sendRedirect("list");

      } catch (ClassNotFoundException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      } catch (SQLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      } finally {
        if (pstmt != null) {
          try {
            pstmt.close();
          } catch (SQLException e) {
            // TODO: handle exception
            e.printStackTrace();
          }
        }

        if (conn != null) {
          try {
            conn.close();
          } catch (SQLException e) {
            // TODO: handle exception
            e.printStackTrace();
          }
        }
      } // finally end
    }
  }
  ```

<br />

- res.sendRedirect()

  - 다른 화면 혹은 서블릿을 호출하는 메서드

  - 기존의 페이지를 재활용하는 경우 사용

  ```java
  @Override
  public void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
    Connection conn = null;
    PreparedStatement pstmt = null;

    String url = "jdbc:oracle:thin:@localhost:1521:xe";
    String user = "edu";
    String password = "edu12";

    try {
      // 내부 로직

      pstmt = conn.prepareStatement(sql);

      // 추가 로직

      pstmt.executeUpdate();

      // 다른 화면 혹은 서블릿을 호출하는 메서드
      res.sendRedirect("./list");

    } catch (ClassNotFoundException e) {
      e.printStackTrace();
    } catch (SQLException e) {
      e.printStackTrace();
    } finally {
      if (pstmt != null) {
        try {
          pstmt.close();
        } catch(SQLException e) {
          e.printStackTrace();
        }
      }

      if (conn != null) {
        try {
          conn.close();
        } catch(SQLException e) {
          e.printStackTrace();
        }
      }
    } // finally end
  }
  ```

<br />

- ServletContext 사용

  - web.xml

  ```xml
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

    <servlet>
      <servlet-name>MemberUpdateServlet</servlet-name>
      <servlet-class>spms.servlets.MemberUpdateServlet</servlet-class>
    </servlet>
    <servlet-mapping>
      <servlet-name>MemberUpdateServlet</servlet-name>
      <url-pattern>/member/update</url-pattern>
    </servlet-mapping>

    <servlet>
      <servlet-name>MemberAdd</servlet-name>
      <servlet-class>spms.servlets.MemberAddServlet</servlet-class>
    </servlet>
    <servlet-mapping>
      <servlet-name>MemberAdd</servlet-name>
      <url-pattern>/member/add</url-pattern>
    </servlet-mapping>

    <welcome-file-list>
      <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
  </web-app>
  ```

  <br />

  - Servlet의 this를 context로 사용

  > import jakarta.servlet.ServletContext; 주의

  ```java
  String driver = this.getInitParameter("driver");
  String url = this.getInitParameter("url");
  String user = this.getInitParameter("user");
  String password = this.getInitParameter("password");

  // 위의 로직을 아래로 변경

  ServletContext sc = this.getServletContext();

  String driver = sc.getInitParameter("driver");
  String url = sc.getInitParameter("url");
  String user = sc.getInitParameter("user");
  String password = sc.getInitParameter("password");
  ```
