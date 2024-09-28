## Conditional Rendering

- 조건에 따른 렌더링 => **조건부 렌더링**

- 어떤 조건(조건문)에 따라서 렌더링이 달라지는 것

  - ex) true변 버튼을 보여줌, false이면 버튼을 가림

  ```js
  import React from "react";

  function UserGreeting(props) {
    return <h1>다시 오셨군요!</h1>;
  }

  function GuestGreeting(props) {
    return <h1>회원 가입을 해 주세요.</h1>;
  }

  function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;

    if (isLoggedIn) {
      return <UserGreeting />;
    }

    return <GuestGreeting />;
  }
  ```

<br />

- JavaScript의 Truthy와 Falsy

  - Boolean 자료형: 참(true) / 거짓(false)

  - Truthy: true는 아니지만 true로 여겨지는 값

  - Falsy: false는 아니지만 false로 여겨지는 값

  ```js
  // truthy

  true;
  {
  } // (empty object)
  []; // (empty array)
  42; // (number, not zero)
  "0", "false"; // (string, not empty)

  // falsy
  false;
  0, -0; // (zero, minus zero)
  0n; // (BigInt zero)
  "", "", ``; // (empty string)
  null;
  undefined;
  NaN; // (not a number)
  ```

<br />

- **`Element Variables(엘리먼트 변수)`**

  - React의 Element를 변수처럼 사용

  ```js
  import React from "react";

  function LoginButton(props) {
    return <button onClick={props.onClick}>로그인</button>;
  }

  function LogoutButton(props) {
    return <button onClick={props.onClick}>로그아웃</button>;
  }

  function LoginControl(props) {
    const [isLoggedIn, setIsLoggedIn] = useState(false);

    const handleLoginClick = () => {
      setIsLoggedIn(true);
    }

    const handleLogoutClick = () => {
      setIsLoggedIn(false);
    }

    // isLoggedIn의 값에 따라 컴포넌트 대입 후 사용
    let button;
    if(isLoggedIn) {
      button = <LogoutButton onClick={handleLogoutClick} />
    } else {
      button = <LoginButton onClick={handleLoginClick} />
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn}>
        {button}
      </div>
    )
  }
  ```

<br />

- **`Inline Condition`**

  - 해당 코드를 필요한 곳 안에 사용

  - 조건문을 코드 안에 집어넣는 것

  - Inline if: if문을 필요한 곳에 직접 사용 => && 연산자 사용

    - true && expression -> expression

    - false && expression -> false

    > 단축 평가

  ```js
  import React from "react";

  function Mailbox(props) {
    const unreadMessages = props.unreadMessages;

    return (
      <div>
        <h1>안녕하세요!</h1>
        {unreadMessages.length > 0 && (
          <h2>현재 {unreadMessages.length}개의 읽지 않은 메시지가 있습니다.</h2>
        )}
      </div>
    );
  }
  ```

  ```js
  function LoginControl(props) {
    const [isLoggedIn, setIsLoggedIn] = useState(false);

    const handleLoginClick = () => {
      setIsLoggedIn(true);
    }

    const handleLogoutClick = () => {
      setIsLoggedIn(false);
    }

    return (
      <div>
        <>
      </div>
    )
  }
  ```

  <br />

  - Inline if-else: 삼항 연산자 사용

  ```js
  import React from "react";

  function UserState(props) {
    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn}>
        {isLoggedIn
          ? <LogoutButton onClick={handleLogoutClick} />
          : <LoginButton onClick={handleLoginClick} />
        }
      </div>
    );
  }
  ```

<br />

- Component 렌더링 막기

  - _null을 리턴하면 렌더링 되지 않음_

  ```js
  function WarningBanner(props) {
    if (!props.warning) {
      return null;
    }

    return <div>경고!</div>;
  }
  ```

  ```js
  function MainPage(props) {
    const [showWarning, setShowWarning] = useState(false);

    const handleToggleClick = () => {
      setShowWarning(prevShowWarning => !prevShowWarning);

    }

    return (
      <div>
        <WarningBanner warning={showWarning}>
        <button onClick={handleToggleClick}>
          {showWarning ? "감추기" : "보이기"}
        </button>
      </div>
    )
  }
  ```

  > class 컴포넌트에서 null 리턴과 생명주기는 관계없음

<br />

## List and Keys

- List

  - Array: JavaScript의 변수나 객체들을 하나의 변수로 묶어 놓은 것

- Key

  - 각 객체나 아이템을 구분할 수 있는 고유한 값

  - React의 Key: 아이템을 구분하기 위한 고유한 문자열

<br />

