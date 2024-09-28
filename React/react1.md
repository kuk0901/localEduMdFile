## react는 무엇인가

> 사용자 인터페이스(사용자와의 상호작용)를 만들기 위한 자바스크립트 라이브러리

- **`라이브러리`**: 자주 사용되는 기능들을 정리해 모아 놓은 것

  - ex): 문자열 관련 기능, 숫자 관련 기능, 날짜 관련 기능

- 사용자 인터페이스(User Interface, UI)

  - 사람(사용자)과 사물 또는 시스템, 기계, 컴퓨터 프로그램 등 사이에서 의사소통을 할 수 있도록 일시적 또는 영구적인 접근을 목적으로 만들어진 물리적, 가상적 매개체

  - 사용자와 컴퓨터 프로그램이 서로 상호작용을 하기 위해 중간에서 서로 간에 입력과 출력을 제어해 주는 것

- React: UI 라이브러리

  - 화면을 만들기 위한 기능들을 모아 놓은 것

- 인터렉션이 많은 웹 / 앱을 개발하기 위해 주로 사용

- 웹앱을 만드는 다른 tool인 Vue / Angualar와 많이 비교

- Vue / Angualar : 프레임워크

- React : 라이브러리

<br />

- **`프레임워크 vs 라이브러리`**

  - 프레임워크: 제어 권한을 프레임워크가 가짐

  - 라이브러리: 제어 권한이 개발자에게 있음

  | Framework                                              | Library                             |
  | ------------------------------------------------------ | ----------------------------------- |
  | 어떠한 앱을 만들기 위해 필요한 대부분의 것을 갖고 있음 | 어떠한 특정 기능을 모듈화 해놓은 것 |

  => 프레임 워크는 라이브러리를 포함, 작성한 소스 코드를 호출 <br/>
  => 소스 코드는 어떠한 기능을 구현하기 위해 라이브러리 호출

  <br />

  > 프레임워크는 앱을 만드는데 필요한 대부분의 라이브러리를 갖고 있음
  >
  > 라이브러리들은 특정 기능을 위해 모듈화 되어있음

<br />

- React는 프레임워크가 아닌 라이브러리 : 전적으로 UI를 렌더링하는데 관여

  ```
  - 화면을 바꾸는 라우팅 react-router-dom 모듈 사용
  - 상태 관리를 위해 redux, mobx 등 여러 모듈 사용
  - 빌드를 위해 webpack, npm 등 사용
  - 테스팅을 위해 Eslint, Mocha 등 이용

  => 리액트는 프레임워크 아닌 라이브러리
  ```

- 웹 개발의 트렌드

  - 웹사이트의 작동 원리와 흐름을 함께 이해하는 것이 중요

<br />

- 리액트의 장점

  1. 빠른 업데이트 & 렌더링 속도

     - Virtual DOM

     - DOM(Document Object Model): 웹 페이지를 정의하는 하나의 객체

     ```
     - 화면 업데이트: DOM 직접 수정(DOM 전체 수정)

     - react: 업데이트되어야 할 최소한의 부분만 검색 후 수정, render
     ```

  2. Component-Based

     - 컴포넌트들을 모아서 개발

     - 재사용성(Reusability)

       - 개발 기간 단축

       - 유지 보수 용이

     ```
     - A 프로그램의 Calendar 모듈(date 모듈 의존)
     => B 프로그램에서 date 모듈 없으면 재사용 불가


     - A 프로그램의 String 모듈
     => B 프로그램에서 독립적으로 재사용 가능

     * 의존성을 낮춰 호환성 문제를 줄임
     ```

  3. 활발한 지식 공유 & 커뮤니티

<br />

- 리액트와 재사용성

  - 컴포넌트 베이스 -> 재사용성이 높음

  > 재사용성이 높을 수 있는 컴포넌트를 만드는 것이 중요

<br />

- 리액트의 단점

  1. 방대한 학습량

  2. 높은 상태 관리 복잡도

<br />

- 리액트 사용 이유

  - 상대적으로 배우기 쉬움

  - 여러 기능들을 위해 이미 만들어져 있는 라이브러리 환경

  - 많은 기업들의 사용으로 검증이 된 라이브러리(대표적으로 페이스북)

<br />

## 브라우저가 그려지는 원리 및 가상 돔(DOM)

- React의 주요 특징 : 가상 돔(DOM) 사용

- 웹 페이지 렌더링 과정(Critical Rendering Path, CRP)

  - 웹 브라우저가 HTML 문서를 읽고, 스타일을 입히고 뷰포트에 표시하는 과정

  - 브라우저가 서버에서 페이지에 대한 HTML 응답을 받고 화면에 표시하기 전 여러 단계를 걸침

  1. DOM tree 생성 : 렌더 엔진이 문서를 읽어들여 그것들을 파싱하고 어떤 내용을 페이지에 렌더링할지 결정

  2. Render tree 생성 : 브라우저가 DOM(HTML), CSSOM(CSS)을 결합하는 곳으로 이 프로세스는 화면에 보이는 모든 콘텐츠와 스타일 정보를 모두 포함하는 최종 렌저링 트리를 출력함 => 화면에 표시되는 모든 노드의 콘텐츠 및 스타일 정보를 포함

  3. Layout(reflow) : 브라우저가 페이지에 표시되는 각 요소의 크기와 위치를 계산

  4. Paint : 실제 화면에 그림

  <br />

  > 어떤 인터렉션에 의해 DOM에 변화 발생시 Render Tree가 재생성
  >
  > 즉, 모든 요소들의 스타일을 다시 계산 등 Layout, Repaint 과정까지 다시 거치게 됨
  >
  > => 인터렉션이 많은 경우 작은 변화로 인해 필요 과정을 반복하게 되니 불필요하게 DOM을 조작하는 비용이 커짐

<br />

