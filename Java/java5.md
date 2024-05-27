# 5일차

## java 5일차

- return문

  - 현재 실행 중인 메서드를 종료하고 호출한 메서드로 되돌아감

  - 모든 메서드는 적어도 하나 이상의 return문이 반드시 있어야 함

  ```java
  public class MyMath {

    // void return문
    void add(int a, int b) {
      System.out.println(a + b);
      return; // 명시하지 않고 생략 가능
    }
  }
  ```

<br />

- 사칙연산에서 유효성 검사(validation)

  ```java
  void div(int a, int b) {
    if (a == 0 || b == 0) {
      s
    }
  }
  ```

<br />

- JVM 메모리 구조

  - Static Area

    - 프로그램 실행 중 어떤 클래스가 사용되면, JVM은 해당 클래스의 클래스 파일(\*.class)을 읽고 분석해 클래스에 대한 정보(클래스 데이터)를 static area에 저장

    - 클래스의 static 변수, static 메서드 모두 static zone 영역에 생성

    > instance(new 키워드로 인해 객체)가 생성되면 none-static zone에 멤버 메서드가 생성

  <br />

  - Heap Area

    - 인스턴스가 생성되는 공간, 프로그램 실행 중 생성되는 인스턴스는 모두 영역에 생성

    - 인스턴스 변수(instance variable)들이 생성되는 공간

  <br />

  - Stack

    - 메서드 작업에 필요한 메모리 공간을 제공

    ```
    1. 메서드가 호출되면 호출 스택에 호출된 메서드를 위한 메모리 할당

    2. 해당 메모리는 메서드가 작업을 수행하는 동안 지역변수(매개변수 포함)들과 연산의 중간 결과 등을 저장하는 데 사용

    3. 메서드가 작업을 마치면 할당되었던 메모리 공간은 반환되어 비워짐


    => 각 메서드를 위한 메모리상의 작업 공간은 서로 구별됨

    => 첫 번째 호출된 메서드를 위한 작업 공간이 호출 스택의 맨 밑에 마련되고 첫 번째 메서드 수행 중 다른 메서드 호출 시 그 위에 두 번째로 호출된 메서드를 위한 공간 마련

    => 메서드가 수행을 마치면 사용했던 메모리를 반환하고 스택에서 제거됨
    ```

<br />

- class와 method를 이용한 구구단

  ```java
  // return 타입 int
  int gugudan1(int num) {
    for (int i = num; i <= 9; i++) {
      for (int j = 1; j <= 9; j++) {
        System.out.print(i + " * " + j + " = " + (i * j) + "\t");
      }
      System.out.println();
    }
    return 0;
  }

  // return 타입 void
  void gugudan2(int num) {
    for (int i = num; i <= 9; i++) {
      for (int j = 1; j <= 9; j++) {
        System.out.print(i + " * " + j + " = " + (i * j) + "\t");
      }
      System.out.println();
    }
  }
  ```

  - int를 return하는 gugudan1 메서드: 의미 없는 리턴 타입
