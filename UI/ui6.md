# UI 6일차

## 6일차

- 변수의 라이프 사이클

  - var: 변수

  > 선언의 중복 가능

  ```html
  <head>
    <script>
      var globalNum = 10;
      globalNum2 = 20;

      function numbering() {
        globalNum3 = 30;
        var localNum = 40;

        document.write("globalNum= " + globalNum + "<br />");
        document.write("globalNum2= " + globalNum2 + "<br />");
        document.write("globalNum3= " + globalNum3 + "<br />");
        document.write("localNum= " + localNum + "<br />");
      }

      numbering();
    </script>
  </head>
  ```

  ```html
  <head>
    <script>
      var globalNum = 10;
      globalNum2 = 20;

      function numbering() {
        globalNum3 = 30;
        var localNum = 40;

        document.write("globalNum= " + globalNum + "<br />");
        document.write("globalNum2= " + globalNum2 + "<br />");
        document.write("globalNum3= " + globalNum3 + "<br />");
        document.write("localNum= " + localNum + "<br />");
      }

      document.write("globalNum= " + globalNum + "<br />");
      document.write("globalNum2= " + globalNum2 + "<br />");
      document.write("globalNum3= " + globalNum3 + "<br />");
      document.write("localNum= " + localNum + "<br />");
    </script>
  </head>
  ```

  ```html
  <head>
    <script>
      var name = "global";

      function checkScope() {
        name = "local";

        console.log(name);
      }

      console.log(name);

      checkScope();

      console.log(name);
    </script>
  </head>
  ```

  <br />

  - let: 변수

  > 선언의 중복이 되지 않음

  ```html
  <head>
    <script>
      var varNum = 10;
      var varNum = 20;

      document.write(varNum + "<br />");

      let letNum = 100;
      let letNum = 200;

      document.write(letNum + "<br />");
    </script>
  </head>
  ```

<br />

- let

  - 동일한 변수를 중복해서 선언할 수 없음

  - 블록에서 전역변수처럼 사용할 수 없음

  - 변수 값을 변경할 수 있음(재할당 가능)

<br />

- let과 var의 차이

  ```html
  <head>
    <script>
      if (true) {
        str = "초기화?";
        let str = "asdf";

        alert(str);
      }
    </script>
  </head>
  ```

  ```html
  <head>
    <script>
      if (true) {
        str = "초기화?";
        // let str = "asdf";
        var str = "asdf";

        alert(str);
      }
    </script>
  </head>
  ```

<br />

- const: 상수

  - 상수 선언

  - var, let은 변경되는 데이터(변수)를 선언하지만 const(상수)는 변경하지 않을 데이터를 선언

  - 변수와 상수 차이 이외에 let의 특징을 가지고 있음

  ```html
  <head>
    <script>
      const DIVISION_VALUE = 100;

      let num = 200;

      document.write(num / DIVISION_VALUE);

      num = 300;

      DIVISION_VALUE = 50; // error

      document.write(num / DIVISION_VALUE);
    </script>
  </head>
  ```

<br />

- Dom(Document Object Model)

  - 문서 객체 모델

  - document 객체

  ```
  웹 페이지의 구조는 html이 모든 태그들을 감싸고 있으며,
  그 내부는 다시 각종 설정과 선언으로 이뤄진 head 태그와 body 태그로 나눠짐

  모든 웹 문서는 여러 태그들이 각각의 역할에 따라 구조화되어 있는데.
  "태그 각각을 객체로 인식"하는 개념
  ```

<br />

- 요소 선택

  - getElementById()

  ```html
  <body>
    <div>
      <h1>선택자</h1>
      <h2 id="title">원거리 선택자</h2>
      <ul>
        <li>getElementById</li>
        <li>getElementByTagName</li>
      </ul>
      <h2 id="title">근거리 선택자</h2>
      <ul id="list">
        <li>parentNode</li>
        <li>childNodes</li>
        <li>children</li>
        <li>firstChild</li>
        <li>previousSibling</li>
        <li>nextSibling</li>
      </ul>
    </div>
  </body>
  <script>
    const idTitle2El = document.getElementById("title2");

    idTitle2El.style.fontSize = "20px";

    const idListEl = document.getElementById("list");

    idListEl.style.backgroundColor = "gray";
  </script>
  ```

  <br />

  - getElementsByClassName()

  ```html
  <body>
    <div id="topTag">
      <h1>선택자</h1>
      <h2 id="title">원거리 선택자</h2>
      <ul>
        <li class="oddLiTag">getElementById</li>
        <li>getElementByTagName</li>
      </ul>
      <h2 id="title">근거리 선택자</h2>
      <ul id="list">
        <li class="oddLiTag">parentNode</li>
        <li>childNodes</li>
        <li class="oddLiTag">children</li>
        <li>firstChild</li>
        <li class="oddLiTag">previousSibling</li>
        <li>nextSibling</li>
      </ul>
    </div>
  </body>
  <script>
    const topTagObj = document.getElementById("topTag");
    const topTagOfUlList = topTagObj.getElementsByTagName("ul");

    for (let i = 0; i < topTagOfUlList.length; i++) {
      topTagOfUlList[i].style.border = "1px solid black";
    }

    const topTagOfOddLiList = topTagObj.getElementsByClassNAme("oddLiTag");

    for (let i = 0; i < topTagOfOddLiList.length; i++) {
      topTagOfOddLiList[i].style.fontSize = "10px";
    }
  </script>
  ```

  <br />

  - getElementByTagName()

  ```html
  <body>
    <div>
      <h1>선택자</h1>
      <h2 id="title">원거리 선택자</h2>
      <ul>
        <li>getElementById</li>
        <li>getElementByTagName</li>
      </ul>
      <h2 id="title">근거리 선택자</h2>
      <ul id="list">
        <li>parentNode</li>
        <li>childNodes</li>
        <li>children</li>
        <li>firstChild</li>
        <li>previousSibling</li>
        <li>nextSibling</li>
      </ul>
    </div>
  </body>
  <script>
    const myTagList = document.getElementsByTagName("ul");
    const myTagObj = myTagList[0].getElementsByTagName("li")[1];

    myTagObj.style.border = "3px solid #ff1493";
  </script>
  ```

  <br />

- 상대 위치로 요소를 선택하는 방법

  - parentNode: 선택된 요소의 부모 노드를 선택함

  - childNodes: 선택된 요소의 자식 노드들을 선택(요소 노드, 텍스트 노드) => 배열

  - children: 선택된 요소의 자식 노드들(요소 노드)을 선택 => 배열

  ```html
  <body>
    <div>
      <h1>선택자</h1>
      <h2 id="title">원거리 선택자</h2>
      <ul>
        <li>getElementById</li>
        <li>getElementByTagName</li>
      </ul>
      <h2 id="title">근거리 선택자</h2>
      <ul id="list">
        <li>parentNode</li>
        <li>childNodes</li>
        <li>children</li>
        <li>firstChild</li>
        <li>previousSibling</li>
        <li>nextSibling</li>
      </ul>
    </div>
  </body>
  <script>
    const listUl = document.getElementById("list");
    const parentObj = listUl.parentNode;

    alert(parentObj.tagName);

    // const childeObjList = listUl.childNodes;
    const childeObjList = listUl.children;

    for (let i = 0; i < childObjList.length; i++) {
      document.write(i + 1 + "번째: " + childObjList[i].textContent + "<br />");
    }
  </script>
  ```