- 문제 해결을 위한 가상 돔(Virtual DOM) : 실제 DOM을 메모리에 복사해준 것

  - 가상 돔 작동 : 데이터가 바뀌면 가상 돔(Virtual DOM)에 렌더링 되고 이전에 생긴 가상 돔(Virtual DOM)과 비교해 바뀐 부분만 실제 돔(DOM)에 적용 시킴

  - Diffing : 바뀐 부분을 찾는 과정

  - 재조정(reconciliation) : 바뀐 부분만 실제 돔(DOM)에 적용

  <br />

  > if 요소가 30개 변했다고 해도 한 번에 묶어, 한 번의 실제 돔(DOM) 수정으로 처리
  >
  > => 돔(DOM) 조작 비용 절감

<br />

## react start

- 기존 React 설치 : webpack || Babel 같은 모듈 설치, 설정

- webpack : 오픈 소스 자바스크립트 모듈 번들러, 여러 개로 나눠진 파일들을 하나의 자바스크립트 코드로 압축하고 최적화하는 라이브러리

  - 장점
    1. 여러 파일의 자바스크립트 코드를 압축 및 최적화 => 로딩에 대한 네트워크 비용 절감
    2. 모듈 단위로 개발 가능 => 가독성과 유지보수가 쉬움

<br />

- Babel : 최신 자바스크립트 문법을 지원하지 않는 브라우저들을 위해 최신 자바스크립트 문법을 구형 브라우저에서도 돌 수 있게 변환 시켜주는 라이브러리

  ```javascript
  // Babel Input: ES6(ES2015) arrow function
  [1, 2, 3].map((n) => n + 1);

  // Babel Output: ES5 equivalent
  [1, 2, 3].map(function (n) {
    return n + 1;
  });
  ```

<br />

- 현재의 React 설치 : create-react-app 을 사용해 설치, Babel || Webpack 설정이 이미 되어있기에 시간 절감 가능

  1. React 앱을 만들 폴더 생성
  2. 터미널 실행
  3. **npx create-react-app**

<br />

- npx create-react-app ./
  - npx : 노드 패키지 시행을 도와주는 도구 create-react-app이란 npm 레지스토리에 있는 패키지를 react-todo-app 폴더에서 실행해 리액트를 설치 해줌

<br />

- 명령어

  ```bash
  $ npx create-react-app <your-project-name>
  ```

- 조건

  - Node.js v14.0.0 이상

  - npm v6.14.0 이상

- 일반적으로 사용하는 툴

  - VSCode

```bash
# 경로 변경(change directory)
$ cd my-app

# 애플리케이션 실행
$ npm start
```

<br />

## create-react-app으로 설치된 리액트 기본 구조

- 이름 수정 불가 파일들 : **public/index.html(페이지 템플릿)** || **src/index.js(자바스크립트 시작점)**

- public : public 내부의 파일만 public/index.html에서 사용 가능

- src : 대부분의 리액트 소스 코드 작성 폴더(JS, CSS 파일), Webpack은 이곳의 파일만 확인 이외의 폴더에서 파일 작성시 Webpack에 의해 처리 X

- package.json : 해당 프로젝트에 대한 정보 작성

  1. 프로젝트 이름, 버전, 필요한 라이브러리와 라이브러리의 버전들 명시
  2. 앱을 시작할 때 사용할 스크립트, 앱을 빌드할 때 / 테스트할 때 사용할 스크립트 등 명시

  - dependencies : 필요한 라이브러리, 라이브러리의 버전들 명시
  - scripts : 리액트 앱 실행, 빌드, 테스트 등의 스크립트 명시 / 프로젝트에서 자주 실행해야 할 명령어 작성시 npm 명령어로 실행 가능
  - eslintConfig : 소스 코드 입력 시의 문법, 코드 포맷을 체크해주는 것에 대한 설정 명시

<br />

## SPA(Single Page Application)

- 웹 사이트의 전체 페이지를 하나의 페이지에 담아 동적으로 화면을 바꿔가며 표현
- 전통적인 웹 사이트 : Multi Page Application(a.html / b.html / ...)

- 화면 변경(페이지 전환) : HTML5의 History API를 사용, 자바스크립트 영역에서 History API를 이용해 현재 페이지 내에서 화면 이동이 일어난 것처럼 작동
- React에서의 화면 변경 : React-Router-DOM => History API 사용

```javascript
// 세션 기록의 바로 뒤 페이지로 이동하는 비동기 메서드, 브라우저의 뒤로 가기를 누르는 것과 같은 효과
History.back();

// 세션 기록의 바로 앞 페이지로 이동하는 비동기 메서드, 브라우저의 앞으로 가기를 누르는 것과 같은 효과
History.forward();

// 특정한 세션 기록으로 이동하게 해 주는 비동기 메서드, 1을 넣어 호출하면 바로 앞 페이지로 / -1을 넣어 호출하면 바로 뒤 페이지로 이동
History.go();

// 주어진 데이터를 세션 기록 스택에 넣음, 직렬화 가능한 모든 Javascript 객체를 저장하는 것 가능
History.pushState();

// 최근 세션 기록 스택의 내용을 주어진 데이터로 교체
History.replaceState();
```

<br />

## JSX(Javascript syntax extension)

- 개념

  - A syntax extension to JavaScript

  - 자바스크립트의 확장 문법, 리액트에서는 JSX를 이용해 화면에서 UI가 보이는 모습을 나타냄

  - JavaScript + XML/HTML

    ```js
    // JSX 문법
    const element = <h1>Hello, World!</h1>;
    ```

    - JSX는 createElement를 쉽게 사용하기 위해 사용 => 모든 UI를 만들때 마다 createElement를 사용해 컴포넌트를 만들 수는 없음, JSX 사용 후 Babel이 createElement로 바꿔서 사용

<br />

