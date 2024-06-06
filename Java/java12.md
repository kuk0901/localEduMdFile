# 12이일차

## java 12일차

- **Wrapper class**

  - 기본형 값을 감싸고 있는 클래스라는 뜻에서 붙여진 이름으로 기본형을 클래스로 표현한 것

  - Boolean, Byte와 같이 기본형 타입의 이름 첫 글자가 대문자인 것이 **래퍼 클래스**

<br />

- 기본형과 문자열 간의 변환 방법

  - 기본형 -> 문자열

    - 문자열.valueOf(기본 타입)

    - String.valueOf(기본 타입);

  ```java
  String.valueOf(300);
  ```

  <br />

  - 문자열 -> 기본형

    - 기본타입클래스.parse타입(문자열)

    - String.valueOf(기본 타입);

  ```java
  Integer.parseInt("300");
  ```

  > 프로그래밍에서 반드시 알고 있어야 하는 아주 중요한 내용

  ```
  - 오토박싱(autoboxing): 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것
  - 언박싱(unboxing): 래퍼 클래스의 객체를 기본형 값으로 변환하는 것


  ex)
  - 언박싱: Integer 객체를 int 기본형으로 자동 변환하는 과정.
  - 오토박싱: int 기본형 값을 Integer 객체로 자동 변환하는 과정.


  => 자동 변환 기능은 자바 컴파일러가 코드를 컴파일할 때 자동으로 처리
  => 기본형과 객체형을 더욱 쉽게 사용 가능
  ```

  <br />

  - 예시

  ```java
  int iVal = 300;
  String numStr = String.valueOf(iVal) + 234;

  double dVal = 200.0;
  String douStr = String.valueOf(dVal);

  long lVal = 300L;
  String longNumStr = lVal + "";

  // 객체 타입을 기본형 타입으로 변경해줌
  double sum = Integer.parseInt(numStr) + Double.parseDouble(douStr);

  System.out.println(longNumStr);

  System.out.println(numStr + " + " + douStr + " = " + sum);
  ```

<br />

- **예외 처리(Exception Handling)**

  - 프로그램 실행 중 어떤 원인에 의해 오작동하거나, 비정상적으로 종료되는 경우 해당 결과를 초래하는 원인을 프로그램 에러 || 오류라고 함

  - 발생 시점에 따라 컴파일 에러(compile-time error), 런타임 에러(runtime error)로 나눔

  <br />

  - 컴파일 에러(compile-time error)

    - 컴파일할 때 발생하는 에러

    - 코드 작성 중 오류가 난 경우

    - 컴파일을 성공적으로 마쳐야 클래스 파일이 생성되고, 생성된 클래스 파일을 실행할 수 있게 됨

  <br />

  - 런타임 에러(runtime error)

    - 프로그램의 실행 도중 발생하는 에러

  <br />

  - 논리적 에러(logical error)

    - 컴파일 및 실행은 되지만, <u>의도한 것과 다르게 동작하는 상황</u>

  ```
  - 실행 도중에 발생할 수 있는 잠재적은 오류까지 검사할 수 없기 때문에 컴파일은 잘 되었어도
  실행 중에 에러에 의해서 잘못된 결과를 얻거나 프로그램이 비정상적으로 종료될 수 있음

    - ex) 프로그램이 갑자기 실행을 멈추고 종료되는 경우 등이 이러한 경우

  => 자바에서는 실행 시 발생할 수 있는 프로그램 오류를 에러와 오류 두 가지로 구분
  ```

  <br />

  - 에러(error)

    - 메모리 부족, 스택 오버플로우와 같이 일단 **발생하면 복구할 수 없는** 심각한 오류

  <br />

  - 예외(Exception)

    - **발생하더라도 수습될 수 있는** 비교적 덜 심각한 것

  ```
  에러가 발생하면 프로그램의 비정상적인 종료를 막을 길이 없지만,
  예외는 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료 방지 가능
  ```

  <br />

  - RuntimeException 클래스

    - 주로 프로그래머의 실수에 의해서 발생할 수 있는 예외들

    - ex) 배열의 범위를 벗어남

    - 프로그래머 실수

  <br />

  - Exception 클래스

    - 주로 외부의 영향으로 발생할 수 있는 것들로 프로그램 사용자들의 동작에 의해서 발생하는 경우가 많음

    - ex) 존재하지 않는 파일의 이름 입력, 키보드로 숫자만 입력해야 할 때 문자 입력

    - 사용자 실수와 같은 외적 요인

  <br />

  - 예외 처리 하기

    - try-catch문

    - 에러는 어쩔 수 없지만, 예외는 프로그래머가 이에 대한 <u>처리를 미리 해주어야 함</u>

    - 예외 처리 O => 프로그램이 정상적인 실행 상태 유지

  <br />

  - **예외 처리**

    - 정의: 프로그램 실행 시 발생할 수 있는 **예외 발생에 대비한 코드를 작성**하는 것

    - 목적: 프로그램의 비정상 종료를 막고, 정상적인 실행 상태를 유지하는 것

    - 표현식

    ```java
    try() {
      // 예외가 발생할 가능성이 있는 코드 작성
    } catch(/* 예외 가능 클래스 변수명1 */) {
      // 예외가 발생했을 경우, 이를 처리하기 위한 코드 작성
    } catch(/* 예외 가능 클래스 변수명1 */) {
      // 예외가 발생했을 경우, 이를 처리하기 위한 코드 작성
    } finally {
      // 예외의 발생 여부에 관계없이 항상 수행되어야 하는 코드 작성
      // 무조건 수행되어야 하는 코드
      // 1. 메모리 누수를 막아야 하는 코드
      // 2. 개발자 코드 정리
      // 3. 업무적으로 무조건 처리하는 코드
      // 4. ...
    }
    ```

    > 필요한 만큼 catch문 선언

