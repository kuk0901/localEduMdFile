# JSP & Servlet 2일차

## 2일차

- JSP 기본 구성요소: JSP 작성 시 사용하는 태그와 클래스 등

  - 한 페이지의 특징을 설정하는 디렉티브

  - HTML 사이에 자바 코드를 넣어서 원하는 화면을 만드는 스크립트 요소

  - 스크립트 요소를 줄이고 자바 코드를 숨기면서 웹 프로그램 기능을 하는 액션 태그 등

  ```
  원래 한 페이지에서 다른 페이지로 객체를 전달할 수 없지만 전달하게 만드는 스코프 객체,
  상태가 없는 JSP에서 상태를 만들어 사용하는 세션,
  여러 화면을 보여주기 위한 모듈화,
  객체를 만들지 않고도 이미 준비된 객체를 사용하는 기본 객체,
  한 화면에서 다른 화면으로 이동하는 흐름 제어,
  예외 발생 시 처리 방법,
  저장되어 있는 환경변수를 사용하는 방법

  => 모두가 중요한 JSP 구성요소
  ```

<br />

- 웹 용어

  - 서버와 클라이언트

    - 클라이언트: 문제를 해결해 달라고 요청하는 쪽

    - 서버: 문제를 해결해서 돌려보내는 쪽

  <br />

  - 요청(Request) / 응답(Response)

    - 요청(Request): 클라이언트가 서버에서 문제를 해결해달라고 요구하는 행위

    - 응답(Response): 서버가 문제를 해결해서 클라이언트에게 보여주는 행위

    > 웹 브라우저: 클라이언트 <br /> 웹 서버: 서버

  <br />

  - 프로토콜(protocol)

    - 규약, 규칙, 약속

    - 클라이언트 쪽에서 규약을 지킨 형태의 코드를 보내면 서버 쪽에서 전송받은 코드를 해석하여 규약을 지킨 형태의 코드를 클라이언트에게 다시 보낼 수 있음

    - 클라이언트와 서버 간 전송을 위한 규약

    - 응답 상태코드: 약속된 값을 클라이언트로 보낼 수 있음

  <br />

  - 웹 서버(Web Server)

    - 웹 서버는 서버 쪽 컴퓨터에 있는 소프트웨어

    - 클라이언트의 요청을 받아서 웹 페이지(HTML, 그림파일, CSS, JavaScript 등으로 구성된 문서)를 클라이언트인 웹 브라우저에 응답하는 역할

    ```
    - 웹은 World Wide Web의 줄임말로 문서들이 인터넷으로 연결된 컴퓨터 세계를 말함

    - 하이퍼텍스트는 글자뿐 아니라 그림처럼 보여줄 수 있는 내용물을
    컴퓨터 사용자가 마우스나 키보드입력으로 요청할 때 바로 접근하고 사용할 수 있는 문서
    ```

  <br />

  - HTTP(HyperText Transfer Protocol)

    - 웹 서버에서 서버-클라이언트 사이에 대화(request/response)를 할 수 있도록 만든 규약

    - 헤더와 바디로 구성됨

    - HTTP의 요청과 응답에는 메시지가 포함되어 요청과 응답에 대한 상태를 알 수 있게 함

    - 서버는 요청 헤더 메시지를 읽고 클라이언트의 요청 사항을 파악한 다음 클라이언트에게 응답을 보냄

    - 응답헤더: 요청이 제대로 처리되었는지, 응답해주는 서버의 간단한 정보, 응답 내용의 타입 및 인코딩, 응답 크기 등이 포함됨

  <br />

  - 상태코드(Status code)

    - 클라이언트가 서버에게 요청하면, 서버는 요청을 처리한 다음 그 결과를 3자리 숫자로 된 상태코드와 함께 클라이언트에게 보내줌

    - 성공(200) 이외에는 서버 쪽에서 오류가 발생했다고 볼 수 있어 오류코드라고도 함

    - 주요 상태코드

    | 상태코드 | 설명                             |
    | -------- | -------------------------------- |
    | 200      | 성공                             |
    | 404      | 경로가 잘못됨                    |
    | 500      | 서버 쪽에서 문법적으로 예외 발생 |

  <br />

  - 상태(State) - 무상태(StateLess) / 유상태(StateFull)

    - 무상태(StateLess)

    ```
    - HTTP를 통한 클라이언트와 서버 사이의 대화는 방금 전 대화를 기억하지 못함(무상태)

    - 서버는 웹 브라우저를 통해 받은 요청에 응답한 다음 연결을 끊음

    - 이 상태에서 클라이언트가 서버에게 요청을 하려면 서버에 다시 연결해야 함

    => HTTP Session을 무상태로 한 이유는 많은 클라이언트들의 요청에 의한 웹 서버의 과부하를 방지하기 위한 것
    ```

    - 유상태(StateFull)

    ```
    - DB 서버는 클라이언트와 나눈 대화를 기억함(유상태)

    - 대화가 끝난 후 다시 연결하지 않아도 요청 가능

    - DB 서버가 대화를 끝내려면 명시적으로 끝내야 함

    => DB 서버는 사용ㅈ가 제한적이므로 연결을 유지하느 편이 좋음
    ```

  <br />

  - HTTP 요청 메서드

    - 웹 서버나 웹 애플리케이션 서버는 서버-클라이언트 사이의 요청/응답용 프로토콜로 HTTP를 지원

    - HTTP는 헤드와 바디로 나뉘며, 전송 데이터를 보내는 방법에 따라 요청 메서드 종류가 달라짐

    - JSP/Servlet은 get, post를 주로 사용하고 head는 가끔 사용함

    | 메서드 | 설명                                                                                                                                                                                                     | 특징                                                                                                                                                                                                                                                                                      |
    | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | get    | 서버에 요청 메서드를 보낼 때 헤드에만 내용이 있고 바디에는 전송데이터가 없음 <br /> 요청을 간단하게 하기 위한 것으로 헤드에 포함되는 내용의 크기도 제한적 <br /> 또한 URL에 전송 데이터가 노출될 수 있음 | 1. url에 데이터를 포함 -> 데이터 조회에 적합 <br /> 2. 바이너리 및 대용량 데이터 전송 불가 <br /> 3. 요청라인과 헤드 필드의 최대 크기 <br /> HTTP 사양에는 제한사항 없음 <br /> 대용량 url로 인한 문제 발생 -> 웹 서버에 따라 최대 크기 제한 <br /> 보안에 좋지 않음 <br /> 즐겨찾기 가능 |
    | post   | 서버에 보낼 전송 데이터를 바디에 넣어서 요청 <br /> form을 이용하여 전송데이터를 서버로 보낼 때 사용 <br /> 전송데이터 크기에 제약 없음                                                                  | 1. url에 데이터가 포함되지 않음 -> 외부 노출 방지 <br /> 2. 메시지 본문에 데이터 포함 -> 실행 결과 공유 불가 <br /> 3. 바이너리 및 대용량 데이터 전송 가능                                                                                                                                |
    | head   | 응답용으로 사용되며 헤드에만 내용이 있음 <br /> 바디 없이 헤드 정보, 캐시 정보를 클라이언트에 보낼 때 사용                                                                                               |

  <br />

  - 동기(Synchronous) / 비동기(Asynchronous) 전송

    - 동기(Synchronous): 요청 후 응답이 올 때까지 다른 요청을 할 수 없는 전송 상태

      ```
      - 새로운 요청을 하면 한 페이지 전체를 다시 불러옴

      - 응답할 때 마다 페이지 전체를 다시 불러오고 프로그레스바가 작동
      ```

    <br />

    - 비동기(Asynchronous): 요청한 다음 응답을 받아 화면을 보여주면서 백드라운드에서 서버로부터 데이터를 받는 등 다른 작업을 할 수 있는 전송 상태

      ```
      - 비동기 상태에서 서버는 필요한 데이터만 전소오디도록 응답

      - AJAX(Asynchronous JavaScript And XML)라는 이름으로 많이 사용
      ```

  <br />

  - 정적문서 / 동적문서

    - 정적문서(static document): HTML과 같은 문서

    ```
    - 서버에서 클라이언트로 응답할 때 바디 부분을 HTML로 구성됨

    - HTML은 클라이언트인 웹 브라우저에 전달되어 화면에 출력됨

    - 서버에서는 변환이나 실행이 되지 않으며, 그대로 웹 브라우저에 전달됨
    ```

    <br />

    - 클라이언트 동적문서(dynamic document): 스크립트(자바스크립트),

    ```
    - 스크립트: 서버에서는 변환이나 실행을 하지 않다가 웹 브라우저에서 정상문자 입력 검증이나,
      변환 혹은 실행을 함
    ```

    ```
    - HTML이나 스크립트 언어는 클라이언트 문서임

    - 스크립트는 한 줄씩 인터프리터로 전달되어 실행됨

    - JSP/Servlet은 서버 문서이고, 컴파일되어 실행된 결과를 HTML로 만듦

    - JSP/Servlet은 서버에서 실행되며 클라이언트에게 응답하는 문서를 만들므로 동적문서이고,
      컴파일되어 실행된 결과를 HTML로 만듦

    - 웹 프로그래밍은 클라이언트의 요청을 받아 웹 서버에서 JSP/Servlet을 실행하고 결과를 웹 브라우저로 응답하는 것

    ```

    - 클라이언트 사이드 문서의 종류

      - 정적문서: HTML, CSS

      - 동적문서: 스크립트

    <br />

    - 서버 사이드 문서의 종류

      - 동적문서: JSP/Servlet

    <br />

  - HTTP Server

    - HTML, CSS, JS로 구성된 문서는 서버에서 변환되거나 실행되지 않음

    - 클라이언트 사이드 문서를 요청한 웹 브라우저로 응답해주는 서버

    - 요청에 대한 응답만 함

    <br />

  - 웹 컨테이너(Web Container)

    - 웹 서버에는 HTML을 클라이언트로 보내는 HTTP Server, JSP/Servlet을 실행하는 엔진, 엔진과 JSP/Servlet를 실행하는데 필요한 라이브러리 등을 표함한 웹 컨테이너가 있음

    - 엔진은 JSP/Servlet을 실행해 결과물인 HTML을 만듦

    - 커넥터는 HTML을 HTML Server로 보내 웹 브라우저가 응답하게 함

    <br />

  - 서블릿(Servlet)

    - 컨테이너에서 실행되는 프로그램

    - 즉, 웹 서버(Tomcat)의 컨테이너(카탈리나)에서 실행되는 웹 프로그래밍

    - 요청 또는 요청과 동반하는 파라미터를 서블릿에서 받아 요청을 처리하고 결과를 HTML로 만듦

    - 웹 브라우저는 응답으로 보내진 HTML을 받아서 화면에 출력

    <br />

  - 라이프사이클

    - 웹 프로그래밍은 컨테이너가 반드시 필요하며 프로그래밍을 엔진에서 실행해 원하는 결과 생성하는데, 컨테이너의 명령에 따라 생성 - 초기화 - 실행 - 소멸 등 정해진 행동을 하는 것

    ```
    - 컨테이너는 요청에 따라 정해진 메서드를 호출

    - 요청을 처음으로 받으면 서블릿을 생성함

    - 초기화 메서드를 호출해 생성된 서블릿을 실행하는데 필요한 데이터나 값으 ㄹ얻음

    - 실행 메서드를 호출해 요청을 처리하고 결과를 응답해줌

    - 두 번째 요청부터는 실행 메서드만 호출함

    - 더 이상 요청이 없다면 소멸 메서드를 호출해 자원을 회수하고,
      서블릿의 객체를 제거함

    - 컨테이너는 초기화와 소멸 메서드를 한 번만 호출함
    ```

    <br />

  - JSP(Java Server Page)

    - JSP 중 Server page는 서버에서 실행되는 라이프사이클이 있는 웹 프로그래밍용 클래스

    - java는 개발 언어를 가리킴

    - Tomcat 웹 서버에서 엔진이 JSP를 서블릿으로 변환함

    ```
    1. hello.jsp 생성

    2. org.apache.jsp.hello_jsp.java와 같이 서블릿으로 변환

    3. org.apache.jsp.hello_jsp.class로 컴파일된 다음 실행

    => JSP도 서블릿이므로 서블릿에서 사용하는 객체나 메서드 사용 가능
    ```

    <br />

  - 웹 컴포넌트(Web Component)

    - 컨테이너에서 실행될 수 있도록 필요한 규칙을 준수한 웹 프로그램 묶음

    ```
    - 컴퓨터의 마더보드는 여러 카드를 꽂을 수  있는 구멍(슬롯)이 있고, 각 카드는 정해진 목적이 있음

    - 목적을 이루기 위해 여러 부속품으로 여러 카드를 만듦


    => JSP나 Servlet은 부속품, 웹 컴포넌트는 각 카드에 해당함

    => 각 카드는 마더보드에 꽂혀야 실행됨

    => 슬롯 설치 시 반드시 규칙을 지켜야 하는데, 웹 컴포넌트에서도 "API라는 규칙"을 반드시 지켜야 함
    ```

    > 웹 프로그래밍: 웹 컴포넌트를 만드는 방법, 웹 컴포넌트와 웹 컨테이너 사이의 규칙을 배우는 것이라 할 수 있음

    <br />

  - 컨텍스트(Context)

    - 배포단위, 실행단위의 디렉터리

    ```
    1. 개발 후 JSP나 Servlet, 그림, HTML, web.xml을 압축해 묶어 war(아카이브)를 만듦

    2. war(아카이브)를 웹 컨테이너에 놓으면 압축이 풀려 실행을 위한 디렉터리가 됨

    3. 디렉터리가 바로 컨텍스트이며 모든 파일을 각 용도에 맞게 컨텍스트 안의 디텍터리에 놓아야 함
    ```

    > 컨테이너는 컨텍스트의 web.xml을 가장 먼저 한 번 읽어들임

    <br />

  - WAS(Web Application Server - Java Enterprise Edition Server)

    - WAS: Web Server + Application Server + Service의 의미

    - Web Server: JSP/Servlet을 위한 웹 컨테이너와 HTML을 서비스하는 HTTP Server가 있음

    - Application Server: 비즈니스 로직을 수행하는 EJB 컨테이너가 있음

    - Service: JNDI, JMS, JavaMail 등을 제공하며 다른 기능의 서버와 연결해 사용하게 하거나 서버 안에서 편리하게 사용할 수 있게 함

    > Enterprise Edition의 필수요소를 모두 구현한 서버를 Enterprise Edition Server || WAS라 부름

    > WAS에는 WebLogic, WebSphere, JBoss, Zeus 등 수많은 제품이 있고 Tomcat을 WAS로 부르기도 함

    <br />

  - 레이어(Layer) / 티어(Tier)

    - 레이어: 프로그램의 역할에 따라 논리적으로 나눈 것

    - 티어: 시스템의 역할에 따라 물리적을 나눈 것

    > 혼용되기도 하지만 논리적인 것과 물리적인 것으로 구분됨

    ```
    ex)

    - DB에서 DVD 대출 정보를 찾아 웹 브라우저에 출력하는 웹 프로그램밍을 한다고 가정

    - DVD 대출 프로그램은 DB에 접근해 정보를 가져오는 부분(Data Layer),
      고객 정보인지 비디오 정보인지 요청을 판단하고 실행하는 부분(Business Layer),
      HTML로 결과를 만들고 응답하는 부분(Presentation Layer)으로 나눌 수 있음

    => DVD 대출 프로그램은 역할에 따라 나눌 수 있으며 이를 레이어라 함

    => DB 서버, 웹 서버, 클라이언트인 웹 브라우저 등 시스템을 물리적으로 나눌 수 있는데
      이를 티어라고 함


    * 웹 프로그램밍은 기본적으로 3티어, 레이어는 보통 4개로 나뉨
    ```

    - 화면 레이어: JSP, Servlet이 담당

    ```
    1. 요청을 받아 비즈니스 레이어에 정보를 요청

    2. 결과를 받아 요청한 화면을 동적으로 만듦

    3. 요청한 화면으로 옮김
    ```

      <br />

    - 데이터 레이어(DB 레이어): 영속성 레이어로도 알려진 레이어로 DB에 쿼리를 실행

    ```
    - DB에 접근해 쿼리를 실행하기 위해 JDBC를 이용하거나, 해당 쿼리에 대해
      JDBC 작업을 자동으로 실행해 쉽게 사용할 수 있도록 만든 프레임워크를 사용함
    ```

      <br />

    - 비즈니스 레이어(비즈니스 로직 레이어): 비즈니스 로직을 처리하는 레이어

    ```
    - 화면 레이어는 데이터 레이어를 이용함

    - 비즈니스 레이어에서 얻은 객체를 비즈니스 객체라고 하며, DB의 테이블과 관련 있음

    - 데이터 레이어에서 얻은 결과로 화면 레이어에서 필요한 비즈니스 객체를 만들어도 됨
    ```

      <br />

  - DAO(Data Access Object - 데이터 접근 객체)

    - DB에 관련된 작업(CR: Retrieve UD)을 전문적으로 담당하는 객체

    - DAO 안의 모든 메서드는 모두 DB와 관련된 작업을 함

    ```
    CRUD를 실행하는 메서드는 JDBC를 이용해 DB에 접근해 쿼리를 실행

    다른 개발자도 해당 메서드 호출 시 해당 쿼리를 실행해 결과를 얻을 수 있음
    ```

      <br />

  - DTO(Data Transfer Object - 데이터 전송 객체)

    > VO(Value Object - 데이터 객체)

    - DB의 테이블에 해당하는 객체로 데이블의 칼럼들을 1:1로 저장할 수 있는 멤버필드가 있고 get/set 메서드를 가짐

    - DTO는 로직이 없으며 일반적으로 하나의 DTO가 하나의 행에 해당되고, 대부분 DAO와 같이 사용됨

    - 비즈니스 레이어에서 반환하는 비즈니스 객체

      <br />

  - MVC 패턴(Model - View - Controller)

    - 요청을 처리하는 과정에서 발생하는 처리순서, 데이터 전송, 관리 작업과 데이터 출력에 대한 웹 애플리케이션 작업을 간단하게 도식화함

    ```
    - 요청을 받은 컨트롤러는 요청을 분석

    - 요청에 해당하는 모델을 이용해 비즈니스 로직(데이터베이스에 관련된 작업)을 실행하고 비즈니스 객체를 얻음

    - 해당 뷰로 제어권을 넘김

    - 뷰는 받은 비즈니스 객체를 동적으로 처리하고 HTML로 화면을 만들어 웹 브라우저에 응답함
    ```