- JSX 역할

  - XML, HTML 코드를 JavaScript 코드로 변환

  - React의 createElement 함수 사용 => JavaScript 객체로 변환시킴

  ```js
  // JSX 사용 코드
  class Hello extends React.Component {
    render() {
      return <div>Hello {this.props.toWhat}</div>;
    }
  }

  ReactDOM.render(<Hello toWhat="World" />, document.getElementByID("root"));
  ```

  ```js
  // JSX를 사용하지 않는 코드
  class Hello extends React.Component {
    render() {
      return React.createElement("div", null, `Hello ${this.props.toWhat}`);
    }
  }

  ReactDOM.render(
    React.createElement(Hello, { toWhat: "World" }, null),
    document.getElementById("root")
  );
  ```

  > 최종적으로 JavaScript 코드로 변환됨

  <br />

  - React.createElement()의 결과로 아래와 같은 객체가 생성됨

  ```js
  // React element
  const element = {
    type: "h1",
    props: {
      className: "greeting",
      children: "Hello, World"
    }
  };

  // 이 객체를 읽어 element를 생성
  ```

  ```js
  React.createElement(
    type, // DOM 요소
    [props], // 속성
    [...children] // 자식 element
  );
  ```

  > 리액트에서 JSX가 필수는 아니지만 장점이 많기에 권장됨

<br />

- JSX의 장점

  1. 간결한 코드

     ```js
     // JSX 사용
     <div>Hello, {name}</div>;

     // JSX 사용 X
     React.createElement("div", null, `Hello, ${name}`);
     ```

  2. 가독성 향상

     - 버그를 발견하기 쉬움

  3. Injection Attacks 방어

     - 소스 코드를 작성해 보안을 뚫는 것 방지 => 대부분 사용자 입력에 의함 => 유효성 검사 필요

     - ex) id를 입력하는 input에 js 코드를 쓰는 것 등

     ```js
     const title = response.potentiallyMaliciousInput;

     // 이 코드는 안전
     const element = <h1>{title}</h1>;
     ```

<br />

- JSX 사용법

  - JavaScript 코드 + XML/HTML

  ```js
  const name = "Ollie";
  const element = <h1>안녕, {name}</h1>;

  ReactDOM.render(element.document.getElementById("root"));
  ```

  <br />

  - ...XML/HTMl {JavaScript코드} ...XML/HTML

  ```js
  function formatName(user) {
    return user.firstName + " " + user.lastName;
  }

  const user = {
    firstName: "Inje",
    lastName: "Lee"
  };

  const element = <h1>Hello, {formatUser(user)}</h1>;

  ReactDOM.render(element, document.getElementById("root"));
  ```

  ```js
  function getGreeting(user) {
    if (user) {
      return <h1>Hello, {formatName(user)}!</h1>;
    }

    return <h1>Hello, Stranger.</h1>;
  }
  ```

  <br />

  - 태그의 속성(Attribute)에 값을 넣는 방법

  ```js
  // 큰 따옴표 사이에 문자열을 넣음
  const element1 = <div tabIndex="0"></div>;

  // 중괄호 사이에 자바스크립트 코드를 넣음
  const element2 = <img src={user.avatarUrl} />;
  ```

  <br />

  - 자식(children)을 정의하는 방법

  ```js
  const element = (
    <div>
      <h1>안녕하세요</h1>
      <h2>리액트 공부합니다</h2>
    </div>
  );
  ```

<br />

- JSX 사용에 주의해야 할 문법(문법 / 규칙)

  1. 처음으로 JSX는 컴포넌트에 여러 엘리먼트 요소가 있다면 반드시 부모 요소 하나로 감싸줘야 함

     ```javascript
     // X, 잘못된 코드
     function hello() {
       return (
         <div>Hello world!</div>
         <div>what are you doing</div>
       )
     }

     // O, 올바른 코드
     function hello() {
       return <div>
         <div>Hello world!</div>
         <div>what are you doing</div>
       </div>
     }
     ```

<br />

## Rendering Elements

- Elements

  - 어떤 물체를 구성하는 성분(요소)

  - 리액트 앱을 구성하는 가장 작은 블록들

<br />

- Descriptor -> Element

  - 화면에 나타내는 내용을 기술하는 자바스크립트 객체

  ```
  "Virtual DOM"

    - React Elements

      - DOM Elements의 가상 표현

      - 화면에서 보이는 것들을 기술

  => render

  "Browser DOM"

    - DOM Elements

    - 상대적으로 Virtual DOM보다 무거움
  ```

<br />

- 리액트 Elements의 생김새

  - React Elements는 자바스크립트 객체 형태로 존재

  ```js
  function Button(props) {
    return (
      <button className={`bg-${props.color}`}>
        <b>{props.children}</b>
      </button>
    );
  }

  function ConfirmDialog(props) {
    return (
      <div>
        <p>내용을 확인하셨으면 확인 버튼을 눌러 주세요.</p>
        <Button color="green">확인</Button>
      </div>
    );
  }

  // component rendering을 위해 모든 컴포넌트가
  // createElement() 함수를 통해 element로 변환됨
  {
    type: "div",
    props: {
      children: [
        {
          type: "p",
          props: {
            children: "내용을 확인하셨으면 확인 버튼을 눌러주세요."
          }
        },
        {
          type: Button, // React Component
          props: {
            color: "green",
            children: "확인"
          }
        }
      ]
    }
  }
  ```

<br />

- React Elements의 특징

  - **immutable(불변성)**

    - "Elements 생성 후에는" children이나 attributes를 바꿀 수 없음

  ```
  Virtual DOM

  State Change -> Compute Diff -> Re-render
  ```

  <br />

  - Elements 렌더링하기

  ```html
  <!-- Root DOM Node -->
  <div id="root"></div>
  ```

  ```js
  const element = <div>안녕, 리액트!</div>;
  ReactDOM.render(element, document.getElementById("root"));
  // React Element는 Virtual DOM에 존재
  // DOM Element는 실제 DOM에 존재
  ```

  > React Elements가 렌더링 하는 과정: Virtual DOM에서 실제 DOM으로 이동하는 과정

<br />

- 렌더링 된 Elements를 업데이트하기

  ```js
  function tick() {
    const element = (
      <div>
        <h1>Hello, React!</h1>
        <h2>Time: {new Date().toLocaleTimeString()}</h2>
      </div>
    );

    ReactDOM.render(element, document.getElementById("root"));
  }

  setInterval(tick, 1000);
  ```

  > 불변성을 위해 새로 만들어야 함

<br />

## Components and Props

- **`Components`**

  - 리액트로 만들어진 앱을 이루는 최소한의 단위

  - Component-Based

    - 컴포넌트들을 모아서 개발

    ```
    입력 -> JavaScript function -> 출력


    입력(Props) -> React Component -> 출력(React Element)
    ```