- 여러 개의 Component 렌더링

  - map()

    - _map() 함수 안에 있는 Elements는 반드시 key가 필요_

  ```js
  const doubled = numbers.map((number) => number * 2);
  ```

  ```js
  import React from "react";

  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) => <li>{number}</li>);

  ReactDOM.render(<ul>{listItems}</ul>, document.getElementById("root"));
  ```

  <br />

  - 기본적인 List Component

  ```js
  function NumberList(props) {
    const { numbers } = props;

    const listItems = numbers.map((number) => <li>{number}</li>);

    return (
      <ul>{listItems}</ul>
    )
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers}>,
    document.getElementById("root")
  )
  ```

  > List의 Item은 고유한 key가 있어야 한다는 경고문이 생성됨

  <br />

  - List의 Key

    - Key의 값은 같은 List에 있는 Elements 사이에서만 고유한 값이면 됨

    > 배열에서 item의 순서가 바뀔 수 있는 경우, index 사용은 지양 <br/>
    > 성능에 부정적인 영향, 컴포넌트의 state와 관련해 문제를 일으킬 수 있음

  ```js
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) => (
    <li key={number.toString()}>{number}</li>
  ));

  const todoItems = todos.map((todo) => <li key={todo.id}>{todo.text}</li>);
  ```

<br />

## **`Forms`**

- 사용자로부터 입력을 받기 위해 사용

  - HTML Form

    - 자체적으로 state를 관리

  ```html
  <form>
    <label>이름: <input name="userName" /></label>
    <button type="submit">제출</button>
  </form>
  ```

  <br />

- Controlled Components

  - 값이 리액트의 통제를 받는 Input Form Element

  - 모든 데이터를 state에서 관리 : useState()

  ```js
  import React from "react";

  function NameForm(props) {
    const [value, setValue] = useState("");

    const handleChange = (event) => {
      setValue(event.target.value);
    };

    const handleSubmit = (event) => {
      alert(`입력한 이름: ${value}`);
      event.preventDefault();
    };

    return (
      <form onSubmit={handleSubmit}>
        <label>
          이름:
          <input value={value} onChange={handleChange} />
        </label>
        <button type="submit">제출</button>
      </form>
    );
  }
  ```

  > 사용자의 입력을 직접적으로 제어할 수 있음

<br />

- Textarea 태그

  - 여러 줄에 걸쳐 긴 텍스트를 입력받기 위한 HTML 태그

  ```html
  <!-- html -->
  <textarea>
    안녕하세요, 여기에 이렇게 텍스트가 들어가게 됩니다.
  </textarea>
  ```

  <br />

  ```js
  // react
  import React from "react";

  function RequestForm(props) {
    const [value, setValue] = useState("요청 사항을 입력하세요.");

    const handleChange = (event) => {
      setValue(event.target.value);
    };

    const handleSubmit = (event) => {
      alert("입력한 요청 사항: " + value);
      event.preventDefault();
    };

    return (
      <form onSubmit={handleSubmit}>
        <label>
          요청사항:
          <textarea value={value} onChange={handleChange} />
        </label>
        <button type="submit">제출</button>
      </form>
    );
  }
  ```

<br />

- select 태그

  - drop-down 목록을 보여주기 위한 HTML 태그

  ```html
  <!-- html -->
  <select>
    <option value="apple">사과</option>
    <option value="banana">바나나</option>
    <option selected value="grape">포도</option>
    <option value="watermelon">수박</option>
  </select>
  ```

  ```js
  // react
  import React from "react";

  function FruitSelect(props) {
    const [value, setValue] = useState("grape");

    const handleChange = (event) => {
      setValue(event.target.value);
    };

    const handleSubmit = (event) => {
      alert("선택한 과일: " + value);
      event.preventDefault();
    };

    return (
      <form onSubmit={handleSubmit}>
        <label>
          과일을 선택하세요:
          <!--
            * 여러 개의 옵션 선택:
            <select multiple={true} value={["b", "c"]}></select>
          -->

          <select value={value} onChange={handleChange}>
            <option value="apple">사과</option>
            <option value="banana">바나나</option>
            <option value="grape">포도</option>
            <option value="watermelon">수박</option>
          </select>
        </label>
      </form>
    );
  }
  ```

  > selected를 사용하지 않고, **`value`**를 사용

<br />

- File Input 태그

  - 디바이스의 저장 장치로부터 하나 또는 여러 개의 파일을 선택할 수 있게 해주는 HTML 태그

  ```html
  <input type="file" />
  ```

  - 값이 읽기 전용이기에 React에서는 Uncontrolled Component

<br />

- Multiple Inputs: 여러 개의 state를 선언해 각각의 입력에 대해 사용

  ```js
  import React from "react";

  function Reservation(props) {
    const [haveBreakfast, setHaveBreakfast] = useState(true);
    const [numberOfGuest, setNumberOfGuest] = useState(2);

    const handleSubmit = (event) => {
      alert(`아침식사 여부: ${haveBreakfast}, 방문객 수: ${numberOfGuest}`);
      event.preventDefault();
    };

    return (
      <form onSubmit={handleSubmit}>
        <label>
          아침식사 여부:
          <input
            type="checkbox"
            onChange={(event) => {
              setHaveBreakfast(event.target.checked);
            }}
          />
        </label>
        <br />
        <label>
          방문객 수:
          <input
            type="number"
            value={numberOfGuest}
            onChange={(event) => {
              setNumberOfGuestI(event.target.value);
            }}
          />
        </label>
      </form>
    );
  }
  ```

