# Spring 2일차

## **이론**

- **`어노테이션`**

  ```
  @Service
  @Controller

  등 해당 역할에 맞는 어노테이션 작성
  ```

<br />

- **`application.properties`**

  ```properties
  spring.application.name=SpringBootBasic

  spring.main.web-application-type=servlet
  server.port=8888
  # encoding
  server.servlet.encoding.charset=UTF-8
  server.servlet.encoding.force=true
  server.servlet.context-path=/

  # DB
  spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
  spring.datasource.url=jdbc:oracle:thin:@localhost:1521:xe
  spring.datasource.username=edu
  spring.datasource.password=edu12

  mabtis.config-location=classpath:mybatis-config.xml

  spring.jpa.show-sql=true
  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.properties.hibernate.format_sql=true
  spring.jpa.properties.enable_lazy_load_no_trans=true

  spring.mvc.view.prefix=/WEB-INF/views/
  spring.mvc.view.suffix=.jsp
  ```

<br />

- **`logback.xml`**

  - 디버깅을 위한 설정 작성

<br />

- **`controller`**

  - MemberController

  ```java
  package com.edu.member.controller;

  import java.util.List;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RequestMapping;

  import com.edu.member.domain.MemberVo;
  import com.edu.member.service.MemberService;

  @RequestMapping("/member") // url 맵핑
  @Controller // 컨트롤러 annotation
  public class MemberController {
    // MemberController에 대한 모든 logger 확인 가능
    private Logger log = LoggerFactory.getLogger(MemberController.class);
    private final String logTitleMsg = "==MemberController==";

    // 자동으로 spring bean 객체 생성
    @Autowired
    private MemberService memberService;

    // class 영역에서의 url과 메서드 영역에서의 url을 합친 최종 url
    @GetMapping("/getMemberList")
    // Model => 모델
    public String getMemberList(Model model) {
      log.info(logTitleMsg);
      log.info("getMemberList");

      List<MemberVo> memberList = memberService.memberSelectList();

      model.addAttribute("memberList", memberList);

      return "member/MemberListView";
    }
  }
  ```

<br />

- **`service`**

  - MemberServiceImpl

  ```java
  package com.edu.member.service;

  import java.util.List;

  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;

  import com.edu.member.dao.MemberDao;
  import com.edu.member.domain.MemberVo;

  //member와 관련된 모든 비즈니스 로직 처리 => interface 구현
  @Service
  public class MemberServiceImpl implements MemberService {

    // dependency injection 레벨로 동작
    @Autowired
    public MemberDao memberDao;

    @Override
    public List<MemberVo> memberSelectList() {

      return memberDao.memberSelectList();
    }

  }
  ```

<br />

- **`dao`**

  - MemberDaoImpl

  ```java
  package com.edu.member.dao;

  import java.util.List;

  import org.apache.ibatis.session.SqlSession;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Repository;

  import com.edu.member.domain.MemberVo;

  // 해당 클래스가 데이터 접근 계층(Data Access Layer)의 컴포넌트임을 나타냄
  // 주로 데이터베이스 작업을 처리하는 DAO(Data Access Object) 클래스에 사용
  // 스프링의 컴포넌트 스캔 메커니즘에 의해 자동으로 빈으로 등록 => 이를 통해 의존성 주입(Dependency Injection)이 가능해짐
  @Repository
  public class MemberDaoImpl implements MemberDao {

    // sql과 관련된 모든 코드를 위임받아 처리(쿼리문만 존재)
    @Autowired
    private SqlSession sqlSession;

    @Override
    public List<MemberVo> memberSelectList() {

      // xml 파일의 쿼리문을 가져옴 => id로 접근
      // namespace(full-name)
      return sqlSession.selectList("com.edu.member.memberSelectList");
    }

  }
  ```

<br />