<br />

- **`Props(property)`**

  - 개념

    - 상속하는 **부모 컴포넌트로부터 자식 컴포넌트에** **`데이터 등을 전달`** 하는 방법

  ```
  - Component의 속성

  - 컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체
  ```

  <br />

  - 특징

    - props는 읽기 전용(immutable)으로 자식 컴포넌트 입장에서는 변하지 않음 => 변하게 하고자 하기 위해선 부모 컴포넌트에서 state 변경

  ```
  - React Component의 props(값)는 변경할 수 없음

  - 새로운 props(값)을 컴포넌트에 전달해 새로 Element를 생성

  - 모든 리액트 컴포넌트는 그들의 Props에 관해서는 "순수 함수" 같은 역할을 해야 함

    => 모든 리액트 컴포넌트는 Props를 직접 바꿀 수 없고,
       같은 Props에 대해서는 항상 같은 결과를 보여줘야 함
  ```

  > props 전달하기는 React 앱에서 부모 => 자식으로 정보가 어떻게 흘러가는지 알려줌

  <br />

  - JavaScript 함수의 속성

  ```js
  // pure: 입력값(input)을 변경하지 않으며, 같은 입력값에 대해서는 항상 같은 출력값(output)을 리턴
  function sum(a, b) {
    return a + b;
  }
  ```

  ```js
  // 값 변경 => Impure
  function withdraw(account, amount) {
    account.total -= amount;
  }
  ```

  <br />

  - 사용법

  ```js
  function App(props) {
    return (
      <Profile name="Dollie" introduction="Hello, I'm Dollie" viewCont={1500} />
    );
  }

  function App(props) {
    return (
      <Layout
        width={2560}
        height={1440}
        header={<Header title="Walter blog" />}
        footer={<Footer />}
      />
    );
  }

  // Object 형태로 Props가 모두 전달
  ```

<br />

- Component 만들기

  1. Function Component

     ```js
     function Welcome(props) {
       return <h1>안녕, {props.name}</h1>;
     }
     ```

  <br />

  2. Class Component

     ```js
     class Welcome extends React.Component {
       render() {
         return <h1>안녕, {props.name}</h1>;
       }
     }
     ```

  <br />

  - Component의 이름

    - 항상 대문자로 시작

    ```js
    // HTML div 태그로 인식
    const element1 = <div />;

    // Welcome이라는 React Component로 인식
    const element2 = <Welcome name="인제" />;
    ```

  <br />

  - Component 렌더링

    ```js
    // DOM 태그를 이용한 element
    const element1 = <div />;

    // 사용자가 정의한 Component를 사용한 element
    const element2 = <Welcome name="인제" />;
    ```

    ```js
    function Welcome(props) {
      return <h1>안녕, {props.name}</h1>;
    }

    const element = <Welcome name="인제" />;
    ReactDOM.render(element, document.getElementById("root"));
    ```

  <br />

  - Component 합성

    - Component 안에 또 다른 Component를 쓸 수 있음

    - 복잡한 화면을 여러 개의 Component로 나눠서 구현 가능

    ```js
    function Welcome(props) {
      return <h1>안녕, {props.name}</h1>;
    }

    // Welcome 컴포넌트를 3개 가짐
    function App(props) {
      return (
        <div>
          <Welcome name="Winifred" />
          <Welcome name="Charlie" />
          <Welcome name="Janie" />;
        </div>
      );
    }

    ReactDOM.render(<App />, document.getElementById("root"));
    ```

    > 재사용 가능한 Component를 많이 갖고 있을수록 개발 속도가 빨라짐

<br />

## State and Lifecycle

- **`상태(state)`**

  - 리액트 Component의 변경할 수 있는 상태(데이터)

  - 해당 **`컴포넌트 내부에서 데이터 전달`** 하는 방법 ex) 검색 창에 글을 입력할 때 글이 변하는 것 => state 변경

  - 개발자가 정의

  - state는 변경 가능(mutable)하며 state가 변경될 경우 re-render

  - 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 함

  - state: JavaScript 객체

  ```js
  class LikeButton extends React.Component {
    constrictor(props) {
      super(props);

      // state 정의
      this.state = {
        liked: false
      };
    }
  }
  ```

  > state는 직접 수정할 수 없음 -> 하면 안 됨

  ```js
  // state를 직접 수정(잘못된 사용법
  this.state = {
    name: "Inge"
  };

  // setState 함수를 통한 수정(정상적인 사용법)
  this.setState({
    name: "Inge"
  });
  ```

  - 리액트에서 데이터가 변할 때 화면을 다시 렌더링 해주기 위해서는 React State 사용

  - React State : 컴포넌트의 렌더링 결과물에 영향을 주는 데이터를 갖고 있는 객체

    - State 변경 시 컴포넌트는 리렌더링(Re-rendering)

    - State는 컴포넌트 안에서 관리됨

<br />

- **`Lifecycle(생명주기)`**

  - 리액트 Component의 생명주기

  - 리액트 클래스 컴포넌트의 생명주기

    - class Component의 생명주기 메서드: componentDidMount, componentDidUpdate, componentWillUnmount

  ```
  * 순서

  - 출생(Mounting): constructor -> render -> React updates DOM and refs -> componentDidMount


  - 인생(Updating): New props, setState(), forceUpdate() -> render -> React updates DOM and refs -> componentDidUpdate


  - 사망(Unmounting): componentWillUnmount

  * unmount: 상위 컴포넌트에서 해당 컴포넌트를 더 이상 화면에 표시하지 않을 때
  ```

  > Component가 계속 존재하는 것이 아닌, 시간의 흐름에 따라 생성되고 업데이트되다가 사라짐

<br />

## super(props)

- constructor()

  - 생성자 사용시 인스턴스화된 객체에서 다른 메서드 호출 전에 수행해야 하는 사용자 지정 초기화 제공

  - 클래스를 new 키워드 붙여 인스턴스 객체로 생성하면 넘겨받은 인수와 함께 constructor가 먼저 실행됨

  ```javascript
  // A 컴포넌트
  class User {
    constructor(name) {
      this.name = name;
    }

    sayHi() {
      alert(this.name);
    }
  }

  const user = new User("Douglas");
  user.sayHi();
  ```

