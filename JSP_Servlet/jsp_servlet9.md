# JSP & Servlet 9일차

## 9일차

- taglib 지시어

  - JSP의 태그 확장 메커니즘인 커스텀 태그를 사용하기 위한 지시어

  - taglib 지시어 구문과 사용 형식

  ```html
  <!-- .jsp 파일 -->
  <% taglib uri="태그 라이브러리 경로" tagdir="태그 파일 경로" prefix="태그
  접두어" %>
  ```

  - uri: 태그 라이브러리 위치로 태그를 정의하고 있는 .tld 파일 경로

  - tagdir: 태그 파일로 태그를 구현한 경우 태그 파일 경로

  - prefix: 해당 태그를 구분해서 사용하기 위한 접두어

  > uri 또는 tagdir

<br />

- JSTL(JSP Standard Tag Library)

  - 자바 코드를 사용하지 않고 HTML 형식을 유지하면서 조건문, 반복문, 간단한 연산과 유용한 기능을 사용할 수 있도록 만들어진 표준 커스텀 태그 라이브러리

  - 규격상 JSTL은 core, xml, 데이터베이스, 함수 등으로 구성되어 있으나 뷰 중심의 JSP 구현인 core 정도만 사용됨

<br />

- EL(Expression Language) 표기법

  - 콤마와 대괄호를 사용하여 자바 빈의 프로퍼티나 맵, 리스트, 배열의 값을 더욱 쉽게 꺼내게 해주는 기술

  - JSP에서는 주로 보관소에 들어 있는 값을 꺼낼 때 사용

  - EL 사용 시 액션 태그를 사용하는 것보다 더 간단히 보관소에 있는 객체에 접근해 값을 꺼내거나 메서드 호출 가능

  - EL표기법

  ```
  객체명.프로퍼티          객체명[프로퍼티]
  ${memberDto.no} 또는 ${memberDto["no]}


  * 보관소를 명시하지 않는 경우
  jspContext -> ServletRequest -> HttpSession -> ServletContext -> null

  순서로 객체를 찾다가 최종적으로 없는 경우 null 반환


  * 보관소 명시 방법
  pageScope, requestScope, sessionScope, applicationScope

  => ${requestScope.memberDto.no}
  ```

  <br />

  - 리터럴 표현식

  ```
  - 문자열: ${'문자열'}

  - 정수: ${10 + 20}

  - 부동소수점: ${3.14 + 0.01}

  - 참 || 거짓: ${true}

  - null: ${null}


  * 연산할 때는 타입을 갖지만 최종적으로는 모두 문자열 타입이 됨
  ```

  - 논리 연산자

  ```
  - true and true: ${true && true}

  - false or true: ${false || true}

  - 10 == 11: ${10 eq 11} || ${10 == 11}

  - 10 != 11: ${10 ne 11} || ${10 != 11}

  - 10 <= 11: ${10 le 11} || ${10 <= 11}

  - 10 < 11: ${10 lt 11} || ${10 < 11}

  - 10 >= 11: ${10 ge 11} || ${10 >= 11}

  - 10 > 11: ${10 gt 11} || ${10 > 11}
  ```

  <br />

  - 기타 표현식

  ```
  - empty: ${empty 객체 명}

  - not empty: ${not empty 객체 명}
  ```

<br />

- JSP 파일 수정

  - JSP 전용 태그(스크립트릿)를 JSTL로 수정

  ```html
  <!-- MemberListView.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%> <%@ taglib uri="http://java.sun.com/jsp/jstl/core"
  prefix="c"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
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
      <jsp:include page="/Header.jsp" />

      <h1>회원 목록</h1>
      <p>
        <a href="./add">신규 회원 등록</a>
      </p>

      <c:forEach var="memberDto" items="${memberList}">
        ${memberDto.memberNo},
        <a href="./update?memberNo=${memberDto.memberNo}">
          ${memberDto.memberName} </a
        >, ${memberDto.email}, ${memberDto.createdDate},
        <a href="./delete?memberNo=${memberDto.memberNo}">[삭제]</a>
        <br />
      </c:forEach>

      <jsp:include page="/Tail.jsp" />
    </body>
  </html>
  ```

<br />

