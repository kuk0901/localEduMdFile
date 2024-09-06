# Spring 9일차

## **이론**

- **`프레임워크`**

  - 사전적 의미: 뼈 혹은 틀을 의미

  - 소프트웨어 관점

    - 애플리케이션의 골격에 해당하는 아키텍처를 제공

    - 프레임워크 기반의 애플리케이션을 개발하면 개발자는 비즈니스 로직에 집중할 수 있음

<br />

- 오픈 소스 프레임워크

  - 레이어별 가장 많이 사용하는 자바 기반 프레임워크

  - 스프링의 IoC AOP 모듈을 이용해 스프링 컨테이너에서 동작하는 비즈니스 컴포넌트를 개발함

  - JPA: 하이버네이트를 비롯한 모든 ORM(Object Relation Mapping) 프레임워크 표준

  - MyBatis: XML 파일에 작성한 SQL을 자바 객체와 맵핑해 주는 데이터 맵퍼 프레임워크

<br />

- 오픈 소스 프레임워크의 장점

  ```
  1. 빠른 구현 시간

  2. 관리의 용이성 증가

  3. 개발자들의 역량 획일화

  4. 검증된 아키텍처의 재사용

  5. 아키텍처의 일관성 유지
  ```

<br />

- 스프링 부트의 등장

  ```
  - 스프링의 복잡도 증가

  - 본래 웹 애플리케이션을 목적으로 만든 것이 아님

  - 하지만 대부분 웹 애플리케이션 개발 시 스프링을 사용하는 현상 발생

  => 웹 애플리케이션이 복잡해지면서 원래 목적인 비즈니스 로직에 집중하지 못하게 됨
      (라이브러리 관리 및 XML 환경 설정)

  * 스프링 부트는 스프링 프레임워크와 달리 웹 애플리케이션을 목적으로 함

    - 스프링처럼 많은 설정이 필요하지 않음

      => 개발자들의 쉽게 접근 가능

    - 커맨트 도구를 제공하고 톰캣 같은 내장 서버를 통해, 서버와 관련된 복잡한 설정들을 제거

      => 스프링을 처음 사용하는 개발자도 애플리케이션 관련 설정을 쉽게 처리,
          관리함으로써 개발 자체에 집중 가능
  ```

<br />

- 스프링 부트의 특징

  1. 라이브러리 관리 자동화

     - 메이븐을 기반으로 하는 스타터(Starter)를 통해 애플리케이션이 필요한 라이브러리 의존성 문제 해결

  <br />

  2. 설정의 자동화

     - 추가된 라이브러리와 환경 설정을 자동으로 처리

     ```
     JPA를 사용하기 위해 JPA 스타터 추가 시 스프링 부트가 라이브러리에 포함된
     자동설정 클래스를 인지해 JPA 사용에 필요한 객체들을 자동으로 생성
     ```

  <br />

  3. 라이브러리 버전 자동 관리

     - 스프링 프로젝트에서는 스프링 라이브러리뿐만 아니라 서드파티 라이브러리도 사용, 함께 다운로드

  <br />

  4. 테스트 환경과 내장 서버

     - 기본적으로 jUnit을 비롯한 테스트 관련 라이브러리들을 포함

     - 다양한 계층의 테스트 케이스를 쉽게 작성 가능

     - 톰캣 같은 웹 서버 내장

  <br />

  5. 독립적으로 실행할 수 있는 jar

     - 애플리케이션을 실제 운영 서버에 배포하기 위해서는 패키징을(packaging)을 해야 함

     ```
     웹 애플리케이션 -> WAR(Web Archive) 파일로 패키징

     스프링 부트 => 웹 애플리케이션도 JAR 파일로 패키징 가능(빠른 개발과 배포)
     ```

<br />

- 스프링 부트 프로젝트 구조 이해

  - 소스 폴더

    1. src/main/java

       - 일반적인 자바 클래스 작성

    2. src/main/resources

       - 자바가 아닌 파일(XML 또는 Properties) 작성

    3. src/test/java

       - jUnit 기반의 테스트 코드 작성

    4. static 폴더

       - 이미지나 html 같은 정적인 웹 콘텐츠 저장

    5. templates 폴더

       - 타임리프(Thymeleaf) 같은 템플릿 기반의 뤱 리소스 저장

    6. application.properties 파일

       - 프로젝트 전체에서 사용할 프로퍼티 정보 저장

    7. Maven Dependencies

       - maven에 의해 프로젝트 Path에 등록된 라이브러리 목록

       - pom.xml 파일에 디펜던시를 추가해 라이브러리 관리

    8. 라이브러리 다운로드 물리적 위치

       - 윈도우: c:Users\컴퓨터명\\.m2\repository

<br />

- 스프링 부트 설정(Properties, yaml) 이해

  - 스프링 부트는 애플리케이션에서 사용하는 설정 정보를 외부 프로퍼티로 분리시킴으로써 자바 소스 수정을 최소화함

  - 애플리케이션의 환결을 관리하는 설정 파일

<br />