<br />

- super()

  - 자식 클래스 내에서 부모 클래스의 생성자를 호출할 때 사용

  - 자식 클래스 내에서 부모 클래스의 메서드를 호출할 때 사용

  ```javascript
  class Car {
    constructor(brand) {
      this.carName = brand;
    }

    present() {
      return "I have a " + this.carName;
    }
  }

  class Model extends Car {
    constructor(brand, mod) {
      super(brand); // 부모 생성자 호출
      this.model = mod;
    }

    show() {
      return super.present() + ", it is a " + this.model;
    }
  }

  const myCar = new Model("Ford", "Mustang");
  myCar.show();
  ```

<br />

- super() 이후에 this 키워드

  - 생성자에서는 super 키워드 하나만 사용되거나 this 키워드가 사용되기 전에 호출되어야 함

<br />

- React에서 super에 props를 인자로 전달하는 이유

  - React.Component 객체가 생성될 때 props 속성을 초기화하기 위해 부모 컴포넌트에게 props 전달

  - 생성자 내부에서도 this.props를 정상적으로 사용할 수 있도록 보장하기 위함

<br />

## 부모 컴포넌트에서 State 보관

- 여러 개의 자식으로부터 데이터를 모으거나 두 개의 자식 컴포넌트들이 서로 통신하게 하려면 부모 컴포넌트에 공유 state를 정의해야 함

- 부모 컴포넌트는 props를 사용하여 자식 컴포넌트에 state를 다시 전달할 수 있음

  > 자식 컴포넌트들이 서로 || 부모 컴포넌트와 동기화하도록 함

<br />

- Board에 생성자를 추가하고 9개의 사각형에 해당하는 9개의 null 배열을 초기 state로 설정

  ```javascript
  [null, null, null, null, null, null, null, null, null];
  ```

<br />

## 리액트 불변성 지키기

- 불변성 : 값이나 상태를 변경할 수 없는 것

- 자바스크립트 타입을 통한 불변성 의미

  - 원시 타입(불변성 가짐) : 고정된 크기로 Call Stack 메모리에 저장, 실제 **데이터가 변수에 할당**

  - 참조 타입(가변성 가짐) : 데이터 크기가 정해지지 않고 Call Stack 메모리에 저장, 데이터의 값이 heap에 저장되며 **변수에 heap 메모리 주소값이 할당**

  | 원시 타입                                        | 참조 타입     |
  | ------------------------------------------------ | ------------- |
  | Boolean, String, Number, null, undefined, Symbol | Object, Array |

  <br />

  ```
  기본적으로 Javascript는 원시 타입에 대한 참조 및 값을 저장하기 위해 Call Stack 메모리 공간을 사용하지만, 참조 타입의 경우 Heap이라는 별도의 메모리 공간을 사용함. 이 경우 Call Stack은 개체 및 배열 값이 아닌 메모리에서만 Heap 메모리 참조 ID 값으로 저장.
  ```

  <br />

- 불변성을 지켜야 하는 이유

  1. 참조 타입에서 객체나 배열의 값이 변할 때 원본 데이터가 변경되기에 원본 데이터를 참조하고 있는 다른 객체에서 예상치 못한 오류가 발생할 수 있어 프로그래밍의 복잡도가 올라감

  2. 리액트에서 화면을 업데이트할 때 불변성을 지켜 값을 이전 값과 비교해 변경된 사항을 확인한 후 업데이트하기 때문에 불변성을 지켜줘야 함

<br />

- 불변성을 지키는 방법

  - 참조 타입에서는 값을 바꿨을 때 Call Stack 주소 값은 같은데 Heap 메모리 값만 바꿔주기에 불변성을 유지할 수 없으므로 아예 새로운 배열을 반환하는 메서드 사용

  - 새로운 배열을 반환하는 메서드 : 전개연산자(spread operator), map, filter, slice, reduce

  - 원본 데이터를 변경하는 메서드 : splice, push

  ```javascript
  const arr = [1, 2, 3, 4];
  const sameArr = arr;

  sameArr.push(5);
  console.log(arr === sameArr); // true
  ```

  ```javascript
  const arr = [1, 2, 3, 4];
  const differentArr = [...arr, 5];

  console.log(arr === differentArr); // false
  ```

<br />

## **`Hooks`**

- ReactCont 2018에서 발표된 class없이 state를 사용할 수 있는 새로운 기능

- React component

  | Class Component   | Functional Component |
  | ----------------- | -------------------- |
  | 더 많은 기능 제공 | 더 적은 기능 제공    |
  | 더 긴 코드 양     | 짧은 코드 양         |
  | 더 복잡한 코드    | 더 심플한 코드       |
  | 더딘 성능         | 더 빠른 성능         |

- 기존 component 차이점

  - function component

    - state 사용 불가

    - lifecycle에 따른 기능 구현 불가

  - class component

    - 생성자에서 state 정의

    - setState() 함수를 통해 state 업데이트

    - lifecycle methods 제공

<br />

