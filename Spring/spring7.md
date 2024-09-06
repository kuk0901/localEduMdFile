# Spring 7일차

## **`이론`**

- 페이징 연습을 위한 더미 데이터 생성(PL/SQL 프로시저)

  ```sql
  -- PL/SQL 언어
  -- 프로시저 생성
  CREATE OR REPLACE PROCEDURE INSERT_FREE_BOARD_RECORDS AS
  BEGIN
      FOR i IN 1..53 LOOP
          INSERT INTO free_board
          VALUE(FREE_BOARD_ID, MEMBER_NO, FREE_BOARD_TITLE, FREE_BOARD_CONTENT
              , CREATE_DATE, UPDATE_DATE)
          VALUES(FREE_BOARD_ID_SEQ.NEXTVAL, 6,
              '제목:TEST 자료' || FREE_BOARD_ID_SEQ.CURRVAL,
              '내용:TEST 자료' || FREE_BOARD_ID_SEQ.CURRVAL,
              SYSDATE + FREE_BOARD_ID_SEQ.CURRVAL,
              SYSDATE + FREE_BOARD_ID_SEQ.CURRVAL
          );
          END LOOP;
  END;
  /

  -- 프로시저 실행
  BEGIN
      INSERT_FREE_BOARD_RECORDS; -- 프로시저 명
      COMMIT;
  END; -- end
  / -- 실행
  ```

<br />

- CDN(Content Delivery Network)

  - 콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워키에 데이터를 저장하여 제공하는 시스템

<br />

- jQuery 기본 형식

  - window.onload = function () {}

  ```js
  // 풀네임
  $(document).ready(function () {
    실행문;
  });

  // 축약형
  $(function () {
    실행문;
  });
  ```

  <br />

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <script src="./jquery-3.7.1.js"></script>
      <script>
        $("#root").html("<p>Dobby</p><div>1</div>");
      </script>
    </head>
    <body>
      <div id="root"></div>
    </body>
  </html>
  ```