<br />

- 예외처리 실습

  - ExceptionEx1

  ```java
  public static void main(String[] args) {
    int n = 100;
    int result = 0;

    for (int i = 0; i < 10; i++) {
      result = n / (int)(Math.random() * 10);
      System.out.println(result);
    }
  }
  ```

  > runtime error가 발생하는 코드 => 예외 처리

  ```java
  public static void main(String[] args) {
    int n = 100;
    int result = 0;

    for (int i = 0; i < 10; i++) {
      try {

        result = n / (int)(Math.random() * 10);
        System.out.println(result);

      } catch (ArithmeticException ae) {
        System.out.println(0);
        System.out.println("예외 처리 성공");
      }
    }
  }
  ```

<br />

- try-catch문 흐름

  - ExceptionEx2 : 예외가 발생하지 않는 경우

  ```java
  System.out.println(1);
  System.out.println(2);

  try {
    System.out.println(3);
    System.out.println(4);
  } catch(Exception e) {
    System.out.println(5);
  }

  System.out.println(6);
  ```

  - 결과

  ```
  1
  2
  3
  4
  6
  ```

  > try가 성공적인 경우 catch문 실행 X => if문과 유사

  <br />

  - ExceptionEx3 : 예외가 발생하지 않는 경우

  ```java
  int[] numArr = null;
  System.out.println(1);
  System.out.println(2);

  try {
    System.out.println(3);
    numArr[0] = 1;
    System.out.println(4);
  } catch (NullPointerException npe) {
    System.out.println(5);
  }

  System.out.println(6);
  ```

  - 결과

  ```
  1
  2
  3
  5
  6
  ```

  > 예외가 발생한 순간, 그 이후 코드는 작동 X

  <br />

  - e.getMessage()

    - 발생한 예외 클래스의 인스턴스에 저장된 메시지를 얻을 수 있음(사용자용)

  - e.printStackTrace()

    - 예외 발생 당시의 호출 스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력(개발자용)

  <br />

  - 멀티 catch 블록

  ```java
  int[] numArr = null;
  System.out.println(1);
  System.out.println(2);

  try {
    System.out.println(3);
    System.out.println(100 / 0);
    numArr[0] = 1;
    System.out.println(4);
    // 멀티 catch 블록
  } catch (ArithmeticException | NullPointerException e) {
    e.printStackTrace();
    System.out.println(5);
  } catch (Exception e) { // 모든 예외 처리 가능
    System.out.println(6);
  }

  System.out.println(7);
  ```

  <br />

  - finally

  ```java
  int[] numArr = null;
  System.out.println(1);
  System.out.println(2);

  try {
    System.out.println(3);
    // System.out.println(100 / 0);
    // numArr[0] = 1;
    System.out.println(4);
    // 멀티 catch 블록
  } catch (ArithmeticException | NullPointerException e) {
    e.printStackTrace();
    System.out.println(5);
  } catch (Exception e) { // 모든 예외 처리 가능
    System.out.println(6);
  } finally {
    System.out.println("finally");
    System.out.println(7);
  }

  System.out.println(8);
  ```

<br />

- 예외 발생시키기

  - 프로그래머가 고의로 예외를 발생시킴

  - 키워드: throw

  - 방법

    1. 예외를 발생시키고 싶은 예외클래스 객체를 만듦

    2. throw를 이용해서 예외를 발생시킴

  ```java
  public static void main(String[] args) {
      try {
      Exception e = new Exception("고의로 예외 발생시킴");
      throw e;
    } catch (Exception e) {
      System.out.println("에러 메시지: " + e.getMessage());
      e.printStackTrace();
    }

    System.out.println("프로그램 종료");
  }
  ```

<br />

- 메서드에 예외 선언

  - 메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 개발자가 메서드의 선언부를 보았을 때, 해당 메서드를 사용하기 위해서 처리해야 할 예외들 확인 가능

  - 키워드: throws

  - 선언 방법

    1. 메서드 선언부에 키워드 throws를 사용해서 메서드 내에 발생할 수 있는 예외 작성

    2. 예외가 여러 개인 경우 쉼표(,)로 구분

  ```
  void method1() 예외1, 예외2, ,,, Exception {
    // 메서드 내용
  }
  ```

  ```java
  // class
  public class ExceptionEx7Class {

    // 메서드에 예외 선언
    void method1() throws ArithmeticException {
      System.out.println(123 / 2);
      System.out.println(123 / 0);
    }
    void method2() throws Exception {
      String str = null;
      str.equals("ad");
    }
  }
  ```

<br />

- 사용자 정의 예외

  - 기존의 예외 클래스 활용이 아닌 새로운 예외 클래스를 만들어 사용

  ```java
  public class MyException extends Exception {
    private final int ERR_CODE;

    public MyException(String msg, int errCode) {
      super(msg);
      ERR_CODE = errCode;
    }

    public int getERR_CODE() {
      return ERR_CODE;
    }

    public void getErrorMessage() {
      System.out.println("오류 발생");
      System.err.println("연락");
      super.printStackTrace();
    }
  }
  ```

  ```java
  public class StartInstall9 extends Exception {
    public void setupMethod() throws MyException {
      MyException me = new MyException("에러", 5000);
      throw me;
    }
  }
  ```

  ```java
  public class ExceptionEx9 {
    public static void main(String[] args) {
      StartInstall9 st = new StartInstall9();

      try {
        st.setupMethod();
      } catch (MyException e) {
        e.getErrorMessage();
        System.out.println(e.getERR_CODE());
        e.printStackTrace();
      }
    }
  }
  ```