- **`mappers`**

  - mappers/member/MemberMapper.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

  <mapper namespace="com.edu.member">

    <!-- 테이블에 맞게 전부 설정 -->
    <!-- 테이블의 칼럼과 VO 객체의 인스턴스 변수를 맵핑시킴(대소문자 구분) -->
    <!-- id에는 PK 사용, PK가 없다면 result만 사용 -->
    <!-- dao에 있는 method 명 -->
    <resultMap type="memberVo" id="memberResultMap">
      <id column="MEMBER_NO" property="memberNo"/>
      <result column="EMAIL" property="email"/>
      <result column="MEMBER_NAME" property="memberName"/>
      <result column="PWD" property="password"/>
      <result column="CRE_DATE" property="createdDate" javaType="java.util.Date"/>
      <result column="MOD_DATE" property="modifiedDate" javaType="java.util.Date"/>
    </resultMap>

    <select id="memberSelectList" resultMap="memberResultMap">
      SELECT MEMBER_NO, EMAIL, MEMBER_NAME, CRE_DATE
      FROM MEMBER
      ORDER BY MEMBER_NO DESC
    </select>

  </mapper>
  ```

  > resultMap의 수와 select의 column 수는 상관없음 => VO 객체의 getter, setter가 존재하는지 중요

  > VO 객체의 constructor가 없어도 사용 가능

  > default constructor와 getter, setter만 있다면 사용 가능

<br />

- **`IoC(Inversion of Control)`**

  - 제어이 역전, 객체의 생성, 생멍주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미

  - 컴포넌트 의존관계 결정 설정 및 생명주기를 해결하기 위한 디자인 패턴

  ```
  1. 전체 객체를 미리 생성

  2. Injection(필요한 곳에 사용)
  ```

<br />

- **`IoC 컨테이너`**

  > Spring framework: 객체에 대한 생성 및 생명주기를 관리할 수 있는 기능을 제공 <br /> => IoC 컨테이너 기능 제공

  - 객체의 생성을 책임지고, 의존성을 관리

  - POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가짐

  - 개발자들이 직접 POJO를 생성할 수 있지만 컨테이너에게 맞김

  > 스프링에서는 Spring Container라고 부름

<br />

- **`DI(Dependency Injection) 의존성 주입`**

  - 각 클래스 간의 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해 주는 것

  - 스프링 컨테이너에 의해서 관리되는 객체

  - 스프링 빈이라고도 함

  - 스프링 설정 파일에 등록되어 사용, 자동 등록 기능 사용 가능 등

  > 어노테이션으로 주입

<br />

- **`Spring Container`**

  - 관리되는 빈이 모여 있는 곳

  - IoC 컨테이너로서 Application Context 클래스로 구현됨

  - 스프링은 스프링 컨테이너에 빈(자바 객체)을 로딩하여 관리

  - 빈을 자동으로 관리해 주는 기능은 스프링의 핵심 기능 중 하나

<br />

- **`DI의 개념`**

  - 각 클래스 간의 의존관계를 빈 설정(Bran Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해 주는 것

  - 개발자들은 단지 빈 설정 파일에서 의존관계가 필요하다는 정보를 추가하면 됨

  - 객체 레퍼런스를 컨테이너로부터 주입받아서, 실행 시에 동적으로 의존관계가 생성됨

  - 컨테이너가 흐름의 주체가 되어 애플리케이션 코드에 의존관계를 주입해 주는 것

<br />

- **`DI 장점`**

  - 코드가 단순해짐

  - 컴포넌트 간의 결합도가 제거됨

<br />

- **`DI의 유형`**

  - setter 메서드를 이용한 의존성 삽입

  - 생성자를 이용한 의존성 삽입

  - 일반 메서드를 이용한 의존성 삽입

<br />

- **`Application Context`**

  - Bean을 등록, 생성, 조회, 반환 관리하는 기능

  - Spring의 각종 부가 서비스를 추가로 제공

  - Spring이 제공하는 Application Context 구현 클래스가 여러 가지 종류가 있음

<br />

- **`Layered Architecture 특징`**

  - 계층화 아키텍처

  - 효율적인 개발과 유지보수를 위해 계층화하여 개발

  - 대부분의 중/대규모 애플리케이션에서 적용

  - 각 레이어는 독립된 R&R을 가짐

  ```
  - 프레젠테이션 영역

    - 사용자와 상호작용을 담당

    - 사용자의 요청을 분석/응답


  - 비즈니스 영역

    - 기능을 수행

    - 트랜잭션 수행


  - 데이터 영역

    - 데이터의 저장과 조회를 담당

    - 주로 데이터베이스와 연동하여 작업
  ```

<br />

- **`MVC 패턴`**

  - Layered Architecture를 사용한 대표적 패턴

  - Model, View, Controller로 구분

  - UI를 가지는 대부분의 애플리케이션은 MVC 혹은 변형된 MVC 패턴(ex:SpringMVC) 사용

  - 기능을 포함한 비즈니스 레이어, 데이터 접근 역할을 하는 DAO 레이어, 사용자가 인터랙션의 역할을 수행하는 프레젠테이션 레이어로 구분

<br />

- **`Annotation`**

  ```
  - @Autowired

    - component 간 의존관계는 Autowired라는 어노테이션으로 적용
  ```

<br />

- **`MVC 패턴의 개념`**

  - 모델-뷰-컨트롤러는 소프트웨어 공학에서 사용되는 아키텍처 패턴으로 주목적은 Business Login과 Presentation Logic을 분리하기 위함

  - MVC 패턴 사용 시, 사용자 인터페이스로부터 비즈니스 로직을 분리해 애플리케이션의 시각적 요소나 이면에서 실행되는 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있음

  - Model: 애플리케이션의 정보(데이터, 비즈니스 로직 포함)

  - View: 사용자에게 제공할 화면(프레젠테이션 로직)

  - Controller: Model과 View 사이의 상호작용을 관리

<br />

- **`Model 컴포넌트`**

  - 데이터 저장소와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 일을 함

  - 여러 개의 데이터 변경, 추가, 삭제를 하나의 작업으로 묶는 트랜잭션을 다루는 일을 함

  - DAO 클래스, Service 클래스에 해당

<br />

- **`controller 컴포넌트`**

  - 클라이언트의 요청을 받았을 때 그 요청에 대해 실제 업무를 수행하는 모델 컴포넌트를 호출하는 일을 함

  - 클라이언트가 보낸 데이터가 있다면, 모델을 호출할 때 전달하기 쉽게 데이터를 적절히 가공하는 일을 함

  - 모델이 업무 수행을 완료하면, 그 결과를 가지고 화면을 생성하도록 뷰에 전달

  > 클라이언트 요청에 대해 모델과 뷰를 결정하여 전달(Servlet과 JSP를 사용해 작성 가능)

<br />

- **`Spring MVC의 특징`**

  - DI, AOP 같은 기능뿐만 아니라, 서블릿 기반의 웹 개발을 위한 MVC 프레임워크 제공

  - Spring MVC 모델2 아키텍처와 Front Controller 패턴을 프레임워크 차원에서 제공

  - Spring MVC 프레임워크는 Spring을 기반으로 하고 있기 때문에 Spring이 제공하는 트랜잭션 처리나 DI 및 AOP 등을 손쉽게 사용 가능

<br />

- **`SpringMVC의 주요 구성 요소`**

  ```
  1. DispatcherServlet

    -  클라이언트의 요청을 받아서 Controller에게 클라이언트의 요청을 전달하고, 리턴한 결괏값을 View에게 전달해 알맞은 응답 생성


  2. HandlerMapping

    - URL과 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정하는 객체, DispatcherServlet은 하나 이상의 핸들러 맵핑을 가질 수 있음


  3. ModelAndView

    - Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체


  4. ViewResolver

    - Controller가 리턴한 뷰 이름을 기반으로 Controller 처리 결과를 생성할 뷰 결정
  ```

  <br />

  > DB -> DAO -> Service -> Controller -> View

<br />

## **코드**

- **`dao`**

  ```java
  package com.edu.member.dao;

  import java.util.List;

  import com.edu.member.domain.MemberVo;

  public interface MemberDao {

    List<MemberVo> memberSelectList();

    public MemberVo memberExist(String email, String password);
  }
  ```

  ```java
  package com.edu.member.dao;

  import java.util.HashMap;
  import java.util.List;

  import org.apache.ibatis.session.SqlSession;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Repository;

  import com.edu.member.domain.MemberVo;

  @Repository
  public class MemberDaoImpl implements MemberDao {

    // sql과 관련된 모든 코드를 위임받아 처리(쿼리문만 존재)
    @Autowired
    private SqlSession sqlSession;

    @Override
    public List<MemberVo> memberSelectList() {

      // xml 파일의 쿼리문을 가져옴 => id로 접근
      return sqlSession.selectList("com.edu.member.memberSelectList");
    }

    @Override
    public MemberVo memberExist(String email, String password) {

      HashMap<String, Object> paramMap = new HashMap<>();
      paramMap.put("email", email);
      paramMap.put("pwd", password);

      return sqlSession.selectOne("com.edu.member.memberExist", paramMap);
    }
  }
  ```

<br />

- **`service`**

  ```java
  package com.edu.member.service;

  import java.util.List;

  import com.edu.member.domain.MemberVo;

  // member와 관련된 모든 비즈니스 로직 처리 => 인터페이스로 표준화
  public interface MemberService {

    List<MemberVo> memberSelectList();
    public MemberVo memberExist(String email, String password);
  }
  ```

  ```java
  package com.edu.member.service;

  import java.util.List;

  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;

  import com.edu.member.dao.MemberDao;
  import com.edu.member.domain.MemberVo;

  //member와 관련된 모든 비즈니스 로직 처리 => interface 구현
  @Service
  public class MemberServiceImpl implements MemberService {

    // dependency injection 레벨로 동작
    @Autowired
    public MemberDao memberDao;

    @Override
    public List<MemberVo> memberSelectList() {
      return memberDao.memberSelectList();
    }

    @Override
    public MemberVo memberExist(String email, String password) {
      return memberDao.memberExist(email, password);
    }
  }
  ```

<br />

- **`controller`**

  ```java
  package com.edu.member.controller;

  import java.util.List;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.PostMapping;
  import org.springframework.web.bind.annotation.RequestMapping;

  import com.edu.member.domain.MemberVo;
  import com.edu.member.service.MemberService;

  import jakarta.servlet.http.HttpSession;

  @RequestMapping("/member") // url 맵핑
  @Controller // 컨트롤러 annotation
  public class MemberController {
    // MemberController에 대한 모든 logger 확인 가능
    private Logger log = LoggerFactory.getLogger(MemberController.class);
    private final String logTitleMsg = "==MemberController==";

    // 자동으로 spring bean 객체 생성
    @Autowired
    private MemberService memberService;

    // 로그인 화면 이동
    @GetMapping("/login")
    public String login(HttpSession session, Model model) {
      log.info(logTitleMsg);
      log.info("login");

      return "member/LoginFormView";
    }

    // 로그인 회원정보 조회 및 검증
    @PostMapping("/login")
    public String getLogin(String email, String password, HttpSession session, Model model) {
      log.info(logTitleMsg);
      log.info("Welcome MemberController getLogin! " + email + ", " + password);

      MemberVo memberVo = memberService.memberExist(email, password);

      if (memberVo != null) {
        session.setAttribute("member", memberVo);
      }

      return "redirect:/member/list";
    }

    // class 영역에서의 url과 메서드 영역에서의 url을 합친 최종 url
    @GetMapping("/list")
    public String getMemberList(Model model) {
      log.info(logTitleMsg);
      log.info("getMemberList");

      // service
      List<MemberVo> memberList = memberService.memberSelectList();

      model.addAttribute("memberList", memberList);

      return "member/MemberListView";
    }
  }
  ```

<br />

- **`JSP(View)`**

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>
      <script>
        function pageMoveLoginFnc() {
          location.href = "./member/login";
        }
      </script>
    </head>
    <body>
      <jsp:include page="/WEB-INF/views/Header.jsp" />
      <h1>Hello Spring Boot</h1>
      <p>Projects</p>
      <button onclick="pageMoveLoginFnc();">Sign In</button>
    </body>
  </html>
  ```

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>로그인</title>
    </head>
    <body>
      <jsp:include page="/WEB-INF/views/Header.jsp" />

      <h2>사용자 로그인</h2>

      <form action="./login" method="post">
        <label>이메일</label>
        <input
          type="text"
          name="email"
          value=""
          placeholder="ex:hong@test.com"
        />
        <br />
        <label>암호</label> <input type="password" name="password" value="" />
        <br />
        <input type="submit" value="로그인" />
      </form>

      <jsp:include page="/WEB-INF/views/Tail.jsp" />
    </body>
  </html>
  ```
