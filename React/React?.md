# React 공식 문서

## React

- React 앱: 컴포넌트로 구성

- 컴포넌트: 고유한 로직과 모양을 가진 UI(사용자 인터페이스)의 일부로 버튼만큼 작거나 전체 페이지만큼 클 수 있음

- React 컴포넌트: 마크업을 반환하는 자바스크립트 함수

<br />

## 마크업 문법?

```js
function MyButton() {
  return <button>I'm a button</button>;
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

- 위처럼 작성된 JavaScript + XML/HTML

- 대부분의 React 프로젝트는 편의성을 위해 JSX를 사용하고, 로컬 개발에 권장하는 모든 도구는 JSX를 기본적으로 지원

- JSX는 HTML보다 엄격함 -> 태그를 열었다면 꼭 닫아야 함 또한, 여러 개의 JSX 태그 반환 불가(공유되는 부모로 감싸줘야 함)

- className으로 클래스 지정

<Br />

## 데이터 표시

- 중괄호를 사용하면 코드에서 일부 변수를 삽입하여 사용자에게 표시할 수 있도록 자바스크립트로 “이스케이프 백” 할 수 있음

  ```js
  return <h1>{user.name}</h1>;
  ```

- JSX 어트리뷰트에서 따옴표 대신 중괄호를 사용하여 “자바스크립트로 이스케이프” 할 수도 있음

  ```js
  return <img className="avatar" src={user.imageUrl} />;
  ```

<br />

- JSX 문법에서의 style={{}}은 특별한 문법이 아니라 style={ } JSX 중괄호 안에 있는 일반 {} 객체임

  - 스타일이 자바스크립트 변수에 의존하는 경우 style 어트리뷰트를 사용할 수 있음

<br />

## 조건부 렌더링

- 자바스크립트 코드와 동일 방법으로 조건문 작성 가능

  ```js
  let content;
  if (isLoggedIn) {
    content = <AdminPanel />;
  } else {
    content = <LoginForm />;
  }
  return <div>{content}</div>;
  ```

<br />

- 조건부 삼항 연산자를 사용해 간결하 코드로 작성 가능

  - 단, if문과는 달리 JSX 내부에서 작동함

  ```js
  <div>{isLoggedIn ? <AdminPanel /> : <LoginForm />}</div>
  ```

  - else 분기가 필요하지 않으면 더 짧은 && 연산자 사용 가능

  ```js
  <div>{isLoggedIn && <AdminPanel />}</div>
  ```

<br />

## 리스트 렌더링하기

- 컴포넌트 리스트를 렌더링하기 위해서는 for 문 및 map() 함수와 같은 자바스크립트 기능을 사용

  - li에 key 어트리뷰트가 있는 것을 주목

  - 목록의 각 항목에 대해, 형제 항목 사이에서 해당 항목을 고유하게 식별하는 문자열 또는 숫자를 전달해야 함

  - React는 나중에 항목을 삽입, 삭제 또는 재정렬할 때 어떤 일이 일어났는지 알기 위해 key를 사용함

  ```js
  const products = [
    { title: "Cabbage", id: 1 },
    { title: "Garlic", id: 2 },
    { title: "Apple", id: 3 }
  ];

  const listItems = products.map((product) => (
    <li key={product.id}>{product.title}</li>
  ));

  return <ul>{listItems}</ul>;
  ```

<br />

## 이벤트에 응답하기

- 컴포넌트 내부에 _이벤트 핸들러_ 함수를 선언해 이벤트에 응답 가능

  - onClick={handleClick}의 끝에 소괄호가 없는 것을 주목

  - 이벤트 핸들러 함수를 호출하지 않고 전달만 하면 됨

  - React는 사용자가 버튼을 클릭할 때 이벤트 핸들러를 호출

  ```js
  function MyButton() {
    function handleClick() {
      alert("You clicked me!");
    }

    return <button onClick={handleClick}>Click me</button>;
  }
  ```

<br />

## 화면 업데이트하기

- 컴포넌트가 특정 정보를 “기억”하여 표시하기를 원하는 경우, 컴포넌트에 state 추가

  - useState로부터 현재 state (count)와 이를 업데이트할 수 있는 함수(setCount)를 얻을 수 있음

  - 어떤 이름으로도 지정할 수 있지만 [something, setSomething]으로 작성하는 것이 일반적

  <br />

  1. React에서 useState를 가져옴

  ```js
  import { useState } from "react";
  ```

  <br />

  2. 컴포넌트 내부에 state 변수 선언

  ```js
  function MyButton() {
    const [count, setCount] = useState(0);
    // ...
  }
  ```

  <br />

  3. useState 사용

  ```js
  function MyButton() {
    const [count, setCount] = useState(0); // 0으로 초기화, count === 0

    function handleClick() {
      setCount(count + 1); // state 변경을 위해 setCount() 실행하고 새 값을 전달
    }

    return <button onClick={handleClick}>Clicked {count} times</button>;
  }
  ```

  <br />

  4. React가 컴포넌트 함수를 다시 호출

  <br />

  - 같은 컴포넌트를 여러 번 렌더링하면 각각의 컴포넌트는 고유한 state를 얻게 됨

  ```js
  import { useState } from "react";

  export default function MyApp() {
    return (
      <div>
        <h1>Counters that update separately</h1>
        <MyButton />
        <MyButton />
      </div>
    );
  }

  function MyButton() {
    const [count, setCount] = useState(0);

    function handleClick() {
      setCount(count + 1);
    }

    return <button onClick={handleClick}>Clicked {count} times</button>;
  }
  ```

  > 각 버튼이 고유한 count state를 “기억”하고 다른 버튼에 영향을 주지 않는 방식에 주목

<br />

## Hooks 사용하기

- Hooks: use로 시작하는 함수

- React에서 제공하는 내장 Hook과 사용자가 직접 만든 custom Hook이 있음

- Hooks는 다른 함수보다 더 제한적

- 컴포넌트(또는 다른 Hooks)의 상단에서만 Hooks를 호출할 수 있음

- 조건이나 반복에서 useState를 사용하고 싶다면 새 컴포넌트를 추출하여 그곳에 넣으면 됨

<br />

## 컴포넌트 간에 데이터 공유

- 각각의 MyButton에 독립적인 count가 있었고, 각 버튼을 클릭하면 클릭한 버튼의 count만 변경

- 데이터를 공유하고 항상 함께 업데이트하기 위한 컴포넌트가 필요한 경우가 많음

- 두 MyButton 컴포넌트가 동일한 count를 표시하고 함께 업데이트하려면, state를 개별 버튼에서 모든 버튼이 포함된 가장 가까운 컴포넌트로 “위쪽”으로 이동해야 함

  <br />

  1. MyButton에서 MyApp으로 state를 위로 이동

     - 공유된 클릭 핸들러와 함께 MyApp에서 각 MyButton으로 state를 전달

  ```js
  export default function MyApp() {
    const [count, setCount] = useState(0);

    function handleClick() {
      setCount(count + 1);
    }

    return (
      <div>
        <h1>Counters that update together</h1>
        <MyButton count={count} onClick={handleClick} />
        <MyButton count={count} onClick={handleClick} />
      </div>
    );
  }
  ```

  > 전달한 정보를 props라고 함 <br />
  > count state와 handleClick 이벤트 핸들러를 포함하며, 이 두 가지를 각 버튼에 props로 전달

  <br />

  2. 부모 컴포넌트에서 전달한 props를 읽도록 MyButton을 변경

  ```js
  function MyButton({ count, onClick }) {
    return <button onClick={onClick}>Clicked {count} times</button>;
  }
  ```

  <br />

  3. 버튼을 클릭하면 onClick 핸들러가 실행

     - 각 버튼의 onClick prop는 MyApp 내부의 handleClick 함수로 설정되었으므로 그 안에 있는 코드가 실행

     - setCount(count + 1)를 실행하여 count state 변수를 증가시킴

     - 새로운 count 값은 각 버튼에 prop로 전달되므로 모든 버튼에는 새로운 값이 표시

     > “state 올리기” => state를 위로 이동함으로써 컴포넌트 간에 state를 공유
