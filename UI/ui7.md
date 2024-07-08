# UI 7일차

## 7일차

- DOM CRUD

  - 태그 관리(managing, manager)

  | 메서드          | 설명                                                                                                                             |
  | --------------- | -------------------------------------------------------------------------------------------------------------------------------- |
  | createElement   | 태그를 생성하는 메서드                                                                                                           |
  | createTextNode  | 텍스트를 생성하는 메서드                                                                                                         |
  | appendCHild     | 태그를 부모와 자식 관계로 만들어주는 메서드 <br /> 부모태그.appendChild(자식태그)                                                |
  | removeChild     | 태그를 제거해 주는 메서드 <br /> 부모.removeChild(자식) => 자식이 지워짐                                                         |
  | insertBefore    | 태그에 새로운 자식을 추가 <br /> 태그.insertBefore(새로운 자식, 선택 태그 자식) <br /> 선택 태그 자식 앞에 새로운 자식 태그 추가 |
  | replaceChild    | 부모 태그의 자식인 선택 태그를 새 태그로 덮어 씌움 <br /> 부모.replaceChild(새 태그, 선택 태그)                                  |
  | cloneNode       | 선택태그.cloneNode(true or false) <br /> 선택 태그를 복제하여 true인 경우 하위 태그까지 모두 복제                                |
  | getAttribute    | 태그의 특정 속성의 값을 반환 <br /> 선택태그.getAttribute('속성')                                                                |
  | setAttribute    | 태그의 특정 속성에 값 지정 <br /> 선택태그.setAttribute('속성', '값')                                                            |
  | removeAttribute | 태그의 특정 속성을 삭제 <br /> 선택태그.removeAttribute('속성')                                                                  |
  | innerHTML       | 문자 방식으로 요소를 생성하는 방법                                                                                               |

  <br />

- DOM CRUD 사용

  - createElement(), createTextNode(), setAttribute(), appendChild()

  ```html
  <body>
    <div id="theBox">
      <h1>태그 생성 연습</h1>
    </div>
  </body>
  <script>
    var newPTag = document.createElement("p");
    var newButtonTag1 = document.createElement("button");
    var newButtonTag2 = document.createElement("button");
    var newTest1 = document.createTextNode("버튼1");
    var newTest2 = document.createTextNode("버튼2");

    newButtonTag1.setAttribute("id", "btn1");
    newButtonTag2.setAttribute("id", "btn2");

    newButtonTag1.appendChild(newTest1);
    newButtonTag2.appendChild(newTest2);
    newPTag.appendChild(newButtonTag1);
    newPTag.appendChild(newButtonTag2);

    var theObj = document.getElementById("theBox");
    theObj.appendChild(newPTag);
  </script>
  ```

  <br />

  - cloneNode(), insertBefore(), removeChild()

  ```html
  <body>
    <h1>문서 객체 조작</h1>
    <ul id="theList">
      <li>리스트2</li>
      <li>리스트3</li>
      <li>리스트4</li>
      <li>리스트1</li>
    </ul>
  </body>
  <script>
    const theListObj = document.getElementById("theList");
    const liTagList = theListObj.getElementsByTagName("li");

    // <li>리스트1</li> 복제
    const copyLiTagList = liTagList[3].cloneNode(true);

    // 복제된 <li>리스트1</li>를 <li>리스트2</li> 앞으로 삽입
    theListObj.insertBefore(copyLiTagList, liTagList[0]);

    // 기존의 <li>리스트1</li> 삭제
    theListObj.removeChild(liTagList[4]);
  </script>
  ```

  <br />

  - innerHTML

    ```html
    <head>
      <style>
        div {
          width: 300px;
          border: 1px solid;
        }

        p {
          border: 1px solid;
        }
      </style>
    </head>
    <body>
      <h2>innerHTML</h2>
      <div id="divObj"></div>
      <div>demo</div>
    </body>
    <script>
      const divObj = document.getElementById("divObj");

      /*
      let htmlStr = "";
      htmlStr += "<p style='background-color: aqua;'>이벤트? event</p>";
      htmlStr += "<button>마우스 왼쪽 클릭</button>";
    
      divObj.innerHTML = htmlStr;
      */

      divObj.innerHTML = `
        <p style="background-color: aqua;">이벤트? event</p>
        <button>마우스 왼쪽 클릭</button>
      `;
    </script>
    ```