<br />

- Input Null Value

  - value props는 넣되 자유롭게 입력값을 바꿀 수 있게 함: null, undefined

  ```js
  ReactDOM.render(<input value="hi" />, rootNode);

  // setTimeout에 의해 입력 가능한 값으로 만들어줌
  setTimeout(function () {
    ReactDOM.render(<input value={null} />, rootNode);
  }, 1000);
  ```

<br />

## **`Lifting State Up`**

- _하위 컴포넌트의 state를 공통 상위 컴포넌트로 올림_

- Shared State

  - state에 있는 데이터를 여러 개의 하위 컴포넌트에서 공통으로 사용하는 경우

  - 하위 컴포넌트가 공통된 부모 컴포넌트의 state를 공유해 사용

<br >

- 하위 컴포넌트에서 state 공유하기

  - 물의 끓음 여부를 알려주는 컴포넌트

  ```js
  // 하위 컴포넌트
  function BoilingVerdict(props) {
    if (props.celsius >= 100) {
      return <p>물이 끓습니다.</p>;
    }

    return <p>물이 끓지 않습니다.</p>;
  }
  ```

  ```js
  // 부모 컴포넌트
  function Calculator(props) {
    const [temperature, setTemperature] = useState("");

    const handleChange = (event) => {
      setTemperature(event.target.value);
    };

    return (
      <fieldset>
        <legend>섭씨온도를 입력하세요:</legend>
        <input value={temperature} onChange={handleChange} />
        <BoilingVerdict celsius={paresFloat(temperature)} />
      </fieldset>
    );
  }
  ```

  <br />

  - 입력 컴포넌트 추출

  ```js
  const scaleNames = {
    c: "섭씨",
    f: "화씨"
  };

  function TemperatureInput(props) {
    const [temperature, setTemperature] = useState("");

    const handleChange = (event) => {
      setTemperature(event.target.value);
    };

    return (
      <fieldset>
        <legend>온도를 입력해 주세요(단위:{scaleNames[props.scale]})</legend>
        <input value={temperature} onChange={handleChange} />
      </fieldset>
    );
  }
  ```

  ```js
  function Calculator(props) {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
  ```

  <br />

  - 온도 변환 함수 작성

    - 섭씨온도와 화씨온도를 동기화하기 위해 생성

    ```js
    function toCelsius(fahrenheit) {
      return ((fahrenheit - 32) * 5) / 9;
    }

    function toFahrenheit(celsius) {
      return (celsius * 9) / 5 + 32;
    }

    function tryConvert(temperature, convert) {
      const input = parseFloat(temperature);

      // 예외처리
      if (Number.isNaN(input)) {
        return "";
      }

      const output = convert(input);
      const rounded = Math.round(output * 1000) / 1000;

      return rounded.toString();
    }

    tryConvert("abc", toCelsius); // empty string 리턴
    tryConvert("10.22", toFahrenheit); // 50.369 리턴
    ```

    <br />

    - Shared State 적용(TemperatureInput 변경)

    ```js
    // TemperatureInput component

    const scaleNames = {
      c: "섭씨",
      f: "화씨"
    };

    function TemperatureInput(props) {
      const handleChange = (event) => {
        // 변경 전: setTemperature(event.target.value);
        props.onTemperatureChange(event.target.value);
      };

      return (
        <fieldset>
          <legend>온도를 입력해 주세요(단위:{scaleNames[props.scale]})</legend>
          <!-- 변경 전: <input value={temperature} onChange={handleChange} /> -->
          <input value={props.temperature} onChange={handleChange} />
        </fieldset>
      );
    }
    ```

    - Calculator 컴포넌트 변경

    ```js
    import React from "react";

    function Calculator(props) {
      const [temperature, setTemperature] = useState("");
      const [scale, setScale] = useState("c");

      const handleCelsiusChange = (temperature) => {
        setTemperature(temperature);
        setScale("c");
      };

      const handleFahrenheitChange = (temperature) => {
        setTemperature(temperature);
        setScale("f");
      };

      const celsius =
        scale === "f" ? tryConvert(temperature, toCelsius) : temperature;
      const fahrenheit =
        scale === "c" ? tryConvert(temperature, toFahrenheit) : temperature;

      return (
        <div>
          <TemperatureInput
            scale="c"
            temperature={celsius}
            onTemperatureChange={handleCelsiusChange}
          />
          <TemperatureInput
            scale="f"
            temperature={fahrenheit}
            onTemperatureChange={handleFahrenheitChange}
          />
          <BoilingVerdict celsius={parseFloat(celsius)}>
        </div>
      );
    }
    ```

<br />

## Composition vs Inheritance

