# 6일차

## java 6일차

- 변수의 생명주기(Life Cycle)

  - static method는 main문이 수행되자마자 바로 사용가능

  - 일반 메서드는 new 문장 이후로 사용 가능

<br />

- 기본형 매개변수와 참조형 매개변수

  - primitive tye VS reference type

  - call by value VS call by reference

  | 기본형 매개변수               | 참조형 매개변수             |
  | ----------------------------- | --------------------------- |
  | 변수의 값을 읽기만 할 수 있음 | 변수의 값을 읽고 변경 가능  |
  | read only                     | read & write                |
  | 기본값만 할당 가능            | 인스턴스 주소값만 할당 가능 |

<br />

- 구구단 클래스 만들어 사용

  - 다른 패키지에서 해당 클래스 사용하기 위해 일반 메서드에 public 추가

  - 인스턴스 변수가 없는 경우

  ```java
  package Test.java.model1;

  public class GugudanVO {

    void wantDanPrint(int dan) {

      System.out.println(dan + "단");
      for (int i = 1; i <= 9; i++) {
        System.out.println(dan + " * " + i + " = " + (dan * i));
      }

      System.out.println();
    }
  }
  ```

  <br />

  - GugudanTest(main class)

  ```java
  public class GugudanTest() {
    public static void main(String[] args) {
      GugudanVO gg = new GugudanVO();

      gg.wantDanPrint(2);
    }
  }
  ```

<br />

- 사칙연산 클래스 만들어 사용

  - 다른 패키지에서 해당 클래스 사용하기 위해 일반 메서드에 public 추가

  - 인스턴스 변수가 있는 경우

  ```java
  package Test.java.model1;

  public class MyMathVO {
    int firstNum;
    int secondNum;

    int add() {
      return firstNum + secondNum;
    }

    int sub() {
      return firstNum - secondNum;
    }

    int mul() {
      return firstNum * secondNum;
    }

    int div() {
      if (firstNum == 0 | secondNum == 0) {
        return 0;
      }

      return firstNum / secondNum;
    }

    int rem() {
      if (secondNum == 0) {
        return 0;
      }

      return firstNum % secondNum;
    }
  }
  ```

  <br />

  - MyMathTest(main class)

  ```java
  public class MyMathTest() {
    public static void main(String[] args) {
      MyMathVO mm = new MyMathVO();

      mm.firstNum = 4;
      mm.secondNum = 2;

      System.out.println(mm.add());
      System.out.println(mm.sub());
      System.out.println(mm.mul());
      System.out.println(mm.div());
      System.out.println(mm.rem());

    }
  }
  ```
