# UI 3일차

## 3일차

- CSS(Cascading Style Sheets)

  - style sheets 종류

    - 내부 스타일 시트: HTML 문서의 head 태그에 style 태그를 사용하여, HTML 문서에 사용할 스타일을 지정하는 방식

      ```html
      <head>
        <style>
          p {
            color: red;
          }
        </style>
      </head>
      <body>
        <p>Hello CSS</p>
      </body>
      ```

    <br />

    - 외부 스타일 시트: 하나의 별도의 스타일시트 파일을 통해서 HTML 스타일을 적용하는 방식

      ```html
      <head>
        <link rel="stylesheet" href="css/style.css" />
      </head>
      <body>
        <p>Hello CSS</p>
      </body>
      ```

      ```css
      /* css/style.css */
      p {
        color: red;
      }
      ```

    <br />

    - 인라인 스타일 시트: 인라인 HTML 요소의 style 속성을 사용해 스타일 적용법

      ```html
      <body>
        <p style="color: red;">Hello CSS</p>
      </body>
      ```

    <br />

    ```
    - 일반적으로 외부 스타일 시트 사용

    - 간단한 스타일은 내부 스타일 시트를 사용할 수도 있음

    - 인라인 스타일 시트는 대체로 거의 사용하지 않지만, 가장 우선순위가 높아 꼭 필요한 경우 편리하게 사용 가능
    ```

<br />

- Tag 선택자

  - 태그들을 선택

  - 사용법: 태그 명 {}

  ```html
  <body>
    <p>Hello CSS</p>
    <p>태그 선택자</p>
    <div>태그들을 모두 선택</div>
    <p>Hello CSS</p>
    <p>태그 선택자</p>
  </body>
  ```

<br />

- ID 선택자

  - id로 붙인 이름을 가져다 쓰는 선택자, id 선택자 앞에 #을 붙여서 구분

  > 태그에 이름을 부여함 => 태그의 id 속성에 유일한 이름 부여

  - 사용법: #id속성값 {}

  ```html
  <head>
    <style>
      #myObj {
        color: red;
      }
      #divObj {
        border: 1px solid;
        background-color: olive;
      }
    </style>
  </head>
  <body>
    <p>Hello CSS</p>
    <p id="myObj">id 선택자</p>
    <div id="divObj">id 선택자 태그 선택</div>
    <p>Hello CSS</p>
    <p>태그 선택자</p>
  </body>
  ```

<br />

- class 선택자

  - class로 붙인 이름을 가져다 쓰는 선택자

  - 여러 개의 태그에 같은 class 명을 부여해 그룹화한 것처럼 적용

  - 사용법: .클래스 명 {}

  ```html
  <head>
    <style>
      .commonStyle {
        font-weight: 700;
        width: 200px;
        height: 100px;
        background-color: navy;
        color: #fff;
      }
    </style>
  </head>
  <body>
    <h1 class="commonStyle">Hello CSS</h1>
    <p class="commonStyle">클래스 선택자</p>
    <div class="commonStyle">클래스 명이 같은 태그들을 모두 선택</div>
    <p>Hello CSS</p>
    <p>태그 선택자</p>
  </body>
  ```

<br />

- id 선택자와 class 선택자의 차이점

  - id 선택자는 해당 id의 값이 유일해야 함

  - class 선택자는 같은 class의 값이 여러 개여도 사용 가능

<br />

- 전체 선택자(\*)

  - 모든 태그를 선택함

  - \*(애스터리스크)를 붙여 표기함

  - 사용법: \* {}

  ```css
  * {
    text-align: center;
  }
  ```

<br />

- 하위 선택자

  - 공백으로 표기

  > 공백도 하나의 표기법

  ```html
  <head>
    <style>
      .abox p {
        color: blue;
      }
    </style>
  </head>
  <body>
    <div class="abox">
      <p>7월의 여행지</p>
      <p>8월의 여행지</p>
      <ul>
        <li><p>1주차 여행지</p></li>
        <li><p>3주차 여행지</p></li>
      </ul>
    </div>
    <p>내년의 여행지</p>
  </body>
  ```

<br />

- 자식 선택자

  - 태그 하나를 기준으로 바로 아래(자식) 태그를 선택

  - 사용법: 태그 > 자식 태그

  ```html
  <head>
    <style>
      .abox > p {
        color: blue;
      }
    </style>
  </head>
  <body>
    <div class="abox">
      <p>7월의 여행지</p>
      <ul>
        <li><p>1주차 여행지</p></li>
        <li><p>3주차 여행지</p></li>
      </ul>
      <p>8월의 여행지</p>
    </div>
    <p>내년의 여행지</p>
  </body>
  ```

<br />

- 그룹 선택자

  - 여러 선택자들을 콤마(,)로 구분해 함께 묶어 속성 부여

  ```html
  <head>
    <style>
      #aBox,
      #bBox {
        color: blue;
      }
    </style>
  </head>
  <body>
    <div id="aBox">
      <p>7월의 여행지</p>
      <p>8월의 여행지</p>
      <ul>
        <li><p>1주차 여행지</p></li>
        <li><p>3주차 여행지</p></li>
      </ul>
    </div>
    <p id="bBox">내년의 여행지</p>
  </body>
  ```