- Hooks 등장(모두 use로 시작)

  - 함수형 컴포넌트에서도 생명주기 사용 가능

    1. 데이터를 가져올 수 있음

    2. 컴포넌트 시작과 동시에 API 호출 가능

  <br />

  - state 관련 함수: useState()

    - [변수명, set함수명] = useState(초깃값);

    ```js
    import React, { useState } from "react";

    const Counter = (props) => {
      // 변수 각각에 대한 setState 함수 존재
      const [count, setCount] = useSate(0);

      return (
        <div>
          <p>총 {count}번 클릭했습니다.</p>
          <button onClick={() => setCount(count + 1)}>클릭</button>
        </div>
      );
    };
    ```

  <br />

  - lifecycle 관련 함수

    - Side effect(효과, 영향)를 수행하기 위한 Hook: **`useEffect()`**

    - 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없는 side effect를 컴포넌트에서 실행할 수 있게 해 줌

    - useEffect(이펙트 함수, 의존성 배열);

      ```
      - class Component에선 생명주기를 이용할 때 componentDidMount, ComponentDidUpdate,
        componentWillUnmount 각각 다르게 처리


      - 리액트 훅을 사용할 때는 **useEffect** 안에서 모두 처리
      ```

    - mount, unmount 시에 단 한 번만 실행: 의존성 배열을 빈 배열화

    - 의존성 배열 생략 시 컴포넌트가 업데이트될 때마다 호출

    - ex) 서버에서 데이터를 받아옴, side effect 유발

    ```js
    useEffect(() => {
      // 컴포넌트가 마운트 된 이후,
      // 의존성 배열에 있는 변수중 하나라도 값이 변경되었을 때 실행
      // 의존성 배열에 빈 배열([])을 넣으면 마운트와 언 마운트 시에 단 한 번씩만 실행
      // 의존성 배열 생략 시 컴포넌트 업데이트마다 실행
      ...

      return () => {
        // 컴포넌트가 마운트 해제되기 전에 실행됨
        ...
      }
    }, [의존성 변수1, 의존성 변수2, ...])
    ```

    ```js
    import React, { useState, useEffect } from "react";

    const Counter = (props) => {
      // 변수 각각에 대한 setState 함수 존재
      const [count, setCount] = useSate(0);

      // componentDidMount, componentDidUpdate와 비슷하게 동작
      useEffect(() => {
        // 브라우저 API를 사용해서 document의 title을 업데이트
        document.title = `You clicked ${count} times`;
      });

      return (
        <div>
          <p>총 {count}번 클릭했습니다.</p>
          <button onClick={() => setCount(count + 1)}>클릭</button>
        </div>
      );
    };
    ```

    ```js
    import React, { useState, useEffect } from "react";

    function UserStatus(props) {
      const [isOnline, setIsOnline] = useState(null);

      function handleStatusChange(status) {
        setIsOnline(status.inOnline);
      }

      useEffect(() => {
        ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);

        // 컴포넌트가 unmount 될 때 호출
        return () => {
          ServerAPI.unsubscribeUserStatue(props.user.id, handleStatusChange);
        };
      });

      if (isOnline === null) {
        return "대기중 ...";
      }

      return inOnline ? "온라인" : "오프라인";
    }
    ```

  <br />

  - 최적화 관련 함수

    > Memoization: 결괏값이 바뀌지 않는 경우 값을 재사용하기 위해 저장

    1. **`useMemo()`**: Memoized value를 리턴하는 Hook

       - 의존성 배열을 넣지 않을 경우, 렌더링마다 함수가 실행

       - 의존성 배열이 빈 배열인 경우, 컴포넌트 마운트 시에만 호출

       ```js
       const memoizedValue = useMemo(() => {
         // 연산량이 높은 작업을 수행하여 결과를 반환
         return computeExpensiveValue(의존성 변수1, 의존성 변수2);
       }, [의존성 변수1, 의존성 변수2]);
       ```

      <br />

    2. **`useCallback()`**: useMemo() Hook과 유사하지만, 값이 아닌 함수를 반환

       - 컴포넌트가 렌더링 될 때마다 함수를 재정의 하는 것이 아닌 의존성 배열의 값이 바뀐 경우에만 함수를 재정의해서 리턴

       ```js
       const memoizedCallback = useCallback(() => {
        // 연산량이 높은 작업을 수행하여 결과를 반환
        return doSomething(의존성 변수1, 의존성 변수2);
       }, [의존성 변수1, 의존성 변수2]);
       ```

       - 동일한 역할을 하는 두 줄의 코드

       ```js
       useCallback(함수, 의존성배열);
       useMemo(() => 함수, 의존성배열);
       ```

       ```js
       import { useState } from "react";

       function ParentComponent(props) {
         const [count, serCount] = useState(0);

         // 재렌더링 될 때마다 매번 함수가 새로 정의됨
         const handleClick = (event) => {
           // 클릭 이벤트 처리
         };

         // 컴포넌트가 마운트 될 때만 함수가 정의됨: 의존성 배열이 빈 배열
         // 함수가 다시 정의되지 않는 경우에는 자식 컴포넌트도 재렌더링이 일어나지 않음
         const handleClick = useCallback((event) => {
           // 클릭 이벤트 처리
         }, []);

         return (
           <div>
             <button
               onClick={() => {
                 setCount(count + 1);
               }}
             >
               {count}
             </button>

             <ChildComponent handleCLick={handleClick} />
           </div>
         );
       }
       ```

    <br />

    3. **`useRef()`**: Reference를 사용하기 위한 Hook

       - Reference: 특정 컴포넌트에 접근할 수 있는 객체

       - Reference 객체를 반환

       - refObject.current: 현재 참조하고 있는 Element

       - 렌더링 할 때마다 같은 reference 객체를 반환

       - 내부의 데이터가 변경되었을 때 별도로 알리지 않음

       - 언마운트 전까지 값을 계속 유지함

       > current 속성을 변경한다고 해도 re-rendering이 일어나지 않음

       ```js
       // 해당 초깃값으로 초기화된 reference 반환
       // 컴포넌트가 마운트 해제되기 전까지 계속 유지
       // 변경 가능한 current
       const refContainer = useRef(초깃값);
       ```

       ```js
       import { useRef } from "react";

       function TextInp-utWithFocusButton(props) {
        const inputElem = useRef(null);

        const onButtonClick = () => {
          // current는 마운트된 input element를 가리킴
          inputElem.current.focus();
        }

        return (
          <>
            <!-- node가 변경될 때마다 current 속성에 현재 해당하는 DOM node를 저장 -->
            <input ref={inputElem} type="text" />
            <button onClick={onButtonClick}>Focus the input</button>
          </input>
        )
       }
       ```

        <br />

       - Callback ref

         - ref의 DOM 노드가 연결되거나 분리되었을 경우 코드 실행을 원할 경우 사용

         - DOM 노드의 변화를 알 수 있는 기초적인 방법

         - react는 ref가 다른 노드에 연결될 때마다 callback을 호출

       ```js
        function MeasureExample(props) {
          const [height, serHeight] = useState(0);

          const measuredRef = useCallback(node => {
            if (node !== null) {
              setHeight(node.getBoundingClientRect().height);
            }
          }, []);


          return (
            <>
              <h1 ref={measuredRef}>안녕, 리액트</h1>
              <h2>위 헤더의 높이는 {Math.round(height)}px 입니다.</h2>
            </h1>
          )
        }
       ```

