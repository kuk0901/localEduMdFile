# Spring 3일차

## 이론

- Controller

  - 사용자 요청(URL 기반)에 해당하는 Controller의 특정 메서드가 호출됨

  ```
  Controller는 요청의 파라미터가 있으면 처리하고 비즈니스 처리를 위해서 서비스 컴포넌트를 주입받아서 실행

  => 실행된 결과를 전달받아 화면 정보와 함께 DispatcherServlet에게 반환
  ```

  <br />

  - 일반적으로 사용되는 Annotation

    - Controller

    - RequestMapping

    - Autowired

  <br />

  - 컨트롤러에서 클라이언트의 요청을 처리한 후에 다른 페이지로 리다이렉트 하고 싶은 경우

    - redirect:/경로

  <br />

  - 요청을 처리한 후 다른 페이지로 포워드 하고 싶을 때

    - forward:/경로

  ```
  > 경로 부분이 "/"로 시작하면 웹 애플리케이션 내에서 절대 경로로 사용됨

  > "/"로 시작하지 않으면 어노테이션의 경로를 기준으로 상대 경로로 사용됨
  ```

<br />

- Open API(Application Programming Interface)

  - 개방형 API

  - API가 응용 프로그램을 개발할 때 사용하는 인터페이스라는 의미

  - 프로그래밍에서 사용할 수 있는 개방된 상태의 인터페이스

  ```
  - daum, naver 등의 포털 사이트 / 통계청, 기상청 등과 같은 관공서에도 갖고 있는 데이터를
    외부 응용 프로그램에서 사용할 수 있도록 open api를 제공하고 있음

  - 대부분 open api는 rest 방식으로 지원되고 있음
  ```

<br />

- RESTful(Representational State Transfer)ful API

  - HTTP URI + HTTP Method

  - HTTP URI를 통해 제어할 자원을 명시하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 해당 자원(RESOURCE)을 제어하는 명령을 내리는 방식의 아키텍처

  - HTTP 프로토콜에 정의된 4개의 메서드들이 자원에 대한 CRUD Operation을 정의

    - 요청 방식(선택)

    ```
    (HTTP method: CRUD)
    - GET: read(select)

    - POST: create(insert)

    - PUT: update or create

    - DELETE: delete
    ```

<br />

- ORM(Object Relational Mapping)

  - 객체-관계 매핑

  - OOP(Object Oriented Programming)에서 쓰이는 객체라는 개념을 구현한 클래스와 RDB(Relational DataBase)에서 쓰이는 데이터인 테이블 자동으로 맵핑(연결)하는 것

  ```
  - 클래스와 테이블은 서로가 기존부터 호환 가능성을 두고 만들어진 것이 아니기 때문에 불일치가 발생

  => ORM을 통해 객체 간의 관계를 바탕으로 SQL문을 자동으로 생성하여 불일치를 해결
  ```
