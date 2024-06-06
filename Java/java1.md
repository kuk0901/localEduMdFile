# 1일차

## Java 1일차

- JDK : 자바 개발도구

- JRE : 자바 실행환경

<br />

- JDK vs JRE

  - JDK(Java Development kit) : 자바 언어로 프로그램을 개발할 수 있도록 지원하는 도구들이 포함

    - JRE + 개발에 필요한 실행파일(수 많은 것 중에 javac라는 컴파일러가 있음)

  <br />

  - JRE(Java Runtime Environment) : 개발은 불가능 하지만 자바 언어로 개발된 프로그램을 실행할 수 있음(실행에 관련된 지원만 함)

    - JVM + 클래스라이브러리(Java API)

<br />

- 자바(Java)

  - 운영 체제(Operation System, 플랫폼)에 독립적

  - 자바로 작성한 프로그램은 운영 체제의 종류에 관계 없이 실행 가능

<br />

- 객체지향언어

  - 객체 지향 개념의 특징인 상속, 다형성, 캡슐화 등이 잘 적용된 순수한 객체 지향 언어

<br />

- 자동 메모리 관리(Garbage Collection)

  - 가비지컬렉터가 자동으로 메모리 관리

  - 몇 가지를 제외하고 대부분 프로그래머가 메모리를 따로 관리하지 않아도 됨

<br />

- 네트워크와 분산 처리 지원

<br />

- 멀티 스레드 지원

<br />

- 동적 로딩(Dynamic Loading) 지원

  - 실행 시에 모든 클래스가 로딩되지 않고 필요한 시점에 클래스를 로딩하여 사용 => 메모리를 효율적으로 사용 가능

<br />

- JVM(Java Virtual Machine)

  - 자바 가상 머신, 자바를 실행하기 위한 가상 기계

  - JVM 때문에 운영 체제에 독립 가능

<br />

- 이클립스 단축어

  ```
  syso 작성 후 ctrl + space: System.out.println(");
  ```

<br />

- 명명 규칙: 이름을 짓는 규칙

  - 클래스 이름의 첫 글자는 대문자

  - 변수와 메서드의 이름의 첫 글자는 소문자

  - 여러 단어로 이루어진 경우 두 번째 단어부터 첫 글자를 대문자로 사용

  - 상수의 이름은 모두 대문자, 여러 단어로 이루어진 경우 '\_'으로 구분

  > 낙타 표기법 : indexOf <br /><br />
  > 파스칼 표기법 : StringBuffer <br /><br />
  > 스네이크 표기법 : student_age

<br />

- 기본형과 참조형

  - 기본형(primitive type): 계산을 위한 실제 값 저장, 8개

  - 선언 방식: 타입 변수명;

  <br />

  - 참조형(reference type): 객체의 주소(번지) 저장, 기본형을 제외한 나머지 타입

  - 선언 방식: 클래스이름 변수이름;

<br />

- 합성(concat)

  - any type + 문자열 => 문자열 + 문자열 => 문자열

  - 문자열 + any type => 문자열 + 문자열 => 문자열

<br />

- 코드를 읽는 순서

  - =(할당 연산자)를 사용: 우 -> 좌

  - 단순 계산: 좌 -> 우

  ```java
  System.out.println(" ", " ", 7); // "  7"
  System.out.println(7 + 7 + ""); // "14"
  System.out.println("" + 7 + 7); // "77"

  // 계산한 결과가 함수의 매개변수로 들어감
  ```

  > 할당에서의 계산 순서가 우에서 일어난 후 좌로 할당

<br />

- scanner를 사용한 입력 받기

  ```java
  import java.util.Scanner;

  public class VarStringBasic2 {
    public static void main(String[] args) {

      Scanner sc = new Scanner(System.in);

      System.out.println("아무 글이나 입력");

      String input = sc.nextLine();
      System.out.println("작성한 글 -> " + input);

      int num = sc.nextInt();
      System.out.println("작성한 글 -> " + num);

    }
  }
  ```