- **`Composition`**

  - 여러 개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것 => 합성

  - **`Containment`**

    - 하위 컴포넌트를 포함하는 형태의 합성 방법

    - Sidebar나 Dialog 같은 Box 형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없음

    - children이라는 prop을 사용해서 조합

    <br />

    - children prop을 사용한 FancyBorder 컴포넌트

    ```js
    function FancyBorder(props) {
      return (
        <div className={"FancyBorder FancyBorder-" + props.color}>
          {props.children}
        </div>
      );
    }
    ```

    ```js
    function WelcomeDialog(props) {
      return (
        <FancyBorder color="blue">
          <!-- 하위 컴포넌트를 props.children으로 전달 -->
          <h1 className="Dialog-title">어서오세요</h1>
          <p className="Dialog-message">
            우리 사이트에 방문하신 것을 환영합니다!
          </p>
        </FancyBorder>
      );
    }
    ```

    - 여러 개의 children 집합이 필요한 경우

      - props를 정의해서 각각 원하는 컴포넌트를 넣어줌

    ```js
    function SplitPane(props) {
      return (
        <div className="SplitPane">
          <div className="SplitPane-left">{props.left}</div>
          <div className="SplitPane-right">{props.right}</div>
        </div>
      );
    }

    function App(props) {
      return <SplitPane left={<Contacts />} right={<Chat />} />;
    }
    ```

    <br />

  - **`Specialization`**

    - 범용적인 개념을 구별이 되게 구체화하는 것

    - 객체지향의 언어에서는 상속을 사용해 Specialization을 구현

    - 리액트에서는 합성(Composition)을 사용해 Specialization 구현

    ```js
    function Dialog(props) {
      return (
        <FancyBorder color="blue">
          <h1 className="Dialog-title">{props.left}</h1>
          <p className="Dialog-message">{props.message}</p>
        </FancyBorder>
      );
    }

    function WelcomeDialog(props) {
      return (
        <Dialog
          title="어서 오세요"
          message="우리 사이트에 방문하신 것을 환영합니다!"
        />
      );
    }
    ```

    > title, message에 따라 Dialog의 용도가 달라짐 => 경고, 안내, 환영 등

    <br />

    - Containment와 Specialization을 같이 사용하기

      - Containment을 위한 props.children 사용

      - Specialization을 위한 직접 정의한 props 사용

    ```js
    function Dialog(props) {
      return (
        <FancyBorder color="blue">
          <h1 className="Dialog-title">{props.left}</h1>
          <p className="Dialog-message">{props.message}</p>
          {props.children}
        </FancyBorder>
      );
    }

    function SignUpDialog(props) {
      const [nickname, setNickname] = useState("");

      cost handleChange = (event) => {
        setNickname(event.target.value);
      }

      const handleSignUp = () => {
        alert(`어서 오세요, ${nickname}님!`);
      }

      return (
        <Dialog
          title="화성 탐사 프로그램"
          message="닉네임을 입력해 주세요."
        >
          <!-- 위는(title, message) Specialization -->
          <!-- 아래는 Containment -->
          <input value={nickname} onChange={handleChange} />
          <button onClick={handleSignUp}>가입하기</button>
        </Dialog>
      )
    }
    ```

    <br />

- Inheritance

  - 다른 컴포넌트로부터 상속받아서 새로운 컴포넌트를 만드는 것

  - 단, 상속을 활용한 좋은 사례가 없음

```
복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 만들고,
만든 컴포넌트들을 조합해 새로운 컴포넌트를 만드는 것을 권장
```

<br />

## **`Context`**

- 기존의 props를 이용한 data 전달: 컴포넌트의 props를 통한 데이터 전달 => 단방향

  - 상위 컴포넌트에서 하위 컴포넌트로 전달

  - 데이터 전달을 위해 사용하지 않는 컴포넌트에서도 props를 받아서 전달해야 하는 상황 발생 => 반복적인 코드

<br />

- Context: 컴포넌트 트리를 통해 곧바로 컴포넌트로 데이터를 전달하는 방식

  - Context를 사용해야 하는 경우

  ```
  - 여러 개의 Component들이 접근해야 하는 데이터

    - 로그인 여부, 로그인 정보, UI 테마, 현재 언어, 캐싱된 데이터 등
  ```

  <br />

  - props 사용 방식

  ```js
  function App(props) {
    return <Toolbar theme="dark" />;
  }

  function Toolbar(props) {
    // ThemedButton에 theme를 전달하기 위해 theme prop을 가져야만 함
    // 현재 테마를 알아야 하는 모든 버튼에 대해 props로 전달하는 것은 매우 비효율적임
    return (
      <div>
        <ThemedButton theme={props.theme} />
      </div>
    );
  }

  function ThemedButton(props) {
    return <Button theme={props.theme} />;
  }
  ```

  <br />

  - context 방식

  ```js
  // 데이터를 컴포넌트 트리로 곧바로 전달
  // 현재 테마를 위한 컨텍스트 생성 -> 기본 값은 light
  const ThemeContext = React.createContext("light");

  // Provider를 사용해 하위 컴포넌트들에게 현재 테마 데이터를 전달
  // 모든 하위 컴포넌트들은 컴포넌트 트리 하단에 얼마나 깊이 있는지에 관계없이 데이터를 읽을 수 있음
  // 현재 테마 값으로 dark를 전달
  function App(props) {
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }

  function ThemedButton(props) {
    // 리액트를 가장 가까운 상위 테마 Provider를 찾아서 해당되는 값을 사용함
    // 해당되는 Provider가 없을 경우 기본값(light)을 사용
    // 상위 Provider가 있기 때문에 현재 테마의 값은 dark
    return (
      <ThemeContext.Consumer>
        {(value) => <Button theme={value} />}
      </ThemeContext.Consumer>
    );
  }
  ```