<br />

- ref vs state

  - 렌더링에 정보를 사용할 때 해당 정보를 state로 유지

  - 이벤트 핸들러에게만 필요한 정보이고 변경이 일어날 때 리렌더링이 필요하지 않다면, ref를 사용하는 것이 더 효율적

  | refs                                                                     | state                                                                                              |
  | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
  | useRef(initialValue) 는 { current: initialValue } 을 반환                | useState(initialValue) 은 state 변수의 현재 값과 setter 함수 [value, setValue] 를 반환             |
  | state를 바꿔도 리렌더 되지 않음                                          | state를 바꾸면 리렌더 됨                                                                           |
  | Mutable-렌더링 프로세스 외부에서 current 값을 수정 및 업데이트할 수 있음 | ”Immutable”—state 를 수정하기 위해서는 state 설정 함수를 반드시 사용하여 리렌더 대기열에 넣어야 함 |
  | 렌더링 중에는 current 값을 읽거나 쓰면 안 됨                             | 언제든지 state를 읽을 수 있음 단, 각 렌더마다 변경되지 않는 자체적인 state의 snapshot이 있음       |

  ```
  * snapshot으로서의 state: state 변수를 설정하여도 이미 가지고 있는 state 변수는 변경되지 않고, 대신 리렌더링이 발동


  * 렌더링: React가 컴포넌트 즉, 함수를 호출한다는 뜻

    - 해당 함수에서 반환하는 JSX는 시간상 UI의 스냅샷과 같음

    - prop, 이벤트 핸들러, 로컬 변수는 모두 렌더링 당시의 state를 사용해 계산


  * React의 컴포넌트 리렌더링

    1. React가 함수를 다시 호출

    2. 함수가 새로운 JSX 스냅샷을 반환 -> 스냅샷 계산

    3. React가 함수가 반환한 스냅샷과 일치하도록 화면을 업데이트 -> DOM tree 업데이트


  * 컴포넌트 메모리로써 state

    - 함수가 반환된 후 사라지는 일반 변수와 다름, React 자체에 존재

    - React가 컴포넌트를 호출하면 특정 렌더링에 대한 state의 스냅샷을 제ㅔ공

    - 컴포넌트는 "해당 렌더링의 state 값을 사용해" 곗나된 새로운 props 세트와,
      이벤트 핸들러가 포함된 UI의 스냅샷을 JSX에 반환

    1. React에 state를 업데이트하라고 명령

    2. React가 state 값을 업데이트

    3. React는 상태 값의 스냅샷을 컴포넌트에 전달


  > state 설정 시 다음 렌더링에 대해서만 변경됨
  > state 변수의 값은 이벤트 핸들러의 코드가 비동기적이더라도 렌더링 내에서 절대 변경되지 않음
  ```

  <br />

  - 스냅샷으로서의 state 요약

    - state를 설정하면 새 렌더링을 요청

    - React는 컴포넌트 외부에, 마치 선반에 보관하듯 state를 저장

    - useState를 호출하면 React는 해당 렌더링에 대한 state의 스냅샷을 제공함

    - 변수와 이벤트 핸들러는 다시 렌더링해도 "살아남지" 않음, 모든 렌더링에는 고유한 이벤트 핸들러가 있음

    - 모든 렌더링(및 그 안의 함수)은 항상 React가 그 렌더링에 제공한 state의 스냅샷을 "보게" 됨

    - 렌더링 된 JSX에 대해 생각하는 방식과 유사하게 이벤트 핸들러에서 state를 대체할 수 있음

    - 과거에 생성된 이벤트 핸들러는 그것이 생성된 렌더링 시점의 state 값을 가짐

    > 주의: 현재 렌더링에서는 state 값이 고정되어 있음

<br />

- Hook의 규칙

  - **최상위 레벨**에서만 호출: 제어문, 함수 등

    - 컴포넌트가 렌더링 될 때마다 매번 같은 순서로 호출되어야 함

  ```js
  // 잘못된 Hooks 사용법
  function MyComponent(props) {
    const [name, setName] = useState("Mildred");

    if (name !== "") {
      useEffect(() => {
        ...
      });
    }
  }
  ```

  <br />

  - **함수 컴포넌트**에서만 Hook 호출

  > eslint-plugin-react-hooks: hooks의 규칙을 따르도록 강제함

  ```js
  const memoizedValue = useMemo(() => {
    // 연산량이 높은 작업을 수행하여 결과를 반환
    return computeExpensiveValue(의존성 변수1, 의존성 변수2);
  }, [의존성 변수1, 의존성 변수2]);
  ```

<br />

