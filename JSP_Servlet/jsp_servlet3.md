# JSP & Servlet 3일차

## 3일차

- 웹 프로젝트 폴더 구조

  1. src

  ```
  - 자바소스 파일을 두는 폴더

  - 서블릿 클래스나 필터, 리스너 등 필요한 모든 자바 클래스 파일을 둠

  - 프로퍼티 파일도 이 폴더에 둠
  ```

  <br />

  2. build/classes

  ```
  - 컴파일된 자바 클래스 파일이 놓이는 폴더

  - 물론 패키지에 소속된 클래스인 경우 이 폴더에 해당 패키지가 자동으로 만들어짐


  * 깃허브에서 .gitignore 파일에 추가해야 함
  ```

  <br />

  3. WebContent(webapp로 변경됨)

  ```
  - HTML, CSS, JavaScript, jsp, 이미지 파일 등 웹 콘텐츠를 두는 폴더

  - 웹 애플리케이션을 서버에 배치할 때 이 폴더의 내용물이 그대로 복사됨
  ```

  <br />

  4. WebContent/WEB-INF

  ```
  - 웹 애플리케이션의 설정과 관련된 파일을 두는 폴더

  - 이 폴더에 있는 파일은 클라이언트에서 요청할 수 없음

  - HTML이나 JavaScript, CSS 등 클라이언트에서 요청할 수 있는 파일들을 이 폴더에 두어선 안 됨(보안 영역)
  ```

  <br />

  5. WebContent/WEB-INF/lib

  ```
  - 자바 아카이브 파일을 두는 폴더

  - Java Archive의 합성어를 확장자 명(.jar)로 사용 중


  * 아카이브: 기록보관소

  * 아카이브 파일: 클래스 파일과 프로퍼티들을 모아놓은 보관소 파일
  ```

<br />

- GenericServlet

  - 기존의 Servlet 인터페이스를 이용하는 것보다 더 쉽게 작성하여 사용할 수 있음

  - 간단한 버전으로 Lifecycle에 관련된 init()과 destory() 메서드와 ServletConfig를 제공

  - service() 메서드만 구현

<br />

