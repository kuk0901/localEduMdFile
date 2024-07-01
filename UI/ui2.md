# UI 2일차

## 2일차

- p 태그

  - 텍스트와 인라인 요소를 포함할 수 있지만, 블록 레벨 요소는 포함할 수 없음

  ```html
  <body>
    <p>
      p 태그는 블록 태그를 포함할 수 없음
    </p>

    <p>
      <h1>
        p 태그는 블록 태그를 포함할 수 없음
      </h1>
    </p>
  </body>
  ```

  > 태그 선언이 잘못되었지만, 실행은 됨

<br />

- 컴파일러(compiler) vs 인터프리터(interpreter)

  ```
  - 컴파일러: 프로그램 전체를 스캔하여 이를 모두 기계어로 번역(하나라도 틀린 코드는 오류)

  - 인터프리퍼: 프로그램 실행 시 한 번에 한 문장씩 번역 => 한 줄씩 해석 후 실행(순차적)
  ```

<br />

- a(link) 태그

  - 다른 html 문서의 이동이나 동일한 html 문서 내에서 이동할 수 있음

  - 다른 곳으로 정보를 연결해 주는 역할

  ```html
  <body>
    <a href="https://www.naver.com/" target="_blank">NAVER</a>
  </body>
  ```

<br />

- 크로스 브라우징, 크로스 브라우저

  - 최대한 많은 종류의 웹 브라우저에서 정상적으로 작동하는 웹 페이지를 만드는 방법론 중 하나

  ```
  - 이기종 컴퓨팅

    - 하나 이상의 프로세서 또는 코어를 사용하는 시스템


  - 이기종 시스템(Heterogeneous System)

    - CPU와 GPU의 벽을 허물고 소프트웨어가 두 부품의 컴퓨팅 자원을 자유롭게 활용한다'는 의미

  => 이기종: 하나 이상의 프로세서 또는 코어를 사용하는 기기?
  ```

<br />

- img

  - 이미지를 삽입할 때 사용

  - src 속성에 이미지의 경로를 지정해야 함(상대 경로 || 절대 경로 작성)

  ```html
  <body>
    <img scr="./images/bt.png" alt="검색" />
  </body>
  ```

  ```
  - 절대 경로(dbasolute path)

    - 나의 위치와 상관없이 무조건 원하는 자료를 찾는 개념

    - 파일의 root부터 해당 파일까지의 전체 경로(URL)

    - ex) D:\IntelLocalEdu1\sqldeveloper(윈도우 운영체제)


  - 상대 경로(relative path)

    - 나를 기준으로 원하는 자료를 찾아가는 개념

    - 현재 파일의 위치를 기준으로 연결하려는 파일의 상대적인 경로를 적는 것

    - ex) images/logo.png -> ./images/logo.png


  - .: 현재 위치

  - /: 폴더와 파일 구분자

  - ..: 상위 폴더로 이동
  ```

<br />

- 주소 체계 분석

  - URL(주소)

  ```
  http://localhost:8090/HtmlBasic/ch1_42imgBasic.jsp

  1. http: http 프로토콜

  2. localhost: ip 주소

  3. 8090: 포트 번호

  4. HtmlBasic/ch1_42imgBasic.jsp(프로젝트명/나머지 webapp에서 시작되는 경로): 프로젝트 주소


  * localhots = 127.0.0.1 = 내 컴퓨터 주소
  192 또는 2?? 번대로 시작하면 외부 ip 주소
  ```

<br />

- 목록 관련 태그

  - ol(ordered lis)

    - 순서가 있는 목록을 정의할 때 사용

  - ul(unordered lis)

    - 순서가 없는 목록을 정의할 때 사용

  > 공통점: 자식 태그로 반드시 li 태그 사용(다른 태그는 자식 태그로 사용 불가)

  - li(list item)

    - 텍스트, 인라인 태그, 블록 태그 모두 포함 가능

  ```html
  <body>
    <!-- list 순서 있음 -->
    <ol>
      <li>a</li>
      <li>b</li>
      <li>c</li>
    </ol>

    <!-- list 순서 없음 -->
    <ul>
      <li>a</li>
      <li>b</li>
      <li>c</li>
    </ul>
  </body>
  ```

<br />

- 표(Table)

  - 웹 문서에서 표를 만드는 태그

  - 블록태그이며 행(tr)과 열(td)을 가짐, td 태그는 셀(cell)이라고 부름

  ```html
  <!DOCTYPE html>
  <html lang="ko">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
      <script defer src="./index.js"></script>
      <style>
        table {
          border: 1px solid;
          border-collapse: collapse;
        }
        td {
          border: 1px solid;
          padding: 5px;
          text-align: center;
        }
      </style>
    </head>

    <body>
      <table>
        <!-- 1행 1열 -->
        <tr>
          <td>1</td>
        </tr>

        <!-- 2행 2열 -->
        <tr>
          <td>1</td>
          <td>2</td>
        </tr>
      </table>
    </body>
  </html>
  ```
