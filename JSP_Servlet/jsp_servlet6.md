# JSP & Servlet 6일차

## 6일차

- All In One 방식

  - 서블릿 하나에서 모든 로직을 다 처리하는 방식

  - 목적이 거의 변하지 않고 명확하다면 올인원도 나쁜 선택은 아님

  ```
  - 장점

    1. 모든 기능이 하나의 클래스에 들어가 있어 빠르고 작업하기 편리함

    2. 유지 비용이 적게 듬

    3. 의사 결정이 빠름


  - 단점

    1. 특정 로직을 변경하고 싶을 때 어디에서 문제가 발생하고 변경해야 할지 찾기 어려우며 특정 기능만 수정하기 어려움

    2. 맡은 업무가 마비되면 전체에 문제가 생김

    3. 내용이 많아지면 작업이 어려움


  => 요약: 현재의 글로벌 프로그램 개발에는 적합하지 않음
  ```

<br />

- MVC 패턴

  - All In One 방식과 거의 반대임

  ```
  => 요약: 현재의 글로벌 프로그램 개발에 적합함(분업 가능)
  ```

<br />

- MVC 패턴 실습

  - DTO

  ```java
  package Test.java.dto;

  import java.util.Date;

  public class MemberDto {
    private int memberNo;
    private String email;
    private String password;
    private Date createdDate;
    private Date modifiedDate;

    public MemberDto() {
    }

    public MemberDto(int memberNo, String email, String password, Date createdDate, Date modifiedDate) {
      this.memberNo = memberNo;
      this.email = email;
      this.password = password;
      this.createdDate = createdDate;
      this.modifiedDate = modifiedDate;
    }

    public int getMemberNo() {
      return memberNo;
    }

    public void setMemberNo(int memberNo) {
      this.memberNo = memberNo;
    }

    public String getEmail() {
      return email;
    }

    public void setEmail(String email) {
      this.email = email;
    }

    public String getPassword() {
      return password;
    }

    public void setPassword(String password) {
      this.password = password;
    }

    public Date getCreatedDate() {
      return createdDate;
    }

    public void setCreatedDate(Date createdDate) {
      this.createdDate = createdDate;
    }

    public Date getModifiedDate() {
      return modifiedDate;
    }

    public void setModifiedDate(Date modifiedDate) {
      this.modifiedDate = modifiedDate;
    }

    @Override
    public String toString() {
      return "MemberDto{" +
          "memberNo=" + memberNo +
          ", email='" + email + '\'' +
          ", password='" + password + '\'' +
          ", createdDate=" + createdDate +
          ", modifiedDate=" + modifiedDate +
          '}';
    }
  }
  ```

  <br />

  - MemberListView.jsp

  ```html
  <!-- MemberListView.jsp -->
  <%@ page import="spms.dto.MemberDto"%>
  <%@ page import="java.util.ArrayList"%>
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <title>Insert title here</title>
      <style></style>
      <script></script>
    </head>
    <body>
      <p>
        <a href="./add">신규 회원 등록</a>
      </p>
      <%
        ArrayList<MemberDto> memberList = (ArrayList<MemberDto>)request.getAttribute("memberList");
        for(MemberDto memberDto : memberList) {
      %>
      <%=memberDto.getMemberNo() %>,
      <a href='./update?memberNo=<%=memberDto.getMemberNo()%>'>
        <%=memberDto.getMemberName()%>
      </a>,
      <%=memberDto.getMemberName()%>,
      <%=memberDto.getEmail()%>,
      <%=memberDto.getCreatedDate()%>,
      <a href='./delete?memberNo=<%=memberDto.getMemberNo()%>'>[삭제]</a>
      <br>
      <%
        } // for end
      %>
    </body>
  </html>
  ```

  <br />

  - index.jsp

  ```html
  <!-- index.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>
      <script>
        function pageMoveMemberListFnc() {
          location.href = "./member/list";
        }
      </script>
    </head>
    <body>
      <h1>Hello JSP&amp;Servlet</h1>
      <p>환영</p>
      <button onclick="pageMoveMemberListFnc();">회원 목록으로 이동</button>
    </body>
  </html>
  ```

