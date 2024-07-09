# UI 8일차

## 8일차

- 구구단 2 ~ 9단을 화면에 출력

  ```html
  <head>
    <style>
      #container {
        margin: auto;
      }

      #container .gugudan_container {
        margin: 5px auto;
      }

      #container .gugudan_container span {
        display: inline-block;
        width: 100px;
        border: 1px solid;
        font-size: 18px;
        text-align: center;
      }
    </style>
  </head>
  <body></body>
  <script>
    let gugudanTagsStr = '<div id="container">';

    for (let i = 2; i <= 9; i++) {
      gugudanTagsStr += '<div class="gugudan_container">';

      for (let j = 1; j <= 9; j++) {
        gugudanTagsStr += "<span>" + i + " * " + j + " = " + i * j + "</span>";
      }

      gugudanTagsStr += "</div>";
    }

    gugudanTagsStr += "</div>";

    document.body.innerHTML = gugudanTagsStr;
  </script>
  ```

<br />

- 이벤트와 onclick 속성 사용

  ```html
  <body>
    <button onclick="testFun();">이벤트 발생</button>
  </body>
  <script>
    function testFun() {
      alert("함수는 다양하게 쓰이는데 이벤트랑 1:1로 사용하는 경우가 있다");
    }

    let tnt = 1;

    function otherFnc() {
      alert(tnt++);
    }

    otherFnc();
  </script>
  ```

<br />

- 내장객체 Date

  ```html
  <body></body>
  <script>
    document.write("<h1>현재 날짜/시간 정보</h1>");

    const today = new Date();
    let nowMonth = today.getMonth() + 1; // 현재 월(0~11)
    let nowDate = today.getDate(); // 현재 일
    let nowDay = today.getDay(); // 현재 요일 (0~6) === 일~토
    let nowHours = today.getHours(); // 현재 시간
    let nowMinutes = today.getMinutes(); // 현재 분
    let nowSeconds = today.getSeconds(); // 현재 초
    let nowTime = today.getTime(); // 1970년 1월 1일부터 밀리초 경과된 시간

    document.write("월: " + nowMonth + "<br />");
    document.write("일: " + nowDate + "<br />");
    document.write("요일: " + nowDay + "<br />");
    document.write("시: " + nowHours + "<br />");
    document.write("분: " + nowMinutes + "<br />");
    document.write("초: " + nowSeconds + "<br />");
    document.write("경과시간: " + nowTime + "<br />");

    document.write("<h1>날짜 바꾸기</h1>");

    today.setMonth(11); // 월을 12월로 지정
    today.setDate(25); // 일을 25일로 지정
    nowMonth = today.getMonth() + 1;
    nowDate = today.getDate();

    document.write("월: " + nowMonth + "<br />");
    document.write("일: " + nowDate + "<br />");
  </script>
  ```

<br />

- 내장객체 Math 사용

  ```html
  <body>
    <div id="imgContainer">
      <img id="imgTag" alt="" src="./images/img1.jpg" />
    </div>
    <input
      id="btn"
      type="button"
      value="이미지 랜덤 생성"
      onclick="randomImg();"
    />
  </body>
  <script>
    // id 값으로 img 태그를 가져옴
    function randomImg() {
      const imgTag = document.getElementById("imgTag");
      // 범위 내 난수를 랜덤으로 추출 후 소수점 내림처리 후 1 더함
      let imgNum = Math.floor(Math.random() * 3) + 1;
      imgTag.setAttribute("src", "./images/img" + imgNum + ".jpg");
    }

    // tag name으로 img 태그들을 가져와 인덱스를 통해 컨트롤
    function randomImg() {
      const imgTag = document.getElementsByTagName("img");
      // 범위 내 난수를 랜덤으로 추출 후 소수점 내림처리 후 1 더함
      let imgNum = Math.floor(Math.random() * 3) + 1;
      imgTag[0].setAttribute("src", "./images/img" + imgNum + ".jpg");
    }
  </script>
  ```

<br />

- 내장객체 String 주요 메서드

  - 자바와 매우 유사

<br />

- 자바스크립트 함수명 명명규칙

  - 기본적으로 camelCase에 fnc를 붙임

  ```js
  function testFnc() {
    //
  }
  ```

<br />

- JavaScript Array

  - 배열의 반복(for of문, for in문, forEach() 메서드)

  ```js
  // for of문
  for (let index of arr) {
    console.log(index);
  }
  ```

  ```js
  // for in문
  // 인덱스 반환
  for (let index in arr) {
    console.log(index);
  }
  ```

  ```js
  // 배열의 forEach() 메서드
  let numArr = new Array();

  numArr[0] = 10;
  numArr[1] = 20;
  numArr[2] = 30;

  numArr.forEach(function (obj) {
    console.log(obj);
  });
  ```

  <br />

  - 2차원 배열

  ```js
  // 1차원 배열(x축)
  let oneArr = new Array();
  oneArr[0] = 10;
  oneArr[1] = 20;
  oneArr[2] = 30;

  // 2차원 배열(x축, y축)
  let twoArr = new Array();
  twoArr[0] = new Array();
  twoArr[1] = new Array();

  twoArr[0][0] = 10;
  twoArr[0][1] = 20;
  twoArr[0][2] = 30;

  twoArr[1][0] = 40;
  twoArr[1][1] = 50;
  twoArr[1][2] = 60;

  alert(twoArr[0][0]);
  alert(twoArr[0]);
  alert(twoArr[1][2]);
  ```

## <br />

- for문, html 조작 연습: 구구단 곱셈에 따라 공백 일정하게 추가하기

  - getter, setter 사용

  ```js
  let gugudanStr = "";

  // 최초의 setter
  function setGugudan(putNum) {
    let blank = "";

    for (let i = 1; i <= 9; i++) {
      for (let j = 1; j <= i; j++) {
        blank += "&nbsp;";
      }
      gugudanStr +=
        "<div>" +
        putNum +
        blank +
        "*" +
        blank +
        i +
        blank +
        "=" +
        blank +
        putNum * i +
        "</div>";
      blank = "";
    }
  }

  // setter 수정
  function setGugudan2(putNum) {
    let blank = "";

    for (let i = 1; i <= 9; i++) {
      blank += "&nbsp;";
      gugudanStr +=
        "<div>" +
        putNum +
        blank +
        "*" +
        blank +
        i +
        blank +
        "=" +
        blank +
        putNum * i +
        "</div>";
    }
  }

  function getGugudan() {
    return gugudanStr;
  }

  setGugudan2(2);

  const gugudanResult = getGugudan();

  const gugudanDivTag = document.getElementById("gugudanDiv");
  gugudanDivTag.innerHTML = gugudanResult;
  ```
