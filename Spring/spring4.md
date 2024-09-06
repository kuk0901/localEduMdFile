# Spring 4일차

## **`이론`**

- Mybatis

  - 자바 오브젝트와 SQL문 사이의 자동 Mapping 기능을 지원하는 Object Mapper

  - SQL을 별도의 파일로 분리해서 관리하게 해 주며, 객체-SQL 사이의 파라미터 Mapping 작업을 자동으로 해주기 때문에 많은 인기를 얻고 있는 기술

  <br />

  - 장점

    - Hibernate나 JPA(Java Persistence API)처럼 새로운 DB 프로그래밍 패러다임을 익혀야 하는 부담 없음

    - 개발자가 익숙한 SQL을 그대로 이용하면서 JDBC 코드 작성의 불편함도 제거

    - 도메인 객체나 VO 객체를 중심으로 개발 가능

  <br />

  - 특징

  ```
  1. 쉬운 접근성과 코드의 간결함

    - 가장 간단한 퍼시스턴스 프레임워크

    - xml 형태로 서술된 jdbc 코드라고 생각해도 될 만큼 jdbc의 모든 기능을 mybatis가 대부분 제공

    - 복잡한 jdbc 코드를 걷어내며 깔끔한 소스코드 유지 가능

    - 수동적인 파라미터 설정과 쿼리 결과에 대한 맵핑 구문 제거 가능


  2. SQL문과 프로그래밍 코드 분리

    - SQL에 변경이 있을 때마다 자바 코드를 수정하거나 컴파일하지 않아도 됨

    - SQL 작성과 관리, 검토를 DBA와 같은 개발자가 아닌 다른 사람에게 맡길 수도 있음


  3. 다양한 프로그래밍 언어로 구현 가능

    - Java, C#, .NET, Ruby 등
  ```

  <br />

  - 주요 컴포넌트

    - SqlSession

      - 핵심적인 역할을 하는 클래스로서 SQL 실행이나 트랜잭션 관리를 실행함

      - SqlSession 오브젝트는 Thread-safe하지 않으므로 thread마다 필요에 따라 생성함

    - mapping 파일

      - SQL문과 ORM(Object Relational Mapping)을 설정

  <br />

  - 동적 SQL

    - Dynamic SQL

      - 검색 조건에 따라 검색해야 하는 SQL문이 달라지기 때문에, 이를 처리하기 위해 동적 SQL이 사용됨

  <br />

  - 동적 SQL 사용 시 주의 사항

    1. mybatis 구문을 이용해 SQL문이 실행 시에 변경되기 때문에, 모든 케이스에 대해 테스트가 이뤄져야 함

    2. 동적 SQL문이 없는 상태에서 정상적인 실행을 확인 후, 동적 SQL을 추가해 개발

  <br />

  - 표현식

    ```
    - if

    - trim

    - choose(when, otherwise)

    - foreach
    ```

    > JSTL 문법을 따름
