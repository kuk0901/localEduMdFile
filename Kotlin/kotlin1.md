# Kotlin 1일차

## 1일차

- 코틀린(kotlin) 언어 특징

  - 자료형에 대한 오류를 미리 잡을 수 있는 정적 언어

  - 널 포인터로 인한 프로그램의 중단을 예방할 수 있음

  - 객체지향 프로그래밍과 함수형 프로그래밍 모두 가능

  - 다양한 플랫폼에서 작동하도록 만들어짐

  - 타입 추론: 자료형은 지정하지 않고 변수를 선언할 경우 변수에 할당된 값을 보고 알아서 자료형을 지정

  <br />

  - 파일명 소문자: 함수 기반(함수형 프로그래밍) => 함수형 파일명

  - 파일명 소문자: 클래스 기반(객체지향 프로그래밍) => 클래스 파일명

  <br />

- 자료형 작성

  - 자료형을 작성할 때는 절대적으로 모두 대문자로 작성

  - 기본형, 참조형 모두 동일

  ```kotlin
  fun main() {
    // val = immutable -> 불변 === 상수, 다른 값으로 변경 불가
    val intro: String = "안녕"
    val num: Int = 20

    println("intro: " + intro) // 사용 X
    println("num : $num") // 사용 O

    // var = mutable -> 가변 === 변수, 다른 값으로 변경 가능
    var name: String = "?"
    name = "코틀린"

    pringln("name: $name") // 변수 하나
  }
  ```

<br />

- 변수 선언

  - 선언 키워드 변수명: 자료형 = 값 -> val username: String = "길동"

  ```kotlin
  fun main() {
    val number = 10
    var lang = "Korean"
    val secondNum: Int = 20
    lang = "English"
  }
  ```

<br />

- 문자열 연산

  ```kotlin
  fun main() {
    var str1: String = "Hello"
    var str2 = "World"
    var str3 = "Hello"

    println("str1 === str2: ${str1 === str2}") // 블럭 사용
    println("str1 === str3: ${str1 === str3}") // 블럭 사용
  }
  ```

<br />

- 형식화된 다중 문자열

  ```kotlin
  fun main() {
    val formattedStr = """
      var a = 5
      신기하다
      백틱과 유사
    """.trimIndent()

    println(formattedStr)
  }
  ```

<br />

- null

  - 일반적 변수 선언 시에는 null값 할당 불가

  - null을 허용하려면 자료형 뒤에 물음표(?) 기호를 명시해야 함

  ```kotlin
  fun main() {
    var str1: String = "Hello"
    str1 = null // error

    println("str1: $str1")

    // null을 허용하려면 자료형 뒤에 물음표(?) 기호를 명시
    var str2: String? = "Hello"
    str2 = null

    println("str2: $str2")
  }
  ```

  > 타입 추론 시 null이 아닌 데이터 타입에 null 값 허용 불가

<br />

- safe call(안전 호출) 연산자, non-null 단정 기호

  - safe call

    - Null 안전성을 제공

    - 객체가 null이 아닐 때만 메서드나 프로퍼티에 접근

    - 객체가 null이면 전체 표현식이 null을 반환

  - non-null assertion

    - nullable 타입을 non-null 타입으로 강제 변환

    - 변수가 null이 아닐 경우: non-null 값 반환

    - 변수가 null일 경우: NullPointerException 발생

  ```kotlin
  fun main() {
    var str1: String? = "hhhhh"

    str1 = null

    println("str1: $str length: ${str1.length}")

    // safe-call
    println("str1: $str length: ${str1?.length}")

    // assert exp
    println("str1: $str length: ${str1!!.length}")
  }
  ```

<br />

- etc

  - readLine(): 한 줄 입력(null 허용)

  - readln(): 한 줄 입력(null 허용 X)

  ```kotlin
  fun main() {
    var str: String? = readLine()

    if (str == "oh") println(1)
    else if (str == "eee") println(2)
    else println(3)
  }
  ```

<br />

- safe call(안전 호출) 연산자는 기본적으로 엘비스 연산자와 함게 사용됨

  - ${str1?.length ?: -1} -> null이 아닌 경우 str1?.length, null인 경우 -1

  - null이 아닌 경우 ?: null인 경우

  ```kotlin
  var str1: String? = "hello world"

  printlln("str1: $str1 length: ${str1?.length ?: -1}")
  ```

<br />

- Casting

  - 데이터 타입에 물음표가 붙는 경우 기본형 데이터 타입이여도 참조형으로 바뀜

  ```kotlin
  val a: Int = 128
  val b = a

  printlnl(a === b) // true

  val c: Int? = a
  val d: Int? = a
  val d: Int? = c

  printlnl(c == d) // true
  printlnl(c === b) // false
  printlnl(c === e) // true
  ```

<br />

- 숫자에서 저장되는 값이 128보다 작으면 그 값은 캐시에 저장됨

  - Byte -128 ~ 127

  ```kotlin
  val a: Int = 127
  val b = a

  printlnl(a === b) // true

  val c: Int? = a
  val d: Int? = a
  val d: Int? = c

  printlnl(c == d) // true
  printlnl(c === b) // true
  printlnl(c === e) // true
  ```

  > 무조건 객체화됨
