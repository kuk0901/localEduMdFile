# Kotlin 4일차

## 4일차

- 문제 풀이

  - 약수 구하기

  ```kotlin
  fun main() {
    val x = 10
    var arr = ArrayList<Int>()

    val arrValidationFnc = {num: Int -> 10 % i == 0}
    val addStrFnc = {num: Int -> if (num != 1) ", $num" else "$num"}

    // 단순 문자열 출력
    for (i in 1 <= .. <= x) {
      if (arrGetFnc(i)) print(addStrFnc(i))
    }

    // List 사용
    for (i in 1 <= .. <= x) {
      if (arrGetFnc(i)) arr.add(i)
    }

    println("$arr \n")

    // 전체 제어문 사용
    for (i in 1 <= .. <= x) {
      if (arrGetFnc(i)) {
        if (i != 1) print(", $i")
        else print("$i")
      }
    }
  }
  ```

  <br />

  - 도형 그리기

  ```kotlin
  fun main() {
    shapeFnc1()
    println()
    shapeFnc2()
  }

  fun shapeFnc1(num: Int = 4) {
    for (line in 1 <= .. <= num) {
      for (i in num - line >= downTo >= 1) print(" ")
      for (j in 1 <= ..< 2 * line) print("*")

      println()
    }
  }

  fun shapeFnc2(num: Int = 4) {
    // 도형 위
    for (line in 1 <= .. <= num) {
      for (i in num - line >= downTo >= 1) print(" ")
      for (j in 1 <= ..< 2 * line) print("*")

      println()
    }

    // 도형 아래
    for (line in 1 <= .. <= num) {
      for (i in 1 <= .. <= line) print(" ")
      for (j in 1 <= ..< 2 * (num - line)) print("*")

      println()
    }
  }
  ```

  <br />

  - 최소 공배수

  ```kotlin
  fun main() {
    val ggd = {x: Int, y: Int ->
      var r: Int
      var a: Int = x
      var b: Int = y

      while(b != 0) {
        r = a % b
        a = b
        b = r
      }

      a
    }

    val gcm = {a: Int, b: Int -> a * b / gcd(a, b)}

    println(gcm(5, 10))
    println(gcm(13, 25))
  }
  ```

  <br />

  - 문자열을 출력하는 람다식

  ```kotlin
  fun main() {
    // 두 문자열을 연결
    // 람다식, 문자열, 문자열
    strUtil({str1: String, str2: String -> println("$str1, ${str2}!")}, "하이하이", "방가방가")

    // 두 숫자 중 더 큰 수 찾기
    strUtil({str1: String, str2: String ->
      if (str1.toInt() > str2.toInt()) println("$str1 vs ${str2} ${str1}가 크다")
      else if (str1.toInt() < str2.toInt()) println("$str1 vs ${str2} ${str2}가 크다")
      else println("메롱")
    }, "10", "20")

    strUtil({str1: String, str2: String ->
      if (str1.toInt() > str2.toInt()) println("$str1 vs ${str2} ${str1}가 크다")
      else if (str1.toInt() < str2.toInt()) println("$str1 vs ${str2} ${str2}가 크다")
      else println("메롱")
    }, "2000", "100")
  }

  fun strUtil(strFnc: (String, String) -> Unit, str1: String, str2: String): Unit {
    strFnc(str1, str2)
  }
  ```

<br />

- 배열

  - arrayOf

  - arrayOf\<T>도 가능하지만 infArrayOf도 사용 가능

  > stringArrayOf는 X

  ```kotlin
  import kotlin.random.Random

  val numArr = arrayOf(1, 2, 3, 4)

  val strArr = arrayOf("cat", "dog", "lion")

  println(numArr.get(0))
  println(numArr.get(1))
  println(numArr.get(2))
  println(numArr.get(3))

  for (obj in strArr) {
    println(obj)
  }

  val strArr2 = arrayOf<Int>(1, 12)
  val strArr3 = intArrayOf<Int>(1, 12)
  // val strArr4 = stringArrayOf() -> arrayOf()

  val randomNumber = Random.nextInt()
  println(randomNumber)
  ```

- 문제 풀이

  - 랜덤 함수를 사용해 배열의 값과 비교 후 같을 때까지 반복해서 조회 후 조회가 끝난 회수 출력

  ```kotlin
  fun main() {
    val numbers = arrayOf(1, 3, 9, 4, 7)
    var count = 0
    var randomNumber: Int

    while(true) {
      randomNumber = Random.nextInt(1, 10)
      println("랜덤 숫자 확인: $randomNumber")

      for (number in numbers) {
        count++

        if (number == randomNumber) {
          println("count: ${count}번만에 찾았다")
          return
        }
      }

      println("$randomNumber 없다")
    }
  }
  ```