<br />

- JSP 전용 태그

  - 지시자(Directive)

  ```
  - 시작 태그 및 종료 태그: <%@ 지시자 %>

  - JSP 페이지의 전체 속성을 지시

  - 지시자나 속성에 따라 특별한 자바 코드를 생성함

  - 지시자에는 page, taglib, include가 있음

  - <%@ 지시자 속성="값" 속성="값",,,%>

  - JSP 페이지와 관련된 속성을 정의할 때 사용하는 태그

  - language 속성: 스크립트릿이나 표현식, 선언부를 작성할 때 사용할 프로그래밍 언어를 지정(대소문자 구분)

  - contentType 속성: 출력할 데이터의 MIME 타입과 문자 집합을 지정

    - text/html은 출력할 데이터가 HTML 텍스트임을 가리킴

  - pageEncoding 속성: 출력할 데이터의 문자 집합을 지정(기본값 ISO-8859-1)
  ```

  <br />

  - 스크립트릿(Scriptlet Element)

  ```
  - 시작 태그 및 종료 태그: <% %>

  - 사용할 스크립트 언어를 설정(Java 포함)

  - JSP 페이지 안에 자바 코드를 넣을 때 사용

  - 스크립트릿 태그 안에 작성한 내용은 서블릿 파일을 만들 때 그대로 복사됨

  ```

  <br />

  - JSP 기본 객체(Implicit Object)

  ```
  - request, response, pageContext, session, application, config, out,
    page, exception 객체가 존재

  - 별도의 선언 없이 바로 사용 가능

  ```

  <br />

  - 선언문(Declaration)

  ```
  - 시작 태그 및 종료 태그: <%! %>

  - JSP 페이지에서 사용되는 변수와 메서드 선언

  - 선언문은 서블릿 클래스의 멤버를 선언할 때 사용하는 태그

  - 선언문의 위치는 어디든 상관없음(사용 안 함)
  ```

  <br />

  - 표현식(Expression)

  ```
  - 시작 태그 및 종료 태그: <%= %>

  - 변수의 값이나 메서드의 결과를 출력

  - 문자열을 출력할 때 사용 => 표현식 안에는 결과를 반환하는 자바 코드가 와야 함

  - ex) <%=문자열을 반환하는 자바 코드 %>
  ```

  <br />

  - 주석(comment)

  ```
  - 한 줄 주석: <% //한 줄 주석 %>

  - 여러 줄 주석: <%-- 여러 줄 주석 --%>
  ```

<br />

- JSP 전용 문법 배우기 1

  ```html
  <!-- jsp -->
  <% page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>JSP 전용 문법 배우기 1</title>
      <style></style>
      <script></script>
    </head>
    <% int n = 10; String greetStr = "아침이다~"; %>
    <body>
      <h1><%=greetStr %></h1>
      <p><%=n + 20%></p>
    </body>
  </html>
  ```

<br />

- JSP 전용 문법 배우기 2

  - expression.jsp

  ```html
  <!-- expression.jsp -->
  <% page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>JSP 전용 문법 배우기 2</title>
      <style></style>
      <script></script>
    </head>
    <%
      int n = 10;
      String greetStr = "아침이다~";
    %>
    <body>
      <h2>객체 다루기</h2>
      <h1><%=greetStr %></h1>
      <p><%=n + 20%></p>

      <form action="./backEnd" method-"post">
        <label>책 이름</label>
        <input name="bookName" />
        <label>호텔 번호</label>
        <input type="number" value="hotelNum" />

        <input type="submit" value="백단에 데이터 전송" />
      </form>
    </body>
  </html>
  ```

  <br />

  - HotelPark.java

  ```java
  package tj.edu;

  import jakarta.servlet.ServletException;

  public class HotelPark extends HttpServlet {
    private static final long serialVersionUID = 1L;

    public HotelPark() {
      super();
    }

    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      res.getWriter().append("Served at: ").append(req.getContextPath());
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      String bookName = req.getParameter("bookName");
      String hotelNum = req.getParameter("hotelNum");

      System.out.println(bookName + 10);
      System.out.println(hotelNum + 10);

      // String n = (String) req.getAttribute("n");

      // Object obj = req.getAttribute("greetStr");
      // String greetStr = (String) obj;

      // System.out.println(n);
      // System.out.println(greetStr);
    }
  }
  ```

  <br />