<br />

- Context를 사용하기 전에 고려할 점

  - component와 context가 연동되면 재사용성이 떨어짐

  - 다른 레벨의 많은 component가 데이터를 필요로 하는 경우가 아니라면 props로 데이터 전달

  ```js
  // Page 컴포넌트는 PageLayout 컴포넌트를 렌더링
  <Page user={user} avatarSize={avatarSize} />

  // PageLayout 컴포넌트는 NavigationBar를 렌더링
  <PageLayout user={user} avatarSize={avatarSize} />

  // NavigationBar 컴포넌트는 Link 컴포넌트를 렌더링
  <NavigationBar user={user} avatarSize={avatarSize} />

  // Link 컴포넌트는 Avatar 컴포넌트를 렌더링
  <Link href={user.permalink}>
    <Avatar user={user} avatarSize={avatarSize} />
  </Link>
  ```

  <br />

  - 변수에 저장하는 element variable 방법 사용

  ```js
  function Page(props) {
    const user = props.user;

    const userLink = (
      <Link href={user.permalink}>
        <Avatar user={user} avatarSize={avatarSize} />
      </Link>
    );

    // Page 컴포넌트는 PageLayout 컴포넌트를 렌더링
    // 이때 props로 userLink를 함께 전달함
    return <PageLayout userLink={userLink} />;
  }

  // PageLayout 컴포넌트는 NavigationBar를 렌더링
  <PageLayout userLink={...} />

  // NavigationBar 컴포넌트는 props로 전달받은 userLink element를 리턴
  <NavigationBar userLink={...} />
  ```

  > 상위 컴포넌트는 복잡해지고 하위 컴포넌트는 유연해짐

  <br />

  - 하위 컴포넌트를 여러 개의 변수로 나눠서 전달

    - 하위 컴포넌트의 의존성을 상위 컴포넌트와 분리할 필요가 있는 대부분에 경우 적합

    - 렌더링 전, 하위 컴포넌트가 상위 컴포넌트와 통신해야 하는 경우 렌더 props를 통해 처리 가능

  ```js
  function Page(props) {
    const user = props.user;

    const topBar = (
      <NavigationBar>
        <Link href={user.permalink}>
          <Avatar user={user} avatarSize={avatarSize} />
        </Link>
      </NavigationBar>
    );

    const content = <Feed user={user} />;

    return <PageLayout topBar={topBar} content={content} />;
  }
  ```

> 하나의 데이터에 다양한 레벨에 있는 중첩된 컴포넌트들이 접근할 필요가 있는 경우 context 사용

> 해당 데이터와 데이터의 변경 사항을 모두 하위 컴포넌트들에게 브로드캐스트(Broadcast)해 줌

<br />

- Context API

  - 컨텍스트 생성

  ```js
  // React.createContext()

  const MyContext = React.createContext(/* 기본 값 */);

  /*
    - 리액트에서 렌더링이 일어날 때 하위 컨텍스트 객체를 구독하는 하위 컴포넌트가 나오면 
    현재 컨텍스트의 값을 가장 가까이에 있는 상위 레벨의 Provider로부터 받아오게 됨
  
    - 상위 레벨에 매칭되는 Provider가 없다면 기본 값이 사용됨
  
    * 기본 값으로 undefined를 넣으면 기본 값이 사용되지 않음
  */
  ```

  <br />

  - 하위 컴포넌트들이 해당 컨텍스트의 데이터를 받을 수 있도록 설정

  ```js
  // Context.Provider

  <MyContext.Provider value={/* some value */}>
    <App />
  </MyContext.Provider>

  /*
    - context.Provider 컴포넌트로 하위 컴포넌트들을 감싸주면
      모든 하위 컴포넌트들이 해당 컨텍스트의 데이터에 접근 가능

    - 컨슈밍 컴포넌트: 하위 컴포넌트들이 데이터를 소비함
      => 컨텍스트의 값의 변화를 지켜보다가 값이 변경되면 리렌더링

    * 하나의 Provider 컨포넌트는 여러 개의 컨슈밍 컴포넌트와 연결될 수 있음
    * 여러 개의 Provider 컨포넌트는 중첩되어 사용 가능

    - Provider 컨포넌트로 감싸진 모든 컨슈밍 컴포넌트는 Provider의 value prop이 변경될 때마다 리렌더링 됨
    - 값이 변경되었을 때 상위 컴포넌트가 업데이트 대상이 아니더라도 하위에 있는 컴포넌트가
    context를 사용한다면 하위 컴포넌트에서는 업데이트가 일어남
      => 값의 변화를 판단하는 기준은 Object.is() 함수와 동일
  */
  ```