- **`yaml 파일`**

  - 야믈, 야물이라고도 불림

  - XML, JSON과 마찬가지로 데이터의 의미와 구조를 쉽게 전달하기 위한 파일

  - 기존의 XML, JSON보다 쉽게 작성할 수 있고, 가독성이 뛰어남

  - properties 설정을 yaml 파일로 대체할 수 있음

  - src/main/resources 소스 폴더에 있는 application.properties 파일 제거 후 yaml 파일로 대체할 수 있음(들여쓰기 주의)

<br />

- **`REST 컨트롤러`**

  - @RequestController를 사용하여 REST 컨트롤러 구현

  - 컨트롤러 클래스는 반드시 main 클래스의 하위 패키지에 위치해야 함

<br />

- 컨트롤러 빈 등록

  - @GetMapping은 Get 방식의 요청을 처리

  - 리턴한 데이터를 브라우저에 그대로 전달하기 때문에 별도로 View 화면을 만들 필요가 없음

<br />

- ajax를 이용한 freeBoard 추가

  - Header.JSP

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%> <%@ taglib uri="http://java.sun.com/jsp/jstl/core"
  prefix="c"%>

  <script>
    function pageMoveMemberListFnc() {
      location.href = "/member/list";
    }

    function pageMoveFreeboardListFnc() {
      location.href = "/freeBoard/list";
    }
  </script>

  <div
    style="background-color: #dd7c73; color: #fff; height: 20px; padding: 5px"
  >
    SPMS(Simple Project Management System)

    <span
      style="border: 1px solid greenyellow; color: greenyellow; cursor: pointer"
      onclick="pageMoveMemberListFnc();"
      >회원</span
    >

    <span
      style="border: 1px solid greenyellow; color: greenyellow; cursor: pointer"
      onclick="pageMoveFreeboardListFnc();"
      >자유게시판</span
    >

    <c:if test="${sessionScope.member.email ne null}">
      <span style="float: right; text-align: right">
        ${member.memberName}

        <input id="inputMemberNo" type="hidden" value="${member.memberNo}" />

        <!-- 공통단의 코드들은 가능한 절대 경로로 설정해야 어떤 구조에서든 동작 -->

        <a style="color: #86eb86" href="${sessionScope.rootPath}/member/logout"
          >(로그아웃)</a
        >
      </span>
    </c:if>
  </div>
  ```

  <br />

  - FreeBoardListView.jsp

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%> <%@ taglib uri="http://java.sun.com/jsp/jstl/core"
  prefix="c"%>

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>자유게시판 목록</title>

      <style type="text/css">
        table,
        tr,
        th,
        td {
          border: 1px solid black;

          border-collapse: collapse;
        }

        tr > th {
          background-color: gray;
        }

        .aTagStyle {
          cursor: pointer;
        }

        .aTagStyle:hover {
          color: lightgreen;

          background-color: gray;
        }
      </style>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script type="text/javascript">
        // jQuery

        $(function () {
          $("#aFreeBoardInsert").on("click", function (event) {
            const myObj = $(this);

            event.preventDefault(); // 이벤트의 기본 동작을 막음

            const containerTag = $("#container");

            let htmlStr = "";

            htmlStr += '<table style="width: 1000px;">';
            htmlStr += "<tr>";
            htmlStr += '<td class="tableSubject">주제</td>';
            htmlStr += '<td style="width: 735px;">';
            htmlStr +=
              '<input type="text" id="freeBoardTitle" name="freeBoardTitle" value="" size="100px" />';
            htmlStr += "</td>";
            htmlStr += "</tr>";

            htmlStr += "<tr>";
            htmlStr += '<td style="width: 980px;" colspan="2">';
            htmlStr +=
              '<textarea id="freeBoardContent" name="freeBoardContent" rows="10" cols="100" style="width: 990px;">';
            htmlStr += "</textarea>";
            htmlStr += "</td>";
            htmlStr += "</tr>";

            htmlStr += "</table>";

            htmlStr += "<div>";
            htmlStr += "<span>";
            htmlStr +=
              '<button onclick="pageMoveFreeBoardListFnc();">이전페이지</button>';
            htmlStr +=
              '<button id="btnFreeBoardInsert">수정페이지로 이동</button>';
            htmlStr += "</span>";
            htmlStr += "</div>";

            containerTag.html(htmlStr);
          });

          // 동적 이벤트 등록

          $("#container").on("click", "#btnFreeBoardInsert", function (event) {
            const myObj = $(this);

            const inputMemberNoTag = $("#inputMemberNo");
            const freeBoardTitleTag = $("#freeBoardTitle");
            const freeBoardContentTag = $("#freeBoardContent");

            const jsonDataObj = {
              freeBoardId: 0,
              memberNo: inputMemberNoTag.val(),
              freeBoardTitle: freeBoardTitleTag.val(),
              freeBoardContent: freeBoardContentTag.val(),
              createDate: null,
              updateDate: null
            };

            $.ajax({
              url: "/freeBoard/",
              method: "POST",
              contentType: "application/json",
              data: JSON.stringify(jsonDataObj),
              dataType: "json",

              success: function (data) {
                alert(data);
                location.href = "./list";
              },

              error: function (xhr, status) {
                alert(xhr.status);
                alert(status);
              }
            }); // ajax end
          });
        }); // onload

        function pageMoveFreeBoardDetailFnc(tableTdTag) {
          let parentTr = tableTdTag.parentNode;
          let freeBoardIdStr = parentTr.children[0].textContent;
          let userSelectFreeBoardIdObj = document.getElementById(
            "userSelectFreeBoardId"
          );

          userSelectFreeBoardIdObj.value = freeBoardIdStr;

          let freeBoardListFormObj =
            document.getElementById("freeBoardListForm");
          freeBoardListFormObj.submit();
        }
      </script>
    </head>

    <body>
      <jsp:include page="/WEB-INF/views/Header.jsp" />

      <div id="container">
        <h1>자유게시판 목록</h1>

        <p>
          <a id="aFreeBoardInsert" href="#">자유게시판 글쓰기</a>
        </p>

        <table>
          <tr>
            <th>번호</th>
            <th>주제</th>
            <th>작성자</th>
            <th>생성날짜</th>
            <th>수정날짜</th>
            <th>비고[삭제]</th>
          </tr>

          <c:forEach var="freeBoardVo" items="${freeBoardList}">
            <tr>
              <td>${freeBoardVo.freeBoardId}</td>
              <td class="aTagStyle" onclick="pageMoveFreeBoardDetailFnc(this);">
                ${freeBoardVo.freeBoardTitle}
              </td>
              <td>${freeBoardVo.memberName}</td>
              <td>${freeBoardVo.createDate}</td>
              <td>${freeBoardVo.updateDate}</td>
              <td style="text-align: center">[삭제]</td>
            </tr>
          </c:forEach>
        </table>

        <jsp:include page="/WEB-INF/views/common/Paging.jsp">
          <jsp:param value="${pagingMap}" name="pagingMap" />
        </jsp:include>

        <form id="pagingForm" action="./list" method="post">
          <input
            type="hidden"
            id="curPage"
            name="curPage"
            value="${pagingMap.pagingVo.curPage}"
          />
        </form>
      </div>

      <jsp:include page="/WEB-INF/views/Tail.jsp" />

      <form id="freeBoardListForm" action="./list" method="post">
        <input
          id="userSelectFreeBoardId"
          type="hidden"
          name="freeBoardId"
          value=""
        />
      </form>
    </body>
  </html>
  ```

  <br />

  - FreeBoardController.java

  ```java
  package com.edu.freeBoard.controller;

  import java.util.HashMap;
  import java.util.List;
  import java.util.Map;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.http.ResponseEntity;
  import org.springframework.web.bind.annotation.PostMapping;
  import org.springframework.web.bind.annotation.RequestBody;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestMethod;
  import org.springframework.web.bind.annotation.RequestParam;
  import org.springframework.web.bind.annotation.RestController;
  import org.springframework.web.servlet.ModelAndView;

  import com.edu.freeBoard.domain.FreeBoardVo;
  import com.edu.freeBoard.service.FreeBoardService;
  import com.edu.util.Paging;

  @RestController
  @RequestMapping("/freeBoard")
  public class FreeBoardController {

    private Logger log = LoggerFactory.getLogger(FreeBoardController.class);
    private final String logTitleMsg = "==FreeBoardController==";

    @Autowired
    private FreeBoardService freeBoardService;

    // List
    @RequestMapping(value = "/list", method = { RequestMethod.GET, RequestMethod.POST })
    public ModelAndView freeBoardList(@RequestParam(defaultValue = "1") int curPage) {
      log.info(logTitleMsg);
      log.info("@RequestMapping freeBoardList curPage: {}", curPage);

      int totalCount = freeBoardService.freeBoardSelectTotalCount();

      Paging pagingVo = new Paging(totalCount, curPage);
      int start = pagingVo.getPageBegin();
      int end = pagingVo.getPageEnd();

      List<FreeBoardVo> freeBoardList = freeBoardService.freeBoardSelectList(start, end);

      Map<String, Object> pagingMap = new HashMap<>();
      pagingMap.put("totalCount", totalCount);
      pagingMap.put("pagingVo", pagingVo);

      ModelAndView mav = new ModelAndView("freeBoard/FreeBoardListView");
      mav.addObject("freeBoardList", freeBoardList);
      mav.addObject("pagingMap", pagingMap);

      return mav;
    }

    // ajax
    @PostMapping("/")
    public ResponseEntity<Map<String, String>> freeBoardInsertCtr(@RequestBody FreeBoardVo freeBoardVo) {
      log.info(logTitleMsg);
      log.info("@PostMapping freeBoardInsertCtr freeBoardVo: {}", freeBoardVo);

      freeBoardService.freeBoardInsertOne(freeBoardVo);

      Map<String, String> jsonMap = new HashMap<String, String>();
      jsonMap.put("result", "success");

      return ResponseEntity.ok(jsonMap);
    }
  }

  ```