- JSP 전용 문법 배우기 번외

  - expression2.jsp

  ```html
  <!-- expression2.jsp -->
  <% page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>JSP 전용 문법 배우기 2</title>
      <style></style>
      <script></script>
    </head>
    <%
      int n = 10;
      String greetStr = "아침이다~";

      // setAttribute 실행 예시
      Request"Dispatcher dispatcher = request.getRequestDispatcher("/backEnd");
      dispatcher.forward(request, response)
    %>
    <body>
      <h2>객체 다루기</h2>
      <h1><%=greetStr %></h1>
      <p><%=n + 20%></p>

      <form action="./backEnd" method-"post">
        <label>책 이름</label>
        <input name="bookName" />
        <label>호텔 번호</label>
        <input type="number" value="hotelNum" />

        <input type="submit" value="백단에 데이터 전송" />
      </form>
    </body>
  </html>
  ```

  <br />

  - HotelPark.java

  ```java
  package tj.edu;

  import jakarta.servlet.ServletException;

  public class HotelPark extends HttpServlet {
    private static final long serialVersionUID = 1L;

    public HotelPark() {
      super();
    }

    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      doPost(req, res);
    }

    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      String bookName = req.getParameter("bookName");
      String hotelNum = req.getParameter("hotelNum");

      System.out.println(bookName + 10);
      System.out.println(hotelNum + 10);

      int n = (Integer) req.getAttribute("n");

      Object obj = req.getAttribute("greetStr");
      String greetStr = (String) obj;

      System.out.println(n);
      System.out.println(greetStr);
    }
  }
  ```

<br />

