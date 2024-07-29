# JSP & Servlet 8일차

## 8일차

- 액션 태그(Action)

  - JSP 액션: JSP에서 기본적으로 제공하는 태그들의 집합

  - JSP에서 객체 생성과 공유, 페이지 이동과 전달, 태그 파일 작성 등에 필요한 기능을 제공하는 일종의 커스텀 태그

  > 표준 액션 태그라도 불리며 JSP 접두어를 사용함

  ```
  - JSP 페이지를 작성할 때, 가능한 자바 코드의 삽입을 최소화하는 것이 유지 보수에 좋음

  - 이를 위해 JSP에서는 다양한 JSP 전용 태그를 제공

  - JSP 액션을 사용하면 자바로 직접 코딩하는 것보다 빠르고 쉽게 원하는 기능을 작성할 수 있음
  ```

<br />

- 자바 빈(bean)

  - 자바의 재활용 가능한 컴포너트 모델을 말하는 것으로, 웹 개발에만 국한된 개념이 아니며 POJO라고 하는 단순한 구조를 가짐

  - 특정 기술이나 프레임워크에 종속하지 않고 기본 생성자와 멤버 변수에 대한 getter / setter 메서드를 제공하고 직렬화할 수 있는 자바 클래스를 의미함

  ```
  - 객체지향 프로그래밍의 핵심 컨셉은 컴포넌트 모델로, 프로그램 모듈화를 위해 컴포넌트를 사용함

  - 따라서 컨테이너 기반으로 운영되는 시스템에서는 여러 객체를 활용하기 위하여 단순하면서도 정형화된 구조를 통해 동일한 방식으로 객체를 다루는 방법을 제공할 수 있어야 함
  ```

<br />

- 자바 빈 구조

  ```
  1. 인자가 없는 생성자로 구성

  2. 파일 혹은 네트워크를 통해 객체를 주고받을 수 있는 직렬화 구조가 가능

  3. getter / setter 메서드를 통해 멤버 변수에 접근(기본 접근 private)
  ```

<br />

- useBean

  - JSP에서 자바 빈 객체를 생성하거나 참조하기 위한 액션

  - 속성

    ```
    - .id: 자바 빈을 특정 scope에 저장하거나 가져올 때 사용하는 이름(변수명),
           현재 페이지에서는 해당 인스턴스를 참조하기 위한 변수명이 됨

           => servlet의 request.setAttribute(키, 밸류)에서의 키와 동일해야 함

    - .scope: 해당 클래스 타입의 객체를 저장하거나 가지고 오는 범위, 데이터 보관소를 지정

    - .class: 생성하거나 참조하려는 객체의 클래스 명으로, 반드시 패키지 명까지 명시해야 함
              (추상 클래스나 인터페이스는 사용 불가)

              => id 값이 없으면 클래스 풀네임에 맞춰 생성

    - .type: 특정 타입의 클래스를 명시할 때 사용,
             추상 클래스 / 인터페이스 / 일반 클래스가 될 수 있으며
             class 속성의 클래스에서 상속 혹은 구현이 이뤄져야 함


    * 일반적으로는 id, scope, class만 작성
    ```

<br />

- useBean action 사용

  ```html
  <!-- Header.jsp -->
  <%@ page import="spms.dto.MemberDto"%> <%@ page language="java"
  contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  <jsp:useBean id="memberDto" scope="session" class="spms.dto.MembetDto" />
    <!-- <% MemberDto sessionMemberDto = (MemberDto) session.getAttribute("memberDto");%> -->

    <div
      style="background-color: #663399; color: #fff; height: 35px; padding: 5px; line-height: 35px; font-weight: 700;"
    >
      SPMS(Simple Project Management System)
      <span style="float: right;">
        <%=memberDto.getMemberName()%>
        <!-- 공통단의 코드들은 가능한 절대 경로로 설정해야 어떤 구조에서든 동작 -->
        <a
          style="color: #86eb86;"
          href="<%=request.getContextPath()%>/auth/logout"
          >(로그아웃)</a
        >
      </span>
    </div></jsp:useBean
  >
  ```

  ```html
  <!-- MemberListView.jsp -->
  <%@ page import="spms.dto.MemberDto"%> <%@ page import="java.util.ArrayList"%>
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>
      <style></style>
      <script></script>
    </head>
    <body>
      <p>
        <a href="./add">신규 회원 등록</a>
      </p>
      <jsp:useBean
        id="memberList"
        scope="request"
        class="java.util.ArrayList"
        type="java.util.ArrayList<spms.dto.MemberDto>"
      />
      <%
        java.util.ArrayList<MemberDto> memberList = (ArrayList<MemberDto>) request.getAttribute("memberList");

          if (memberList == null) {
            memberList = new ArrayList<spms.dto.MemberDto>();
              request.setAttribute("memberList", memberList);
          }
      %>
      <% for(MemberDto memberDto : memberList) { %> <%=memberDto.getMemberNo()
      %>,
      <a href="./update?memberNo=<%=memberDto.getMemberNo()%>">
        <%=memberDto.getMemberName()%> </a
      >, <%=memberDto.getMemberName()%>, <%=memberDto.getEmail()%>,
      <%=memberDto.getCreatedDate()%>,
      <a href="./delete?memberNo=<%=memberDto.getMemberNo()%>">[삭제]</a>
      <br />
      <% } // for end %>
    </body>
  </html>
  ```

