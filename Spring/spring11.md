# Spring 11일차

## **코드**

- **`Lombok`**

  - 여러 가지 @어노테이션을 제공하고 컴파일 과정에서 자동으로 개발자가 원하는 메서드를 생성/주입 방식으로 동작하는 라이브러리

  <br />

  1. **기능**

     - model 클래스나 Entity 같은 도메인 클래스 등에 반복되는 getter, setter, toString 등의 메서드를 자동으로 만들어 주는 기능을 함

  2. **장점**

     - 복잡하고 반복되는 코드를 어노테이션 기반의 코드 자동 생성으로 생산성이 향상되고 코드가 축소되어 가독성 및 유지보수성을 높일 수 있음

  3. **단점**

     - 코드가 직접 눈에 보이는 게 아니므로 직관성이 떨어질 수 있음

  4. **자주 사용되는 어노테이션**

     ```
     - "@NorgsConstructor"

       - 파라미터가 없는 기본 생성자를 만들어줌

     - "@AllArgsConstructor"

       - 모든 필드 값을 파라미터로 받는 생성자를 만들어줌

     - @EqualsAndHashCode

       - equals와 hashcode를 자동으로 생성해 주는 어노테이션

     - @Data

       - "Getter", "Setter", RequiredArgsConstructor, "toString", EqualsAndHashCode를 한 번에 설정해 주는 어노테이션

       - 실무에서는 너무 무겁고 객체의 안정성을 지키기 때문에 @Data의 활용을 지양함

     - Builder

       - 자동으로 해당 클래스에 빌더를 추가함
     ```

<br />

- 기존의 웹 접근 방식과 RESTful API 방식과의 차이점

  | 종류    | Method | 기존 게시판             | Method         | REST API를 지원하는 게시판 |
  | ------- | ------ | ----------------------- | -------------- | -------------------------- |
  | 글 목록 | GET    | /bbs/list               | `GET`          | /bbs                       |
  | 긁 읽기 | GET    | /bbs/list/{articleId}   | `GET`          | /bbs/{articleId}           |
  | 글 등록 | POST   | /bbs/list/resist        | `POST`         | /bbs                       |
  | 글 삭제 | GET    | /bbs/remove/{articleId} | `DELETE`       | /bbs/{articleId}           |
  | 글 수정 | POST   | /bbs/modify/{articleId} | `PUT`, `PATCH` | /bbs/{articleId}           |

  ```
  - 기존의 게시판

    - GET, POST만으로 CRUD 처리하며, URI 액션을 나타냄

  - REST 게시판

    - 4가지 메서드를 모두 사용하여 CRUD를 처리하며, URI는 제어하려는 자원을 나타냄
  ```

<br />

- **`JSON(Javascript Object Notation)`**

  - WEB에서 Data 교환 방식(사실상 표준)

  - 경량 Data 교환 형식

  - 표현식: 사람과 기계 모두 이해하기 쉬우며 용량이 적어서, 최근에는 json이 xml을 대체해서 데이터 전송 등에 많이 사용됨

  - 특정 언어에 종속되지 않으며, 대부분의 프로그래밍 언어에서 json 포맷의 데이터를 핸들링할 수 있는 라이브러리를 제공

<br />

- **`핵심 REST 어노테이션`**

  - @RequestBody: HTTP request body를 java 객체로 전달받을 수 있음

  - @ResponseBody: Java 객체를 HTTP Response Body로 전송할 수 있음

  > 자바 객체를 XML이나 JSON으로 변환해서 전송할 수 있는 기능 제공