<br />

- Provider value에서 주의해야 할 사항

  - Provider 컴포넌트가 재렌더링될 때마다 모든 하위 consumer 컴포넌트가 재렌더링 됨

    - 컨텍스트는 재렌더링 여부를 결정할 때 레퍼런스 정보를 사용하기 때문에 Provider의 부모 컴포넌트가 재렌더링 되었을 경우 의도치 않게 컨슈머 컴포넌트가 재렌더링 일어날 수 있음

  ```js
  function App() {
    return (
      <MyContext.Provider value={{ something: "something" }}>
        <Toolbar />
      </MyContext.Provider>
    );
  }

  // 프로바이더 컴포넌트가 재렌더링 될 때마다 모든 하위 컨슈머 컴포넌트를 재렌더링 함
  // => value prop을 위한 새로운 객체가 매번 새롭게 생성되기 때문
  // => value를 직접 넣는 것이 아닌 컴포넌트의 state로 옮기고
  //    해당 state의 값을 넣어줘야 함
  ```

  ```js
  function App(props) {
    const [value, setValue] = useState({ something: "something" });

    return (
      <MyContext.Provider value={value}>
        <Toolbar />
      </MyContext.Provider>
    );
  }
  ```

  <br />

  - Class.contextType

    - Provider 하위에 있는 class 컴포넌트에서 context의 데이터에 접근하기 위해 사용

    - React.createContext 함수를 통해 생성된 context 객체가 대입될 수 있음

    - this.context를 통해 상위에 있는 Provider 중에서 가장 가까운 것의 값을 가져올 수 있음

    - 렌더 함수를 포함한 모든 생명주기 함수 어디에서든지 this.context 사용 가능

  ```js
  class MyClass extends React.Component {
    componentDidMount() {
      // MyContext의 값을 이용하여 원하는 작업 수행 가능
      let value = this.context;
    }

    componentDidUpdate() {
      let value = this.context;
    }

    componentWillUnmount() {
      let value = this.context;
    }

    render() {
      let value = this.context;
      // MyContext의 값에 따라서 컴포넌트들을 렌더링
    }
  }

  // MyContext의 데이터에 접근할 수 있게 해 줌
  // 단 하나의 컨텍스트만 구독 가능
  MyClass.contextType = MyContext;
  ```

  <br />

  - Context.Consumer

    - 컨텍스트의 데이터를 구독하는 컴포넌트

    - 컴포넌트에서 컨텍스트 구독 방법

    ```
    - class 컴포넌트: Class.contextType

    - function 컴포넌트: Context.Consumer
    ```

  ```js
  <MyContext.Consumer>
    // function as a child
    {value => /* 컨텍스트의 값에 따라서 컴포넌트들을 렌더링 */}
  </MyContext.Consumer>

  // 자식으로 들어간 함수가 현재 컨텍스트의 value를 받아서 react.node로 리턴
  // 함수로 전달되는 value는 Provider의 value prop과 동일

  // if 상위 컴포넌트에 Provider가 없다면 value 파라미터는
  // createContext를 호출할 때 넣는 기본 값과 동일한 역할을 함
  ```

    <br />

  - function as a child: 컴포넌트의 자식으로 함수를 사용하는 방법

  ```js
  // children이라는 prop을 직접 선언하는 방식
  // => 하위 컴포넌트들을 children이라는 prop으로 전달
  <Profile children={(name) => <p>이름: {name}</p>} />

  // Profile 컴포넌트로 감싸서 children으로 만드는 방식
  // => 컴포넌트 대신 함수를 사용
  <Profile>
    {(name) => <p>이름: {name}</p>}
  </Profile>
  ```

  <br />

  - Context.displayName

    - Context 객체는 displayName이라는 문자열 속성을 가짐

    - react 개발자 도구를 통해 확인 가능

  ```js
  const MyContext = React.createContext(/* some value */);
  MyContext.displayName = "MyDisplayName";

  // 개발자 도구에 "MyDisplayName.Provider"로 표시됨
  <MyContext.Provider>

  // 개발자 도구에 "MyDisplayName.Consumer"로 표시됨
  <MyContext.Consumer>
  ```

<br />

