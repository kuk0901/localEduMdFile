# Kotlin 2일차

## 2일차

- 함수 생성

  ```kotlin
  fun sum(a: Int, b: Int): Int {
    return a + b
  }

  /*
  fun 함수이름(변수명: 자료형타입, 변수명: 자료형타입, ...): 반환타입 {
    표현식...

    return 반환값
  }
  */
  ```

<br />

- Void vs Unit

  > Unit은 자바의 void형과 대응함

  - Unit: 특수한 객체 반환

  - Void: 아무것도 반환하지 않음

<br />

- 반복문

  - 표현식: for (요소 변수 in 컬렉션 또는 범위) { 반복할 내용 }

  ```kotlin
  // i in ..마지막숫자
  for (i in 1..5) {
    println(i)
  }
  ```

<br />

- 동일 패키지에서 함수는 공유됨

  - 동일 패키지에서는 모든 파일 안에서 동일 이름의 함수명 존재 불가

  ```kotlin
  // etc.funBasic1
  fun main() {
    test()
  }

  // error
  fun test() {
    println("다른 코틀린 파일에서 함수 호출")
  }

  // etc.funBasic2

  // error
  fun test() {
    println("다른 코틀린 파일에서 함수 호출")
  }

  // error 발생
  ```

  ```kotlin
  // etc.funBasic1
  fun main() {
    test() // 다른 코틀린 파일에서 함수 호출
  }

  fun test() {
    println("다른 코틀린 파일에서 함수 호출")
  }

  // etc.funBasic2
  /* fun test() {
    println("다른 코틀린 파일에서 함수 호출")
  }
  */

  // 동작
  ```

  <br />

  - 동일한 이름의 함수가 파일에 있을 때 다른 패키지의 함수를 가져와 사용하는 경우

  ```kotlin
  package com.edu.etc

  import com.edu.etx.importTest.test

  fun main() {
    test() // 다른 코틀린 파일에서 함수 호출2
  }

  fun test() {
    println("다른 코틀린 파일에서 함수 호출1")
  }
  ```

  ```kotlin
  package com.edu.etc.importTest

  fun test() {
    println("다른 코틀린 파일에서 함수 호출2")
  }
  ```
