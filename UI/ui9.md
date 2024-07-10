# UI 9일차

## 9일차

- 함수

  - 내장함수: 과거의 표현

  | 기본 함수 표현   | 사용자 정의 함수             |
  | ---------------- | ---------------------------- |
  | 이미 제공되는 것 | 기본이 아닌 것, 내가 만든 것 |

<br />

- 숫자 변환 함수

  - Number(): 숫자로 변환해 주는 함수

  - parseInt(): 숫자와 문자가 포함되어 있을 경우 정수 부분만 숫자로 변환해 주는 함수

  ```html
  <body>
    <h2>Password field</h2>

    <p>The <strong>input type="password"</strong> defines a password field:</p>

    <form action="/action_page.php">
      <label for="username">Username:</label><br />
      <input type="text" id="username" name="username" /><br />
      <label for="pwd">Password:</label><br />
      <input type="password" id="pwd" name="pwd" /><br /><br />
      <input type="submit" value="Submit" />
    </form>

    <p>
      The characters in a password field are masked (shown as asterisks or
      circles).
    </p>
  </body>
  <script>
    function clickFnc() {
      let inputUserNameObj = document.getElementById("userName");
      let val1 = inputUserNameObj.value;

      let inputMoneyObj = document.getElementById("money");
      let val2 = inputMoneyObj.value;

      val1 += 10;
      console.log("val1: ", val1);

      val2 += 10;
      console.log("val2: ", val2);
    }
  </script>
  ```

<br />

- 인자 값이 있는 함수

  ```html
  <body>
    인자 값이 있는 함수
    <button onclick="greetFnc('반갑습니다');">인사1</button>
    <button onclick="greetFnc('안녕히 계세요 여러분~');">인사2</button>
  </body>
  <script>
    function greetFnc(param) {
      alert(param);
    }
  </script>
  ```

<br />

- Events

  - onmouseover, onmouseout

  ```html
  <body>
    <h1>HTML DOM Events</h1>
    <h2>The onmouseover Event</h2>

    <img
      onmouseover="bigImg(this)"
      onmouseout="normalImg(this)"
      border="0"
      src="smiley.gif"
      alt="Smiley"
      width="32"
      height="32"
    />

    <p>
      The function bigImg() is triggered when the user moves the mouse pointer
      over the image.
    </p>
    <p>
      The function normalImg() is triggered when the mouse pointer is moved out
      of the image.
    </p>
  </body>
  <script>
    function bigImg(x) {
      x.style.height = "64px";
      x.style.width = "64px";
    }

    function normalImg(x) {
      x.style.height = "32px";
      x.style.width = "32px";
    }
  </script>
  ```

  <br />

  - toggle

    - on/off처럼 두 상태 중 하나를 선택하여 사용하는 키로, 쉽게 말하면 켰다 껐다 하는 것

    - 한 번만 눌러도 그 기능이 계속 지속되는 방식으로 기능하는 키(ex: Caps Lock)

    - 일반적으로 옵션이 두 가지일 때 많이 사용

    - 토글 기능 적용

  ```html
  <body>
    <input
      onfocus="bgColorGreenFnc(this);"
      onblur="bgColorWhiteFnc(this);"
      placeholder="별명 입력"
    />
    <input
      onfocus="bgColorGreenFnc(this);"
      onblur="bgColorWhiteFnc(this);"
      placeholder="취미 입력"
    />
  </body>
  <script>
    function bgColorGreenFnc(obj) {
      obj.style.backgroundColor = "green";
    }

    function bgColorWhiteFnc(obj) {
      obj.style.backgroundColor = "#fff";
    }
  </script>
  ```

<br />

- DOM Level

  ```html
  <body>
    <p>
      When you submit the form, a function is triggered which alerts some text.
    </p>

    <form action="ch3_404eventBasic1.jsp" onsubmit="myFunction()>
      Enter name: <input name="fname" />
      <br />
      Enter 글자: <input name="tempData" />
      <input type="submit" value="데이터 전송"/>
      <input type="reset" value="초기화" />
    </form>
  </body>
  <script>
    function myFunction() {
      alert("form의 데이터들을 유효성 검사함");
    }
  </script>
  ```

  <br />

  ```html
  <body>
    <div>
      When you submit the form, a function is triggered which alerts some text.
      <img id="domLevel1" src="./images/img1.jpg" />
    </div>
  </body>
  <script>
    function myFunction() {
      alert("최초의 자바스크립트 이벤트 연결 방법");
    }

    const domLevel1Obj = document.getElementById("domLevel1");

    domLevel1Obj.onclick = myFunction;
  </script>
  ```

<br />

- 표준 이벤트 모델

  - 객체.addEventListener('이벤트', 함수)

  ```js
  객체.addEventListener((이벤트명)type, (적용할 함수)function(e) {
    // 수행할 코드
  }, (이벤트 작동 원리 설정)capture);
  ```

  - onclick 대신 표준 이벤트 방식인 addEventListener 사용

  ```html
  <body>
    <div>
      이벤트 리스너 등록 방법
      <img id="domLevel2" src="./images/img1.jpg" />
    </div>
  </body>
  <script>
    function myFunction() {
      alert(
        "다양한 브라우저에서 동작을 지원해서 표준임 \n자바스크립트 이벤트 연결 방법3"
      );
    }

    const domLevel2Obj = document.getElementById("domLevel2");

    domLevel2Obj.addEventListener("dblclick", myFunction);
  </script>
  ```

<br />

- 익명 함수

  - 변수에 함수 데이터를 저장하여 변수를 마치 함수처럼 사용할 수 있도록 만들어 줌

  - 변수 선언 이후에 호출해야 함

  ```html
  <body>
    <div>
      이벤트 리스너 등록 방법
      <img id="domLevel2" src="./images/img1.jpg" />
      <img id="otherDomTest" src="./images/img2.jpg" />
    </div>
  </body>
  <script>
    function myFunction() {
      alert(
        "다양한 브라우저에서 동작을 지원해서 표준임 \n자바스크립트 이벤트 연결 방법3"
      );
    }

    const domLevel2Obj = document.getElementById("domLevel2");

    domLevel2Obj.addEventListener("dblclick", myFunction);

    const otherDomTest = document.getElementById("otherDomTest");

    otherDomTest.addEventListener("click", function(e) {
      alert('익명 함수');
    });
  </scr
  ```

  ```js
  function myFunction() {
    alert(
      "다양한 브라우저에서 동작을 지원해서 표준임 \n자바스크립트 이벤트 연결 방법3"
    );
  }

  myFunction();

  // 익명 함수
  const fncObj = (function () {
    alert("오");
  })();

  // 즉시 실행함수
  (function () {
    alert("오");
  })();

  fncObj();
  ```