- 여러 개의 Context 사용하기

  - context.provider를 중첩

  ```js
  // 테마를 위한 컨텍스트
  const ThemeContext = React.createContext("light");

  // 로그인 한 사용자를 위한 컨텍스트
  const UserContext = React.createContext({
    name: "Guest"
  });

  class App extends React.Component {
    render() {
      const { signedInUser, theme } = this.props;

      // App component that provides initial context values
      return (
        <ThemeContext.Provider value={theme}>
          <UserContext.Provider value={signedInUser}>
            <Layout />
          </UserContext.Provider>
        </ThemeContext.Provider>
      );
    }
  }
  ```

  ```js
  function Layout() {
    return (
      <div>
        <Sidebar />
        <Content />
      </div>
    )
  }

  // 컨텍스트 컴포넌트는 두 개의 컨텍스트로부터 값을 가져와서 렌더링함
  function Context() {
    return (
      <ThemeContext.Consumer>
        {theme => (
          <UserContext.Consumer>
            {user => (
              <ProfilePage user={user} theme={theme} />
            )}
          </UserContext.Consumer>
        )}
      </ThemeContext.Consumer>>
    )
  }
  ```

  > 2개 이상의 컨텍스트의 값이 자주 함께 사용될 경우 <br /> 모든 값을 한 번에 제공해 주는 별도의 렌더 Prop 컴포넌트를 만드는 것 고려

<br />

- useContext() hook

  > 함수 컴포넌트에서 컨텍스트를 사용하기 위해 컴포넌트를 매번 컨슈머 컴포넌트로 감싸주는 것은 어려움

  - React.createContext 함수 호출로 생성된 컨텍스트 객체를 인자로 받아서 현재 컨텍스트의 값을 리턴

  - 함수 컴포넌트에서 컨텍스트를 쉽게 사용하게 해 줌

  - 컨텍스트의 값을 다른 방식과 동일하게 컴포넌트 트리상에서 가장 가까운 상위 Provider로부터 받아오게 됨

  - context의 값이 변경되면 변경된 값과 함께 useContext hook을 사용하는 컴포넌트가 리렌더링 됨

  ```js
  function MyComponent(props) {
    // useContext(context객체)
    const value = useContext(MyContext);

    return (
      // ...
    )
  }
  ```

  > useContext hook을 사용하는 컴포넌트의 렌더링이 무거운 경우 별도의 최적화 필요

<br />

## **`Styling`**

- selector(선택자): 엘리먼트에 스타일이 적용되는 규칙

  ```
  * Declaration: property(속성)와 value(값) 작성

  - selector { property: value; property: value; }

    => h1 { color: green; font-size: 16-x;}
  ```

<br />

- selector 종류

  - Element selector: 태그의 이름

  ```css
  h1 {
    color: green;
  }
  ```

  - ID selector: 태그의 id 값(중복 불가)

  ```css
  #section {
    background-color: #000;
  }
  ```

  - Class selector: 태그의 class 값(중복 가능)

  ```css
  .medium {
    font-size: 20px;
  }

  p.medium {
    font-size: 20px;
  }
  ```

  - Universal selector: \*(에스터리스크)

  ```css
  * {
    font-size: 20px;
    color: blue;
  }
  ```

  - Grouping selector: 각 선택자를 콤마로 구분해 동일 스타일 적용

  ```css
  h1,
  h2,
  p {
    color: black;
    text-align: center;
  }
  ```

<br />

- Element의 상태와 관련된 selector

  - hover: 마우스 커서가 element 위에 올라왔을 때

  - active: element가 클릭됐을 때(주로 a 태그에 사용)

  - focus: element가 초점을 갖고 있을 경우(주로 input 태그에 사용)

  - checked: radio button이나 checkbox 같은 유형의 input 태그가 체크되어 있는 경우

  - first-child, last-child: 상위 element를 기준으로 첫 번째 child, 마지막 child일 경우

  ```css
  a:hover {
    color: green;
  }

  span:active {
    transform: scale(1.5);
  }

  input:focus {
    border: 2px solid red;
  }

  div:first-child {
    background-color: yellow;
  }

  div:last-child {
    background-color: gray;
  }
  ```

<br />

- 레이아웃과 관련된 속성: 화면상 배치와 관련 있음

  - display: element를 어떻게 표시할지 결정

    - 대부분 block, inline으로 정의되어 있음

    - none: element를 화면에서 숨김

    - block: 블록 단위로 element 배치

    - inline: element를 라인 안에 넣음(width, height 값 적용 불가)

    - flex: element를 블록 레벨의 flex-container로 표시

      > flex는 container이기 때분에 내부에 다른 element들을 포함

  <br />

  - visibility: 화면에 보여주거나 감춤

    - visible: 화면에 보이게 함

    - hidden: 화면에서 안 보이게 감춤(영역은 그대로 차지)

  <br />

  - position: element를 어떻게 위치시킬 것인지 정의

    - static: element를 원래의 순서대로 위치시킴(기본 값)

    - fixed: element를 브라우저 window에 상대적으로 위치시킴

    - relative: element를 보통의 위치에 상대적으로 위치시킴

    - absolute: element를 절대 위치에 위치시킴(상위 element 기준)

<br />