- **`Custom Hook`** 만들기

  - Custom Hook: `이름이 use로 시작`하고 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수

  - 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있음

    > 각 Custom Hook 호출에 대해서 분리된 state를 얻게 됨

    > 각 Custom Hook의 호출 또한 완전히 독립적

  - Custom Hook을 만들어야 하는 상황

    - 컴포넌트 내부에 중복된 hooks 코드

  ```js
  import React, { useState, useEffect } from "react";

  function UseStatus(props) {
    const [isOnline, setIsOnline] = useState(null);

    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }

      ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);

      return () => {
        ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
      };
    });

    if (isOnline === null) {
      return "대기 중...";
    }

    return isOnline ? "온라인" : "오프라인";
  }
  ```

  ```js
  import React, { useState, useEffect } from "react";

  function UseListItem(props) {
    const [isOnline, setIsOnline] = useState(null);

    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }

      ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);

      return () => {
        ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
      };
    });

    return (
      <li style={{ color: isOnline ? "green" : "black" }}>{props.user.name}</li>
    );
  }
  ```

  > 중복되는 로직을 Custom Hook으로 추출

  ```js
  import { useState, useEffect } from "react";

  // 온라인, 오프라인 확인
  function useUserStatus(useId) {
    const [isOnline, setIsOnline] = useState(null);

    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }

      ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);

      return () => {
        ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
      };
    });

    return isOnline;
  }
  ```

  > Custom Hook 사용하기

  ```js
  function UserStatus(props) {
    // custom hook 사용 부분
    const isOnline = useUserStatus(props.user.id);

    if (isOnline === null) {
      return "대기 중...";
    }

    return isOnline ? "온라인" : "오프라인";
  }

  function UserListItem(props) {
    // custom hook 사용 부분
    const isOnline = useUserStatus(props.user.id);

    return (
      <li style={{ color: isOnline ? "green" : "black" }}>{props.user.name}</li>
    );
  }
  ```

  <br />

  - Hook들 사이에서 데이터를 공유하는 방법

  ```js
  const userList = [
    { id: 1, name: "Pauline" },
    { id: 2, name: "RubyPauline" },
    { id: 3, name: "TheresaPauline" }
  ];

  function ChatUserSelector(props) {
    // data 공유
    const [userId, setUserId] = useState(1);
    const isUserOnline = useUserStatus(userId);

    return (
      <>
        <Circle color={isUserOnline ? "green" : "black"} />
        <select
          value={userId}
          onChange={(event) => setUserId(Number(event.target.value))}
        >
          {userList.map((user) => (
            <option key={user.id} value={user.id}>
              {user.name}
            </option>
          ))}
        </select>
      </>
    );
  }
  ```

<br />

## Handling Event

- event: 특정 사건

  - ex) 사용자가 버튼을 클릭한 사건 => 버튼 클릭 이벤트

<br />

- DOM의 Event

  ```html
  <button onclick="activate()">Activate</button>
  ```

- React의 Event

  ```js
  import React from "react";

  const ButtonComponent = () => {
    return <button onClick={activate}>Activate</button>;
  };

  export default ButtonComponent;
  ```

<br />

- **`Event Handler(Event Listener)`**

  - 어떤 사건이 발생하면, 사건을 처리하는 역할

  <br />

  - class component에서 event handle

  ```js
  // 바인드 사용
  class Toggle extends React.Component {
    constructor(props) {
      super(props);

      this.state = { isToggleOn: true };

      // callback에서 this를 사용하기 위해서는 "바인딩"을 필수적으로 해줘야 함
      this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
      this.setState((prevState) => ({
        isToggleOn: !prevState.isToggleOn
      }));
    }

    render() {
      return (
        <button onClick={this.handleClick}>
          {this.state.isToggleOn ? "켜짐" : "꺼짐"}
        </button>
      );
    }
  }
  ```

  ```js
  // 바인드 없이 사용
  class MyButton extends React.Component {
    handleClick = () => {
      // Class field syntax 사용
      console.log("this is", this);
    };

    render() {
      <!-- Arrow function 사용해 this 바운드 -->
      return <button onClick={this.handleClick}>클릭</button>;
    }
  }
  ```

  <br />

  - function component에서 event handle

  ```js
  function Toggle(props) {
    const [isToggleOn, setIsToggleOn] = useState(true);

    // 방법 1. 함수 안에 함수로 정의
    function handleClick() {
      setIsToggleOn((isToggleOn) => !isToggleOn);
    }

    // 방법 2. arrow function을 사용해 정의
    const handleClick = () => {
      setIsToggleOn((isToggleOn) => !isToggleOn);
    };

    return (
      <button onClick={handleClick}>{isToggleOn ? "켜짐" : "꺼짐"}</button>
    );
  }
  ```

<br />

- Event Handler에 Arguments 전달하기

  - **Argument**: 함수에 전달할 데이터

  <br />

  - class component

  ```js
  class MyButton extends React.Component {

    ...

    render (
      return (
        <!-- Arrow function 사용 -->
        <button onClick={(event) => this.deleteItem(id, event)}>삭제하기</button>

        <!-- bind 사용 -->
        <button onClick={this.deleteItem.bind(this, id)}>삭제하기</button>
      );
    )
  };

  export default MyButton;
  ```

  <br />

  - function component

  ```js
  import React from "react";

  const MyButton = () => {
    const handleDelete = (id, event) => {
      console.log(id, event.target);
    };

    return (
      <button onClick={(event) => handleDelete(1, event)}>삭제하기</button>
    );
  };
  ```

<br />

- 실습(event handler)

  - class component

  ```js
  import React, { Component } from "react";

  export default class ConfirmButton extends Component {
    constructor(props) {
      super(props);

      this.state = {
        isConfirmed: false
      };

      this.handleConfirm = this.handleConfirm.bind(this);
    }

    handleConfirm() {
      this.setState((prevState) => ({
        isConfirmed: !prevState.isConfirmed
      }));
    }

    render() {
      return (
        <button onClick={this.handleConfirm} disabled={this.state.isConfirmed}>
          {this.state.isConfirmed ? "확인됨" : "확인하기"}
        </button>
      );
    }
  }
  ```

    <br />

  - Class fields syntax 사용

    - bind 삭제

    - 화살표 함수 사용

  ```js
  import React, { Component } from "react";

  export default class ConfirmButton extends Component {
    constructor(props) {
      super(props);

      this.state = {
        isConfirmed: false
      };
    }

    handleConfirm = () => {
      this.setState((prevState) => ({
        isConfirmed: !prevState.isConfirmed
      }));
    };

    render() {
      return (
        <button onClick={this.handleConfirm} disabled={this.state.isConfirmed}>
          {this.state.isConfirmed ? "확인됨" : "확인하기"}
        </button>
      );
    }
  }
  ```

  <br />

  - function component로 바꾸기

  ```js
  import React, { useState } from "react";

  const ConfirmButton = (props) => {
    const [isConfirmed, setIsConfirmed] = useState(false);

    const handleConfirm = () => {
      setIsConfirmed((prevIsConfirmed) => !prevIsConfirmed);
    };

    return (
      <button onClick={handleConfirm} disabled={isConfirmed}>
        {isConfirmed ? "확인됨" : "확인하기"}
      </button>
    );
  };

  export default ConfirmButton;
  ```
