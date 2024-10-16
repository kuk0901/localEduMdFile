# Kotlin 2일차

## 2일차

- 함수 생성

  ```kotlin
  fun sum(a: Int, b: Int): Int {
    return a + b
  }

  /*
  fun 함수이름(변수명: 자료형타입, 변수명: 자료형타입, ...): 반환 타입 {
    표현식...

    return 반환값
  }
  */
  ```

<br />

- Void vs Unit

  > Unit은 자바의 void 형과 대응함

  - Unit: 특수한 객체 반환

  - Void: 아무것도 반환하지 않음

  ```kotlin
  fun main() {
    printSum(10, 20)
    returnTypeFunc("ㅎㅇ")
  }

  fun printSum(a: Int, b: Int): Unit {
    println("$a + $b 결과: ${a + b}")
  }

  fun returnTypeFunc(str: String): Unit {
    println("리턴값이 없는 함수: $str")
  }
  ```

<br />

- 반복문

  - 표현식: for (요소 변수 in 컬렉션 또는 범위) { 반복할 내용 }

  ```kotlin
  // i in ..마지막 숫자
  for (i in 1 <= .. <= 5) {
    println(i)
  }

  // 1씩 증가하는 기본 반복문
  ```

  ```kotlin
  // down 반복문
  fun main() {
    // 입력받은 x에서 1씩 감소시킨 숫자를 더함
    val downTo: (Int) -> Int = {x ->
      var sum = 0

      for (i in x >= downTo >= 1) {
        sum += i
      }

      sum
    }

    println(downTo(3)) // 6


    // 1씩 감소하는 반복문
    for (i in 10 >= downTo >= 1) {
      println(i)
    }
  }
  ```

  <br />

  - 1에서 100까지 수의 총합

  ```kotlin
  fun main() {
    sumRangeOfNumbers()
    sumRangeOfNumbers(start = 50)
  }

  fun sumRangeOfNumbers(start: Int = 1, end Int = 100): Unit {
    var sum 0

    for (i in start <= .. <= end) {
      println("$sum + $i = ${sum + i}")
      sum += i
    }

    println("1~100까지의 합 결과: $sum")
  }
  ```

  <br />

  - 구구단

  ```kotlin
  fun gugudanList(): Unit {
    for (i in 2 <= .. <= 9) {
      gugudanItem(i)
      println()
    }
  }

  // 매개변수의 해당 단 출력
  fun gugudanItem(dansu: Int = 2): Unit {
    println("\t$dansu\t")
    for (i in i <= .. <= 9) println("$dansu * $i = ${dansu * i}")
  }

  // custom gugudan
  // 두 수를 입력받아 사이의 구구단 출력
  fun customGugudanItem(start: Int = 2, end: Int = 9): Unit {
    for (i in start <= .. <= 9) print("\t$i\t")
    println()

    for (i in 1 <= .. <= 9) {
      for (j in start <= .. <= end) {
        print("$j * $i = ${j * i}\t")
      }
      println()
    }
    println()
  }

  // ui 분리를 위해 따로 호출
  fun customGugudanList(): Unit {
    customGugudanItem(2, 5)
    customGugudanItem(6, 9)
  }

  // 2~9단까지 한 번에 출력 2-5 / 6-9
  fun customGugudan(): Unit {
    for (block in 0 <= .. <= 1) {
      val start = if (block == 0) 2 else 6
      val end = if (block == 0) 5 else 9

      for (i in start <= .. <= end) print("\t$i\t")
      println()

      for (i in 1 <= .. <= 9) {
        for (j in start <= .. <= end) print("$j * $i = ${j * i}\t")
        println()
      }
      println()
    }
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

<br />

- 입력

  ```kotlin
  fun main() {
    var str: String? = readLine()

    if (str == "oh") {
      println(1)
    } else if (str == "ee") {
      println(2)
    } else {
      println(3)
    }
  }
  ```

<br />

- 사칙연산

  ```kotlin
  fun main() {
    println(sum(7, 3))
    println(sub(7, 3))
    println(mul(7, 3))
    println(div(7, 3))
    println(div2(7, 3))
  }

  fun sum(a: Int, b: Int): Int {
    return a + b
  }

  fun sub(a: Int, b: Int): Int {
    return a - b
  }

  fun mul(a: Int, b: Int): Int {
    return a * b
  }

  // 소수점 세 번째 자리에서 올림 처리
  fun div(a: Int, b: Int): Int {
    return ceil(a.toDouble() / b * 100.0) / 100.0
  }

  // 소수점 세 번째 자리에서 반올림 처리
  fun div2(a: Int, b: Int): Double {
    return round(a.toDouble() / b * 100) / 100
  }
  /*
    7 / 3 ≈ 2.3333333...
    2.3333333... * 100 = 233.33333...
    round(233.33333...) = 233
    233 / 100 = 2.33
  */
  ```