- 가로, 세로 길이와 관련된 속성들

  - width: 가로 길이

  - min-width: 최소 가로 길이

  - max-width: 최대 가로 길이

  - height: 세로 길이

  - min-height: 최소 세로 길이

  - max-height: 최대 세로 길이

<br />

- FlexBox

  - flex container

    - { display: flex; }를 적용한 element

    - flex-direction: 아이템들을 배치할 방향 지정

    - align-items: 아이템들을 어떻게 정렬(교차축)할 것인지를 지정

    - justify-content: 아이템들을 어떻게 나란히(주축) 맞출 것인지를 지정

  - flex items

    - { display: flex; }를 적용한 element의 자식 element

<br />

- Font와 관련된 속성

  - font-family: "사용할 글꼴 이름", <일반적인 글꼴 분류>;

    - serif: 각 글자의 모서리에 작은 테두리를 갖고 있는 형태의 글꼴

    - sans-serif: 모서리에 테두리가 없이 깔끔한 선을 가진 글꼴(컴퓨터에 추천)

    - monospace: 모든 글자가 같은 가로길이를 가지는 글꼴(코딩할 때 주로 사용)

    - cursive: 사람이 쓴 손글씨 모양의 글꼴

    - fantasy: 장식이 들어간 형태의 글꼴

  - font-size: 글자의 크기

  - font-weight: 글자의 두께

  - font-style: 글꼴의 스타일을 지정

    - normal: 일반적인 글자의 형태

    - italic: 글자가 기울어진 형태(별도로 기울어진 형태의 글자들을 직접 디자인해서 만든 것)

    - oblique: 글자가 비스듬한 형태로 나타남(그냥 글자를 기울임)

<br />

- 기타 많이 사용하는 속성

  - background-color: 배경색 지정

  - border: 테두리 스타일 지정(단축 속성)

<br />

- **`styled-components`**

  - css 문법을 그대로 사용하면서 결과물을 스타일링 된 컴포넌트 형태로 만들어주는 오픈소스 라이브러리(컴포넌트 개념)

  ```js
  import React from "react";
  import styled from "styled-components";

  // tagged template literal을 사용해 css가 적용된 컴포넌트를 생성해 줌
  const Wrapper = styled.div`
    padding: 1em;
    background: gray;
  `;

  const Title = styled.h1`
    font-size: 1.5em;
    color: white;
    text-align: center;
  `;

  function MainPage(props) {
    return (
      <Wrapper>
        <Title>
          안녕, 리액트!
        </Title>>
      </Wrapper>>
    )
  }

  export default MainPage;
  ```

  <br />

  - styled-components 기본 사용법

    - tagged template literal을 사용해 구성요소의 스타일을 지정

    - literal: 소스 코드에 고정된 값

    - template literal: 리터럴을 템플릿 형태로 사용하는 JS 문법

      - 백틱을 사용하고 대체 가능한 익스프레션을 넣음

      ```js
      // Untagged template literal
      // 단순한 문자열
      `string text` // 여러 줄(Multi-line)에 걸친 문자열
      `string text line 1
      string text line 2` // 대체 가능한 expression이 들어있는 문자열
      `string text ${expression} string text`;

      // Tagged template literal
      // myFunction의 파라미터로 expression으로 구분된 문자열 배열과
      // expression이 순서대로 들어간 형태로 호출됨
      myFunction`string text ${expression} string text`;
      ```

      ```js
      const name = "Paul";
      const region = "NY";

      function myTagFunction(String, nameExp, regionExp) {
        let str0 = strings[0]; // "제 이름은 "
        let str1 = strings[1]; // "이고, 사는 곳은 "
        let str2 = strings[2]; // "입니다."

        //
        return `${str0}${nameExp}${str1}${regionExp}${str2}`;
      }

      const output = myTagFunction`제 이름은 ${name}이고, 사는 곳은 ${region}입니다.`;

      console.log(output);
      ```

  <br />

  - styled-components의 props 사용법

  ```js
  import React from "react";
  import styled from "styled-components";

  const Button = styled.button`
    color: ${(props) => (props.dark ? "white" : "black")};
    background: ${(props) => (props.dark ? "black" : "white")};
    border: 1px solid black;
  `;

  function Sample(props) {
    return (
      <div>
        <Button>Normal</Button>
        <Button dark>Dark</Button>
      </div>
    );
  }

  export default Sample;
  ```

  <br />

  - styled-components의 스타일 확장

  ```js
  import React from "react";
  import styled from "styled-components";

  // Button component
  const Button = styled.button`
    color: gray;
    border: 2px solid palevioletred;
  `;

  // Button에 style이 추가된 RoundedButton 컴포넌트
  const RoundedButton = styled(Button)`
    border-radius: 16px;
  `;

  function Sample(props) {
    return (
      <div>
        <Button>Normal</Button>
        <RoundedButton>Rounded</RoundedButton>
      </div>
    );
  }

  export default Sample;
  ```

<br />

## Appendix A. 리액트 버전18