- JDBC 실행 순서

  1. DB 객체 준비

  ```java
  Connection conn = null;
  Statement stmt = null;
  ResultSet rs rs = null;

  String url = "jdbc:oracle:thin:@localhost:1521:xe";
  String user = "edu";
  String password = "edu12";
  ```

  <br />

  2. 오라클 객체 불러오기: try 구문 내

  ```java
  stmt = conn.createStatement();
  ```

  <br />

  3. DB에 sql문 전달, 실행: try 구문 내

  ```java
  String sql = "";

  sql += "SELECT MEMBER_MO, EMAIL, PWD, MEMBER_NAME, CRE_DATE\r";
  sql += "FROM MEMBER\r";
  sql += "ORDER BY MEMBER_NO ASC";

  rs = stmt.executeQuery(sql);
  ```

  <br />

  4. SELECT 결과 활용: try 구문 내

  ```java
  PrintWriter out = res.getWriter();

  out.println("<html><head><title>회원목록</title></head>");
  out.println("<body><h1>회원목록</h1>");

  while(rs.next() == true) {
    out.println(
      rs.getInt("MEMBER_NO") + "," +
      rs.getString("MEMBER_NAME") + "," +
      rs.getString("EMAIL") + "," +
      rs.getDate("CRE_DATE") + "<br />"
    );
  }

  out.println("</body></html>");
  ```

  <br />

  5. DB 객체 메모리 해제: finally 구문 내

  ```java
  if(rs != null) {
      try {
        rs.close();
      } catch (SQLException e) {
        // TODO: handle exception
        e.printStackTrace();
      }
  }

  if(stmt != null) {
      try {
        stmt.close();
      } catch (SQLException e) {
        // TODO: handle exception
        e.printStackTrace();
      }
  }

  if(conn != null) {
      try {
        conn.close();
      } catch (SQLException e) {
        // TODO: handle exception
        e.printStackTrace();
      }
  }
  ```

  > webapp/WEB-INF/lib 폴더에 ojdbc6.jar 필요

  <br />

  - 전체 코드

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.sql.Statement;

  import jakarta.servlet.GenericServlet;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.ServletRequest;
  import jakarta.servlet.ServletResponse;


  /**
  * ALT + SHITF + J: API 주석
  * 회원 목록 조회 구현
  *
  */
  public class MemberListServlet extends GenericServlet {

    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {

        // JDBC 실행 순서
        // DB 객체 준비
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        String url = "jdbc:oracle:thin:@localhost:1521:xe";
        String user = "edu";
        String password = "edu12";

        try {
            // 오라클 객체 불러오기
          Class.forName("oracle.jdbc.driver.OracleDriver");

          // 드라이브 매니저에 jdbc 등록 -> db 연결 -> db 객체
          conn = DriverManager.getConnection(url, user, password);

          // sql 실행 객체 준비
          stmt = conn.createStatement();

          String sql = "";

          sql += "SELECT MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE\r\n";
          sql   += "FROM MEMBER\r\n";
          sql += "ORDER BY MEMBER_NO ASC";

          // db에 sql문 전달, 실행
          rs = stmt.executeQuery(sql);

          res.setContentType("text/html");
          res.setCharacterEncoding("UTF-8");

          PrintWriter out = res.getWriter();

          out.println("<html><head><title>회원목록</title></head>");
          out.println("<body><h1>회원목록</h1>");

          // select 결과 활용
          while(rs.next() == true) {
              out.println(
                rs.getInt("MEMBER_NO") + "," +
                rs.getString("MEMBER_NAME") + "," +
                rs.getString("EMAIL") + "," +
                rs.getDate("CRE_DATE") + "<br />"
              );
          }
          out.println("</body></html>");

        } catch (ClassNotFoundException e) {
          // TODO: handle exception
          e.printStackTrace();
        } catch (SQLException e) {
          // TODO: handle exception
          e.printStackTrace();
        } finally {
          // db 객체 메모리 해제
          if(rs != null) {
              try {
                rs.close();
              } catch (SQLException e) {
                // TODO: handle exception
                e.printStackTrace();
              }
          }

          if(stmt != null) {
              try {
                stmt.close();
              } catch (SQLException e) {
                // TODO: handle exception
                e.printStackTrace();
              }
          }

          if(conn != null) {
              try {
                conn.close();
              } catch (SQLException e) {
                // TODO: handle exception
                e.printStackTrace();
              }
          }

        } // finally 종료

    } // service 종료

  }
  ```

<br />

- Statement vs PreparedStatement

  - Statement stmt: 객체를 미리 준비함 => 해킹 가능

  - PreparedStatement: 문자열을 바로 매개변수로 가짐

  ```java
  Connection conn = null;
  PreparedStatement pstmt = null;

  String url = ""; // 실제 값 필요
  String user = ""; // 실제 값 필요
  String password = ""; // 실제 값 필요

  String sql

  pstmt = conn.prepareStatement(sql);

  pstmt.setString(1, emailStr); // 맵핑

  pstmt.executeUpdate(); // 인서트, 업데이트, 딜리트 모두 한 번에 가능
  ```

<br />

- 실습

  - web.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
    id="WebApp_ID" version="6.0">

    <display-name>webMiniBoardBasic</display-name>

    <servlet>
      <servlet-name>MemberList</servlet-name>
      <servlet-class>spms.servlets.MemberListServlet</servlet-class>
    </servlet>

    <servlet-mapping>
      <servlet-name>MemberList</servlet-name>
      <url-pattern>/member/list</url-pattern>
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

  - MemberListServlet

    - extends GenericServlet

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.sql.Statement;

  import jakarta.servlet.GenericServlet;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.ServletRequest;
  import jakarta.servlet.ServletResponse;

  /**
  * ALT + SHITF + J: API 주석
  * 회원 목록 조회 구현
  *
  */
  public class MemberListServlet extends GenericServlet {

    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {

      // JDBC 실행 순서
      // DB 객체 준비
      Connection conn = null;
      Statement stmt = null;
      ResultSet rs = null;

      String url = "jdbc:oracle:thin:@localhost:1521:xe";
      String user = "edu";
      String password = "edu12";

      try {
        // 오라클 객체 불러오기
        Class.forName("oracle.jdbc.driver.OracleDriver");

        // 드라이브 매니저에 jdbc 등록 -> db 연결 -> db 객체
        conn = DriverManager.getConnection(url, user, password);

        // sql 실행 객체 준비
        stmt = conn.createStatement();

        String sql = "";

        sql += "SELECT MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE\r\n";
        sql += "FROM MEMBER\r\n";
        sql += "ORDER BY MEMBER_NO ASC";

        // db에 sql문 전달, 실행
        rs = stmt.executeQuery(sql);

        res.setContentType("text/html");
        res.setCharacterEncoding("UTF-8");

        PrintWriter out = res.getWriter();

        String htmlStr = "";

        htmlStr += "";

        //
        htmlStr += "<p>";
        htmlStr += "<a href=\"./add\">신규 회원 등록</a>";
        htmlStr += "</p>";

        out.println("<html><head><meta charset='UTF-8'/>"
            + "<title>회원목록</title>"
            + "</head>");
        out.println("<body><h1>회원목록</h1>");
        // 신규 회원 등록
        out.println(htmlStr);

        // select 결과 활용
        while (rs.next() == true) {
          out.println(
              rs.getInt("MEMBER_NO") + "," +
                  rs.getString("MEMBER_NAME") + "," +
                  rs.getString("EMAIL") + "," +
                  rs.getDate("CRE_DATE") + "<br />");
        }
        out.println("</body></html>");

      } catch (ClassNotFoundException e) {
        // TODO: handle exception
        e.printStackTrace();
      } catch (SQLException e) {
        // TODO: handle exception
        e.printStackTrace();
      } finally {
        // db 객체 메모리 해제
        if (rs != null) {
          try {
            rs.close();
          } catch (SQLException e) {
            // TODO: handle exception
            e.printStackTrace();
          }
        }

        if (stmt != null) {
          try {
            stmt.close();
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

      } // finally 종료

    } // service 종료

  }
  ```

  <br />

  - MemberAddServlet

    - extends HttpServlet

    - doGet(): 속도가 빠름, 조회용, 보안성 낮음 => 화면(Front)

    - doPost(): 속도가 느림, 생성이나 수정용, 보안성 높음 => 기능(Back)

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.io.PrintWriter;

  import jakarta.servlet.ServletException;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;

  public class MemberAddServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // TODO Auto-generated method stub
      System.out.println("doGet을 수행");

      res.setContentType("text/html");
      res.setCharacterEncoding("UTF-8");
      PrintWriter out = res.getWriter();

      String htmlStr = "";

      htmlStr += "<html><head><title>회원 등록</title></head>";
      htmlStr += "<body>";
      htmlStr += "<h1>회원등록</h1>";
      htmlStr += "<form action='add' method='post'>";
      htmlStr += "이름: <input type='text' name='name' /><br />";
      htmlStr += "이메일: <input type='text' name='email' /><br />";
      htmlStr += "암호: <input type='password' name='password' /><br />";
      htmlStr += "<input type='submit' value='추가' />";
      htmlStr += "<input type='reset' value='취소' />";
      htmlStr += "</form>";
      htmlStr += "</body></html>";

      out.println(htmlStr);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // TODO Auto-generated method stub
    }
  }
  ```

  - MemberAddServlet 완성

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.SQLException;

  import jakarta.servlet.ServletException;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;

  public class MemberAddServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // TODO Auto-generated method stub
      System.out.println("doGet을 수행");

      res.setContentType("text/html");
      res.setCharacterEncoding("UTF-8");
      PrintWriter out = res.getWriter();

      String htmlStr = "";

      htmlStr += "<html><head><title>회원 등록</title></head>";
      htmlStr += "<body>";
      htmlStr += "<h1>회원등록</h1>";
      htmlStr += "<form action='add' method='post'>";
      htmlStr += "이름: <input type='text' name='mamberName' /><br />";
      htmlStr += "이메일: <input type='text' name='email' /><br />";
      htmlStr += "암호: <input type='password' name='password' /><br />";
      htmlStr += "<input type='submit' value='추가' />";
      htmlStr += "<input type='reset' value='취소' />";
      htmlStr += "</form>";
      htmlStr += "</body></html>";

      out.println(htmlStr);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      // TODO Auto-generated method stub
      Connection conn = null;
      PreparedStatement pstmt = null;

      String url = "jdbc:oracle:thin:@localhost:1521:xe";
      String user = "edu";
      String password = "edu12";

      try {
        String emailStr = req.getParameter("email");
        String pwdStr = req.getParameter("password");
        String memberNameStr = req.getParameter("mamberName");

        Class.forName("oracle.jdbc.driver.OracleDriver");
        conn = DriverManager.getConnection(url, user, password);

        String sql = "";

        sql += "INSERT INTO MEMBER\r\n";
        sql += "(MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE, MOD_DATE)/r/n";
        sql += "VALUES(MEMBER_NO_SEQ.NEXTVAL, ?, ?, ?, SYSDATE, SYSDATE)";

        pstmt = conn.prepareStatement(sql);

        pstmt.setString(1, emailStr);
        pstmt.setString(2, pwdStr);
        pstmt.setString(3, memberNameStr);

        pstmt.executeUpdate();

        // 사용자에게 백단에서 무슨 일이 벌어진 건지 알려주는 화면을 제작해야 함
        res.setContentType("text/html");
        res.setCharacterEncoding("UTF-8");

        PrintWriter out = res.getWriter();

        String htmlStr = "";

        htmlStr += "<html>";
        htmlStr += "<head>";
        htmlStr += "<meta charset=\"UTF-8\">";
        htmlStr += "<title>회원등록 결과</title>";
        htmlStr += "</head>";
        htmlStr += "<body>";
        htmlStr += "<p>";
        htmlStr += "등록 성공입니다!";
        htmlStr += "</p>";
        htmlStr += "</body>";
        htmlStr += "</html>";

        out.println(htmlStr);

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
