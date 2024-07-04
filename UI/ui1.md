# UI 1일차

## 1일차

- HTML: Hypertext Markup Language

- DTD(Document Type Definition)

  - 문서는 여기서 정해놓은 규칙에 따라 마크업한다 라는 의미

  - 다양한 브라우저에서 마크업에 대한 일관된 화면 표시가 될 수 있도록 해 줌

<br />

- 태그(Tag)

  - \<p>, \<a> 등과 같이 명령어의 형태가 <>(각 괄호)로 되어 있는 것

  - \<body><\body> 태그와 같이 태그의 시작과 종료를 하나의 요소(Element)라고 함

  > 이러한 요소들을 이용하여 웹 문서를 작성하는 것을 마크업(Markup)이라고 함

<br />

- head 태그 안에서 사용하는 태그

  - meta: html 문서의 설명, 키워드, 문서의 작성자 등을 지정

  - title: html 문서의 제목 지정

  - link: 외부의 css 파일을 html 문서에 연결

  - style: html 문서 내에서 스타일 정의

  - script(body 태그 내부에서도 사용 가능): 자바스크립트 html 문서 내부에서 정의하거나 외부에서 자바스크립트 파일을 불러올 때 사용

<br />

- 속성(Attribute)

  - 태그 내부에 부가적인 설정값을 선언하는 것

  ```html
  <meta charset="UTF-8" />
  ```

<br />

- 마크업 기본 규칙

  1. 태그는 시작 태그와 종료 태그가 있어야 함

  ```html
  <p>aaa</p>
  ```

  <br />

  2. 태그는 제대로 중첩되어야 함

  ```html
  <p><strong>aaa</strong>태그는 제대로 중첩되어야 함</p>
  ```

  <br />

  3. 요소 및 속성 이름은 소문자여야 함

  ```html
  <p><a href="local">aaa</a>태그는 제대로 중첩되어야 함</p>
  ```

  <br />

  4. img 태그에는 alt 속성이 있어야 함

  ```html
  <img scr="images/today.gif" alt="오늘" />
  ```

  <br />

  5. 주석 처리 방법

  ```html
  <!--  -->
  ```

<br />

- 블록 요소(Element) 특징

  1. 블록 태크는 줄 바꿈이 일어남

  2. 텍스트와 인라인 태그를 자식 태그로 포함 가능

  3. 블록 태그 중에는 블록 태그를 자식 태그로 포함할 수 있는 태그와 없는 태그가 있음

<br />

- 인라인 요소(Element) 특징

  - 줄 바꿈이 없음

  - 텍스트와 인라인 태그를 자식 태그로 포함할 수 있음

  - 인라인 태그는 블록 태르를 자식 태그로 포함 불가