- 외부 JSP를 불러와 사용(header, footer)

  > MVC 패턴에서 사용한 MemberDto class 그대로 사용

  > 자세한 전체 코드는 https://github.com/kuk0901/JSP_Servlet_practice 에서 확인

  - MemberListView.jsp

  ```html
  <!-- MemberListView.jsp -->
  <%@ page import="spms.dto.MemberDto"%>
  <%@ page import="java.util.ArrayList"%>
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8">
      <title>Insert title here</title>
      <style>
      a {
        text-decoration: none;
        color: black;
      }

      a:hover {
        text-decoration: underline;
        color: #86eb86;
      }
      </style>

      <script></script>
    </head>
    <body>
      <jsp:include page="/Header.jsp"/>

      <h1>회원 목록</h1>
      <p>
        <a href="./add">신규 회원 등록</a>
      </p>

      <%
        ArrayList<MemberDto> memberList = (ArrayList<MemberDto>)request.getAttribute("memberList");
        for(MemberDto memberDto : memberList) {
      %>

      <%=memberDto.getMemberNo() %>,
      <a href='./update?memberNo=<%=memberDto.getMemberNo()%>'>
        <%=memberDto.getMemberName()%>
      </a>,
      <%=memberDto.getEmail()%>,
      <%=memberDto.getCreatedDate()%>,
      <a href='./delete?memberNo=<%=memberDto.getMemberNo()%>'>[삭제]</a>
      <br>

      <%
        } // for end
      %>

      <jsp:include page="/Footer.jsp"/>
    </body>
  </html>
  ```

  <br />

  - MemberListServlet.java

  ```java
  package spms.servlets;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.ArrayList;
  import java.util.Date;

  import jakarta.servlet.RequestDispatcher;
  import jakarta.servlet.ServletContext;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.annotation.WebServlet;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;
  import spms.dto.MemberDto;

  /**
  * ALT + SHITF + J: API 주석 회원 목록 조회 구현
  *
  */
  @WebServlet(value = "/member/list")
  public class MemberListServlet extends HttpServlet {

    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {

      // JDBC 실행 순서
      // DB 객체 준비
      Connection conn = null;
      PreparedStatement pstmt = null;
      ResultSet rs = null;

      String driver = "";
      String url = "";
      String user = "";
      String password = "";

      // try문 안에 예외처리가 관련이 없더라도
      // 모든 관련 로직은 어떤 문제가 생기는지 알 수 있도록 처리해야 함
      try {

        ServletContext sc = this.getServletContext();

        driver = sc.getInitParameter("driver");
        url = sc.getInitParameter("url");
        user = sc.getInitParameter("user");
        password = sc.getInitParameter("password");

        // 오라클 객체 불러오기
        Class.forName(driver);

        // 드라이브 매니저에 jdbc 등록 -> db 연결 -> db 객체
        conn = DriverManager.getConnection(url, user, password);

        String sql = "";

        sql += "SELECT MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE\r\n";
        sql += "FROM MEMBER\r\n";
        sql += "ORDER BY MEMBER_NO ASC";

        pstmt = conn.prepareStatement(sql);

        // db에 sql문 전달, 실행
        rs = pstmt.executeQuery(sql);

        res.setContentType("text/html");
        res.setCharacterEncoding("UTF-8");

        ArrayList<MemberDto> memberList = new ArrayList<MemberDto>();

        int memberNo = 0;
        String memberName = "";
        String email = "";
        Date creDate = null;

        MemberDto memberDto = null;

        while (rs.next()) {
          memberNo = rs.getInt("MEMBER_NO");
          memberName = rs.getString("MEMBER_NAME");
          email = rs.getString("EMAIL");
          creDate = rs.getDate("CRE_DATE");

          memberDto = new MemberDto();

          memberDto.setMemberNo(memberNo);
          memberDto.setMemberName(memberName);
          memberDto.setEmail(email);
          memberDto.setCreatedDate(creDate);

          memberList.add(memberDto);
        }

        req.setAttribute("memberList", memberList);

        RequestDispatcher dispatcher = req.getRequestDispatcher("/member/MemberListView.jsp");

        dispatcher.include(req, res);

      } catch (ClassNotFoundException e) {

        e.printStackTrace();
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

      } // finally 종료
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {}

  }
  ```

  <br />

  - Header.jsp

  ```html
  <!-- Header.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <div
    style="background-color: #663399; color: #fff; height: 35px; 
    padding: 5px; line-height: 35px; 
    font-weight: 700;"
  >
    SPMS(Simple Project Management System)
  </div>
  ```

  <br />

  - Footer.jsp

  ```html
  <!-- Footer.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <div
    style="
    background-color: #6A5ACD; color: #fff; 
    width: 100vw; height: 30px; padding: 5px; 
    margin-top: 10px; line-height: 30px;"
  >
    SPMS &copy; 2024
  </div>
  ```

  <br />

  - Error.jsp

  ```html
  <!-- Error.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>시스템 오류</title>
      <style></style>
      <script>
        /* 	
        function pageMoveMemberListFnc() {
          location.href = './member/list';
        } 
      */
      </script>
    </head>
    <% String msg1 = (String)request.getAttribute("caseByCase"); %>
    <body>
      <h1>시스템 오류</h1>

      <pre>
        다시 한번 확인해 주세요!
        지금 입력하신 주소의 페이지는 사라졌거나 다른 페이지로 변경되었습니다.
        주소를 다시 확인해 주세요.
        만약 계속해서 이 문제가 발생된다면 
        시스템 운영팀(사내번호: 8282)에 연락하기 바랍니다.
      </pre>

      <div><%= msg1 %></div>

      <!-- <button onclick="pageMoveMemberListFnc();">회원목록으로 이동</button> -->
    </body>
  </html>
  ```