<br />

- useBean 연습1

  ```html
  <!-- Action1.jsp -->
  <%@ page import="tj.edu.MemberDto" %> <%@ page language="java"
  contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Action:useBean1</title>
    </head>
    <% request.setAttribute("tempNum1", 100); int tempNum1 = (int)
    request.getAttribute("tempNum1"); int tempNum1 = (int)
    request.getAttribute("tempNum1"); MemberDto member = new MemberDto();
    request.setAttribute("memberBean", member); %>

    <jsp:useBean id="memberBean" scope="request" class="tj.edu.MemberDto" />

    <jsp:setProperty name="memberBean" property="memberNo" value="1000" />
    <jsp:setProperty name="memberBean" property="email" value="test@test.com" />
    <jsp:setProperty name="memberBean" property="password" value="1234" />
    <jsp:setProperty name="memberBean" property="memberName" value="user1" />
    <body>
      <%= tempNum1 %> <%= memberBean %> <%= memberBean %>
      <br />
      <%= memberBean.getMemberName() %>
      <jsp:getProperty name="memberBean" property="memberName" />
    </body>
  </html>
  ```

<br />

- useBean 연습2

  ```html
  <!-- Action2.jsp -->
  <%@ page import="tj.edu.MemberDto" %> <%@ page language="java"
  contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Action:useBean2</title>
    </head>

    <jsp:useBean id="memberBean" scope="request" class="tj.edu.MemberDto" />

    <jsp:setProperty name="memberBean" property="memberNo" />
    <jsp:setProperty name="memberBean" property="email" />
    <jsp:setProperty name="memberBean" property="password" />
    <jsp:setProperty name="memberBean" property="memberName" />
    <!-- <jsp:setProperty name="memberBean" property="createdDate" />
    <jsp:setProperty name="memberBean" property="modifiedDate" /> -->
    <body>
      <jsp:getProperty name="memberBean" property="memberNo" />
      <jsp:getProperty name="memberBean" property="memberName" />
    </body>
  </html>
  ```

  ```html
  <!-- Action2_Form.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Action:userBean2 form 예제</title>
    </head>
    <body>
      <form action="./Action2.jsp" method="get">
        <input type="number" name="memberNo" value="1" />
        <input type="text" name="password" value="test@test.com" />
        <input type="text" name="email" value="qwer" />
        <input type="text" name="memberName" value="저하늘의 별" />
        <!-- <input type="date" name="createdDate" value="" />
        <input type="date" name="modifiedDate" value="" /> -->

        <input type="submit" value="회원정보" />
      </form>
    </body>
  </html>
  ```

<br />

- useBean 연습3

  - MemberDto의 date 타입을 전부 String으로 바꿔서 사용

  ```html
  <!-- Action2.jsp -->
  <%@ page import="tj.edu.MemberDto" %> <%@ page language="java"
  contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Action:useBean2</title>
    </head>

    <jsp:useBean id="memberBean" scope="request" class="tj.edu.MemberDto" />

    <jsp:setProperty name="memberBean" property="*" />

    <body>
      <jsp:getProperty name="memberBean" property="memberNo" />
      <jsp:getProperty name="memberBean" property="email" />
      <jsp:getProperty name="memberBean" property="password" />
      <jsp:getProperty name="memberBean" property="memberName" />
      <jsp:getProperty name="memberBean" property="createdDate" />
      <jsp:getProperty name="memberBean" property="modifiedDate" />
    </body>
  </html>
  ```

  ```html
  <!-- Action2_Form.jsp -->
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" %>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Action:userBean2 form 예제</title>
    </head>
    <body>
      <form action="./Action2.jsp" method="get">
        <input type="number" name="memberNo" value="1" />
        <input type="text" name="password" value="test@test.com" />
        <input type="text" name="email" value="qwer" />
        <input type="text" name="memberName" value="저하늘의 별" />
        <input type="date" name="createdDate" value="" />
        <input type="date" name="modifiedDate" value="" />

        <input type="submit" value="회원정보" />
      </form>
    </body>
  </html>
  ```
