# Kotlin 3일차

## 3일차

- 함수형 프로그래밍

  - 코틀린: 함수형 프로그래밍(FP: Functional Programming)과 객체 지향 프로그래밍(OOP, Object-Oriented Programming)을 모두 지원하는 다중 패러다임 언어

  - 두 기법은 대규모 프로그래밍의 설계에도 적합하여 많은 현대 프로그래밍 언어가 지향하는 특징

  - 함수형 프로그래밍은 코드가 간략화되고 테스트나 재사용성이 더 좋아지면서 개발 생산성이 늘어나는 장점 덕분에 필수로 알고 있어야 함

  - 함수형 프로그래밍: 순수 함수를 작성해 프로그램의 부작용을 줄이는 프로그래밍 기법

    - 람다식과 고차 함수를 사용함

<br />

- 순수 함수

  - 어떤 함수가 같은 인자에 대하여 항상 같은 결과를 반환하면 "부작용이 없는 함수"라고 말함

  - 부작용이 없는 함수가 함수 외부의 어떤 상태도 바꾸지 않는다면 순수 함수(Pure Function)라고 부름

  - 순수 함수의 특성은 스레드에 사용해도 안전하고 코드를 테스트하기도 쉽다는 장점이 있음

<br />

- 순수 함수 조건

  1. 같은 인자에 대해 항상 같은 값을 반환함

  2. 함수 외부의 어떤 상태로 바꾸지 않음

<br />

- 람다식(Lambda Expressions)

  - {x, y -> x + y} // 람다식의 예(이름이 없는 함수 형태)

  - 함수의 이름이 없고 화살표가 사용됨

  - 수학에서 말하는 람다 대수: 이름이 없는 함수로 2개 이상의 입력을 1개의 출력으로 단순화한다는 개념

  - 함수형 프로그래밍의 람다식: 다른 함수의 인자로 넘기는 함수, 함수의 결과 값으로 반환하는 함수, 변수에 저장하는 함수

  ```kotlin
  fun main() {
    var result: Int
    val multi1 = {x: Int, y: Int -> x * y}

    val multi2: (Int, Int) -> Int = {x: Int, y: Int -> x * y}
    // 변수를 함수처럼 사용: 람다식의 선언 자료형은 람다식 매개변수에 자료형이 명시된 경우 생략 가능
    // = 선언 자료형이 명시되어 있으면 생략 가능
    // -> 함수의 내용의 결과 반환, 표현식이 여러 줄인 경우 마지막 표현식이 반환
    // => (Int, Int)

    val multi3: (Int, Int) -> Int = {x: Int, y: Int ->
      println("x * y")
      var result = x * y
      x * y // 마지막 표현식이 리턴
    }

    // 선언 자료형 생략
    // 람다식 매개변수에 자료형이 명시된 경우 생략 가능
    // 함수의 결과가 예측될 경우 결과의 자료형도 생략 가능 -> int + int의 결과는 int
    // 선언 자료형이 생략되는 경우 콜론없이 할당 연산자를 바로 작성
    val multi4 = {x: Int, y: Int -> x * y}

    // 람다식 매개변수 자료형의 생략
    val multi5: (Int, Int) -> Int = {x, y -> x * y}

    // 잘못된 에시: 추론 불가능
    // val multi6: {x, y -> x * y}

    result = multi(10, 20)
    println(result)
  }

  // fun highFunc(sum: (Int, Int) -> Int, a: Int, b: Int): Int
  // 고차함수형     람다식 매개변수             정수형 매개변수     반환타입
  // { return sum(a, b) }
  ```

<br />

- 일급 객체

  - 함수형 프로그래밍에서는 함수를 일급 객체로 생각

  - 람다식: 일급 객체의 특징을 가지고 있음

  - 특징

    1. 일급 객체는 함수의 인자로 전달할 수 있음

    2. 함수의 반환 값에 사용 가능

    3. 변수에 담을 수 있음

  - 만약 함수가 일급 객체면 일급 함수라 부름, 일급 함수에 이름이 없는 경우 "람다식 함수" 또는 "람다식"이라고 부를 수 있음

  > 람다식은 일급 객체의 특징을 가진 이름 없는 함수

<br />

- 고차 함수(High-order Function)

  - 다른 함수를 인자로 사용하거나 함수를 결과 값으로 반환하는 함수

  - 두 특징을 모두 갖고 있어도 고차 함수라고 말함

  - 일급 객체 혹은 일급 함수를 서로 주고받을 수 있는 함수

  ```kotlin
  fun main() {
    println(highFunc({x, y} -> x + y), 10, 20)
  }

  fun highFunc(sum: (Int, Int) -> Int, a: Int, b: Int): Int = sum(a, b)
  // 고차함수: fun highFunc
  // 람다식 매개변수: sum
  // 자료형이 람다식으로 선언되어 (x, y -> x + y) 형태로 인자를 받는 것이 가능: (Int, Int) -> Int
  // 정수형 매개변수: a: Int, b: Int
  // 반환 자료형: Int
  ```

  > highFunc 함수는 람다식의 표현문에 따라 결국 a + b의 정수값 결과를 반환 sum(a, b)

  ```kotlin
  // 일반 함수를 인자나 반환값으로 사용하는 고차함수
  fun main() {
    val res1 = sum(3, 2)
    val res2 = mul(sum(3, 3), 3)

    println("res1: $res1, res2: $res2")
  }

  fun sum(a: Int, b: Int) = a + b
  fun mul(a: Int, b: Int) = a * b
  ```

  ```kotlin
  fun main() {
    val res1 = sum(30, 20)
    val res2 = sum(30, 30)
    res2 = mul(res2, 3)

    println("res1: $res1, res2: $res2")
  }

  fun sum2(a: Int, b: Int) = a + b
  fun mul2(a: Int, b: Int) = a * b
  ```

  <br />

- 요약

  - 함수형 프로그래밍의 정의와 특징

    1. 순수 함수를 사용해야 함

    2. 람다식을 사용할 수 있음

    3. 고차 함수를 사용할 수 있음