<br />

- 속성 선택자

  - HTML 요소의 속성 유무 또는 속성값을 중괄호([]) 안에 넣어 선택자로 사용

  - 태그명[속성명]

  - 태그명[클래스="클래스명"]

  ```html
  <head>
    <style>
      p[title] {
        border: 5px solid lightblue;
      }

      div[class="aBox"] {
        font-size: 30px;
      }
    </style>
  </head>
  <body>
    <div class="aBox">
      <p title="aaa">7월의 여행지</p>
      <p>8월의 여행지</p>
      <ul>
        <li><p title="aaa">1주차 여행지</p></li>
        <li><p>3주차 여행지</p></li>
      </ul>
    </div>
    <p class="aBox" title="aaa">내년의 여행지</p>
  </body>
  ```

<br />

- 가상 클래스 선택자

  - hover: 마우스가 닿았을 경우 속성을 부여하는 선택자

  - 사용법: 태그 명:hover {}

<br />

- 종속 선택자

  - type 선택자, id 선택자, class 선택자가 결합한 형태

  ```html
  <head>
    <style>
      p#sub_p {
        border: 5px solid lightblue;
      }
    </style>
  </head>
  <body>
    <div id="aBox">
      <p id="sub_p" title="aaa">7워르이 여행지</p>
      <p>8월의 여행지</p>
    </div>
  </body>
  ```

<br />

- CSS 우선순위 규칙

  - Cascading은 작은 계단 모양의 폭포를 말함, 연속적인 물의 흐름에서 높낮이 차이를 우선순위에 비유

  - css는 하나의 태그에 여러 가지 방법으로 동일한 속성의 스타일을 적용하는 경우, 우선순위에 따라서 적용될 스타일이 결정됨

  - 속성을 적용하다 보면 하나의 태그에 본의 아니게 같은 속성이 겹쳐 적용될 때가 있는 데 우선순위에 의해 결정됨

<br />

- 우선순위 결정 요소

  1. 중요도

  2. 명시도

  3. 코드의 순서

  ```
  저작자 !important > 저작자 css > 사용자 css > 브라우저 css
  ```

  > 아무것도 적용하지 않으면 브라우저 css가 적용됨

<br />

- 선택자에 따른 명시도

  ```
  inline style > id > class > 태그

  => 태그 요소에 스타일 속성을 줄 때 선택자를 사용

  => 명시도: 더 구체적이라는 뜻, 구체적일수록 명시도가 높음

  => id 선택자는 class 선택자보다 명시도가 높고, class 선택자는 태그 선택자보다 명시도가 높음

  => 같은 태그 선택자끼리는 뒤에 추가적인 선택자가 더 붙는 것이 더 자세하므로 명시도가 높음
  ```

<br />

- 선택자의 우선순위

  | 선택자                      | 우선순위 |
  | --------------------------- | -------- |
  | 전체 선택자(\*)             | 0        |
  | type 선택자(p, h1, ul..)    | 1        |
  | 가상 선택자(:first-child..) | 10       |
  | class 선택자(.abc)          | 10       |
  | id 선택자(#abc)             | 100      |

  <br />

- 선택자 우선순위 값 계산 예시

  | 선택자          | 우선순위       |
  | --------------- | -------------- |
  | ul              | 1              |
  | .gnb            | 10             |
  | ul.gnb          | 1 + 10 = 11    |
  | #toparea ul     | 100 + 1 = 101  |
  | #toparea .gnb   | 100 + 10 = 110 |
  | #toparea ul.gnb | 100 + 11 = 111 |

<br />

- 같은 선택자끼리라면 깊이가 깊을수록(구체적) 명시도가 높음

  ```html
  <head>
    <style>
      div {
        color: blue;
      }

      div ol {
        color: red;
      }

      div ol .cl_first .cl_second {
        color: green;
      }

      div ol #id_second {
        color: darkgray;
      }
    </style>
  </head>
  <body>
    <div>
      <ol>
        <li id="id_first" class="cl_first">first</li>
        <li id="id_second" class="cl_second">second</li>
        <li id="id_third" class="cl_third">third</li>
      </ol>
    </div>
  </body>
  ```

<br />

- 코드 순서

  - 선택자의 종류와 깊이가 같을 때 우선순위의 결정 방식은 코드의 순서

  - 위에서 아래로 코드가 수행되므로 내용 모두가 동일한 우선순위라면 제일 아래에 있는 코드가 나중에 적용되는 코드임

  ```html
  <head>
    <style>
      div {
        color: blue;
      }

      div ol {
        color: red;
      }

      div ol .cl_first .cl_second {
        color: green;
      }

      div ol .cl_first {
        color: purple;
      }
    </style>
  </head>
  <body>
    <div id="root">
      <ol>
        <li id="id_first" class="cl_first">first</li>
        <li id="id_second" class="cl_second">second</li>
        <li id="id_third" class="cl_third">third</li>
      </ol>
    </div>
  </body>
  ```