- DAO

  ```
  서블릿으로부터 분리해야 하는 기능: 데이터베이스와 연동하여 데이터를 처리하는 부분
  ```

  - 데이터 처리를 전문으로 하는 객체

  - DB나 파일, 메모리 등을 이용해 애플리케이션 데이터를 CRUD 하는 역할을 수행

  - DAO는 보통 하나의 테이블이나 DB 뷰에 대응함(여러 개의 테이블을 조인한 데이터를 다루기도 함)

  ```
  업무 로직에서 데이터 처리 부분을 분리해 별도의 객체로 정의하면,
  여러 업무에서 공통으로 사용할 수 있기 때문에 유지보수가 쉬워지고 재사용성이 높아짐
  ```

  <br />

  - MemberDao.java

  ```java
  // MemberDao.java
  package spms.dao;

  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;

  import java.util.ArrayList;
  import java.util.Date;
  import java.util.List;

  import spms.dto.MemberDto;

  public class MemberDao {
    private Connection connection;

    public void serConnection(Connection conn) {
      this.connection = conn;
    }

    public List<MemberDto> selectList() throws Exception {
      PreparedStatement pstmt = null;
      ResultSet rs = null;

      try {
        String sql = "";

        sql += "SELECT MEMBER_NO, EMAIL, PWD, MEMBER_NAME, CRE_DATE\r\n";
        sql += "FROM MEMBER\r\n";
        sql += "ORDER BY MEMBER_NO ASC";

        pstmt = connection.prepareStatement(sql);

        // db에 sql문 전달, 실행
        rs = pstmt.executeQuery(sql);

        int memberNo = 0;
        String memberName = "";
        String email = "";
        Date creDate = null;

        ArrayList<MemberDto> memberList = new ArrayList<>();

        MemberDto memberDto = null;

        while (rs.next()) {
          memberNo = rs.getInt("MEMBER_NO");
          email = rs.getString("EMAIL");
          memberName = rs.getString("MEMBER_NAME");
          creDate = rs.getDate("CRE_DATE");

          memberDto = new MemberDto(memberNo, email, memberName, creDate);

          memberList.add(memberDto);
        }

        return memberList;
      } catch (Exception e) {
        e.printStackTrace();
        throw e;
      } finally {
        // db 객체 메모리 해제
        try {
          if (rs != null) {
            rs.close();
          }
        } catch (SQLException e) {
          e.printStackTrace();
        }

        try {
          if (pstmt != null) {
            pstmt.close();
          }
        } catch (SQLException e) {
          e.printStackTrace();
        }
      } // finally 종료
    }
  }
  ```

  <br />

  - MemberListServlet.java

  ```java
  //  MemberListServlet.java
  package spms.servlets;

  import java.io.IOException;
  import java.sql.Connection;
  import java.sql.SQLException;

  import java.util.ArrayList;

  import jakarta.servlet.RequestDispatcher;
  import jakarta.servlet.ServletContext;
  import jakarta.servlet.ServletException;
  import jakarta.servlet.annotation.WebServlet;
  import jakarta.servlet.http.HttpServlet;
  import jakarta.servlet.http.HttpServletRequest;
  import jakarta.servlet.http.HttpServletResponse;

  import spms.dao.MemberDao;
  import spms.dto.MemberDto;

  /**
  * ALT + SHITF + J: API 주석
  * 회원 목록 조회 구현
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

      try {
        ServletContext sc = this.getServletContext();

        // 미리 준비된 DB 객체 불러오기
        conn = (Connection) sc.getAttribute("conn");

        MemberDao memberDao = new MemberDao();

        memberDao.serConnection(conn);

        ArrayList<MemberDto> memberList = (ArrayList<MemberDto>) memberDao.selectList();

        req.setAttribute("memberList", memberList);
        res.setContentType("text/html");
        res.setCharacterEncoding("UTF-8");

        RequestDispatcher dispatcher = req.getRequestDispatcher("/member/MemberListView.jsp");

        dispatcher.include(req, res);
      } catch (SQLException e) {
        e.printStackTrace();
      } catch (Exception e) {
        e.printStackTrace();

        req.setAttribute("error", e);
        req.setAttribute("caseByCase", "상황에 맞는 처리 부탁");

        RequestDispatcher dispatcher = req.getRequestDispatcher("/Error.jsp");

        dispatcher.forward(req, res);
      }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {}
  }
  ```

<br />

- MVC 패턴 적용

  ```
  client -- request -> controller --> DAO --> DB

  1. controller -> DAO -> DB: CRUD

  => DAO에서 DB 연결 후 CRUD를 실행


  2. DB -> DAO = model

  => DB에서 CRUD 이후 model을 request에 담음


  3. DAO -> controller = request에 담은 model을 반환


  4. controller: request page(view(JSP))

  => request에 담긴 model을 이용해 요청 페이지(view)를 생성 === JSP


  5. controller -> client

  => 요청 페이지 반환(response)


  * DAO를 통해 DB 작업 실행하고 request에 model(data)을 담음
    controller에서 받은 model을 기반으로 page 생성 및 반환
  ```

  <br />

  - 사용자 정보를 컨트롤러에서 모아서 DAO에서 어떻게 처리할까

    1. 매개변수로 직접 전달

    2. 객체(모델) 생성 후 세터를 이용해 데이터를 저장(가공처리)한 다음, DAO를 호출

  - 메서드의 리턴 값을 이용해 화면 구성을 다르게 할 수 있음

  - executeUpdate() 메서드는 실행된 row count 숫자를 반환 => 0일 경우 추가한 값이 없다는 뜻

<br />

- MVC 패턴 개발 순서

  ```
  DB -> DTO, VO -> DAO -> Controller -> View
  ```

<br />

- CRUD 구현 순서

  ```
  전체 조회 -> 개인 조회 -> 추가 -> 수정 -> 삭제
  ```
