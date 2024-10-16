# Kotlin 5일차

## 5일차

- Array

  ```kotlin
  import java.util.*

  fun main() {
    var numArr = arrayOf(1, 2, 3, 4)

    val strArr = arrayOf("cat", "dog", "lion", 1, 3.233)

    // 변수키워드 변수명 = Array(요소 크기, 초기값)
    val numArr2 = Array(5, { i -> i})

    println("numAr2: ${Arrays.toString(numArr2)}")

    var cnt = 0
    for (i in numArr2) {
      cnt++
      println("$cnt 번째: $i")
    }

    numArr2.forEach { item -> println(item) }

    strArr.forEachIndexed { i, item -> println("strArr[$i]: $item") }

    val testArr = Array(10) { i -> i * 2 }

    testArr.filter { e -> e > 10 }
      .forEach { e -> print("$e\t") }
  }
  ```

<br />

- 문제 풀이

  - Q1

  ```
  - 키보드로부터 정수를 입력받아 입력받은 수까지의 피보나치 수열을 출력하는 프로그램

  ex) 입력: 5 -> 출력: 0 1 1 2 3
  ex) 입력: 10 -> 출력: 0 1 1 2 3 5 8 13 21 34
  ```

  ```kotlin
  fun main() {
    val input: Int = readln().toInt()

    fibonacciNumbersFnc(input)
  }

  fun fibonacciNumbersFnc(num: Int): Unit {
    val fibonacciNumbers = Array(num) { 0 }
    fibonacciNumbers[1] = 1

    for (i in 2 <= ..<num) {
      fibonacciNumbers[i] = fibonacciNumbers[i - 1] + fibonacciNumbers[i - 2]
    }

    fibonacciNumbers.forEach { item -> print("$item\t") }
  }
  ```

  <br />

  - Q2

  ```
  - 배열에 1~45 숫자가 6개 들어있고 이 숫자들은 랜덤
  - 사용자가 키보드 또는 코드로 숫자를 넣음(6, 13, 23)
  - 서로 대조해서 모두 맞을 경우, 2개나 1개가 맞을 경우 맞지 않은 경우에 맞춰 출력

  ex) 3개
  1등 당첨금: 1,000,000
  당첨 숫자: [2, 6, 14, 25, 40, 33]
  님이 고른 숫자: [6, 13, 33]
  맞춘 숫자: [2, 6, 33]

  ex) 2개
  2등 당첨금: 500,000
  당첨 숫자: [2, 6, 14, 25, 40, 33]
  님이 고른 숫자: [6, 13, 33]
  맞춘 숫자: [6, 33]

  ex) 3개
  2등 당첨금: 100,000
  당첨 숫자: [2, 6, 14, 25, 40, 33]
  님이 고른 숫자: [6, 13, 33]
  맞춘 숫자: [6]

  ex) 0개
  맞은 숫자가 존재하지 않습니다.
  당첨 숫자: [2, 6, 14, 25, 40, 33]
  님이 고른 숫자: [0, 7, 43]
  맞춘 숫자: 없음
  ```

  ```kotlin
  fun main() {
    lottoFnc()
  }

  // 랜덤 숫자 배열 지정
  fun randomNumArr(): Array<Int> {
      val arr = Array(6) { 0 }

      for (i in 0 ..<6) {
          val randomNum = (1..45).random()
          arr[i] = randomNum
      }

      return arr
  }

  // 로또 주요 시스템
  fun lottoDrawingRule(systemArr: Array<Int>, userNumArr: Array<Int>): Unit {
      // compare array -> 매칭
      val matchNumArr: Array<Int> = systemArr.filter { userNumArr.contains(it) }.toTypedArray()

      when (matchNumArr.size) {
          3 -> println("1등 당첨금: 1,000,000")
          2 -> println("2등 당첨금: 500,000")
          1 -> println("3등 당첨금: 100,000")
          else -> println("맞은 숫자가 존재하지 않습니다.")
      }

      println("당첨 숫자: ${systemArr.contentToString()}")
      println("님이 고른 숫자: ${userNumArr.contentToString()}")

      if (matchNumArr.isEmpty()) {
          println("맞춘 숫자: 없음")
          return
      }

      println("맞춘 숫자: ${matchNumArr.contentToString()}")
  }

  // 로또 서비스
  fun lottoFnc() {
      val systemNumArr: Array<Int> = randomNumArr()
  //    println(systemNumArr.contentToString())

      println("숫자 3개를 형식에 맞게 입력해 주세요. (ex: 1, 2, 3)")
      val userNumArr: Array<Int> = readln().split(", ").map { it.toInt() }.toTypedArray()

      lottoDrawingRule(systemNumArr, userNumArr)
  }
  ```

  <br />

  - Q3

  ```
  - 크기가 3인 배열에 강아지, 고양이, 앵무새를 넣음 (이들의 배열 위치는 무작위로 변경됨)

  사용자는
  배열에 첫번째 값이 강아지 또는 고양이 또는 앵무새 중에 무엇일지 맞춘다
  두번째 위치의 값도 강아지 또는 고양이 또는 앵무새 중에 무엇일지 맞춘다
  세번째 위치의 값도 강아지 또는 고양이 또는 앵무새 중에 무엇일지 맞춘다

  ex: 정답 배열값이 고양이, 앵무새, 강아지인 경우
  사용자가 강아지, 앵무새, 고양이 를 넣었다면
  출력 값은 [false, true, false]로 출력해서
  사용자에게 무엇이 맞고 틀렸는지 알려줌

  계속 시도해서 전부 true, true, true인 경우

  몇번 시도했는지 횟수를 출력하고 프로그램이 종료됨
  ```

  ```kotlin
  package com.edu.final

  fun main() {
    guessAnimalsService()
  }

  fun guessAnimalsService(): Unit {
      var animals = arrayOf("강아지", "고양이", "앵무새")
      animals = shuffleOfStrArray(animals)

      println("animals: ${animals.contentToString()}")

      var userAnswersArr: Array<String>
      var cnt: Int = 0
      var input: String
      var resultArr: Array<Boolean>

      while (true) {
          println("강아지, 고양이, 앵무새가 한 마리씩 무작위 순서로 철창에 갇혀있습니다.\n갇혀있는 순서를 맞추세요.")
          println("ex: 강아지, 고양이, 앵무새")

          input = readln() // 입력
          cnt++ // 카운트 증가

          userAnswersArr = input.split(", ").map { it.trim() }.toTypedArray()
          resultArr = wordCheckFnc(animals, userAnswersArr)

          if (!resultArr.contains(false)) {
              println("정답입니다.\n시도한 횟수: ${cnt}번")
              return
          }

          println("시도 결과: ${resultArr.contentToString()}")
      }
  }

  // 배열 위치를 랜덤으로 섞음
  fun shuffleOfStrArray(strArr: Array<String>): Array<String> {
      val arr: Array<String> = strArr
      var tempStr: String

      for (i in 0..<arr.size - 1) {
          val randomIndex = (i ..< arr.size).random()
          tempStr = arr[i]
          arr[i] = arr[randomIndex]
          arr[randomIndex] = tempStr
      }

      return arr
  }

  // 입력 단어와 정답 비교 함수
  fun wordCheckFnc(a: Array<String>, b: Array<String>): Array<Boolean> {
      val result = Array<Boolean>(3) { false }

      for (i in 0..<a.size) {
          result[i] = a[i] == b[i]
      }

      return result
  }
  ```
