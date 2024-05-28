# 3일차

## Java 3일차

- Array(배열)

  - 배열을 "new" 연산자를 이용해 생성할 경우, 메모리 공간에 초깃값이 데이터 타입에 맞춰 들어감

  - 정수 => 초깃값 0

  - 실수 => 초깃값 0.0

  - 문자 => ' '(공백)

  - boolean => false

  - 문자열 => null

  > 초깃값은 다른 값에 영향을 주지 않아야 함

  ```java
  - 변수와 메모리 공간 사용

  - 번지(주소) 값(16진수) 작성

  - 번지(주소) 값(16진수)이 가리키는

  * 컴파일 오류 : 발생 시 실행 자체가 되지 않음

  * 런타임 오류 : 코드가 실행되나 실행할 때 오류 => 런타임 오류 시 main 탈출
  ```

  <br />

- 배열의 for문

  ```
  마지막 단계, 동일하게 적용될 무언가가 있는 경우에 반복문 사용

  => 초기화를 위해, 값 할당을 위한 for문 사용은 잘못된 코드
  ```

  <br />

- 문자열 배열

  ```java
  package Test.java.part5;

  public class ArrayBasic4 {
    public static void main(String[] args) {
      String[] strArr = { "홍길동", "이순신", "김유신" };

      System.out.println(strAtt.length);
      System.out.println("=================");

      for (int i = 0; i < strArr.length; i++) {
        System.out.println(strArr[i]);
      }
    }
  }
  ```

<br />

- 포인터: 가리킴

- 인덱스(index): 위치를 잡음

 <br />

- 100, 88, 87, 100이 들어간 배열, 이 모든 값의 총점과 평균을 구하는 로직

  - 소수점 3째 자리에서 반올림

  ```java
  int[] scoreArr = {12, 67, 21, 99};
  int totalScore = 0;
  double avgScore = 0.0;

  for (int i = 0; i < scoreArr.length; i++) {
    totalScore += scoreArr[i];
  }

  avg = (double) totalScore / scoreArr.length;

  avg = (int)(((double) totalScore / scoreArr.length + 0.005) 100) / 100.0;

  System.out.println("총점: " + totalScore);
  System.out.println("평균: " + avgScore);
  ```

  <br />

- 79, 88, 91, 33, 100, 55이 들어간 배열에서 가장 큰 값과 최솟값을 구하는 로직

  ```java
  package Test.java.part5;

  public class ArrayTest2 {
    public static void main(String[] args) {
      int[] nums = {79, 88, 91, 33, 100, 55};
      int max = nums[0];
      int min = nums[0];

      // 최댓값
      for (int i = 0; i < nums.length; i++) {
        if (nums[i] > max) {
          max = nums[i];
        }
      }

      // 최솟값
      for (int i = 0; i < nums.length; i++) {
        if (nums[i] < min) {
          min = nums[i];
        }
      }

      System.out.println("최댓값: " + max);
      System.out.println("최솟값: " + min);
    }
  }
  ```

  <br />

- 로또 번호를 출력하는 프로그램

  ```java
  int[] ballArr = new int[45];

  for (int i = 0; i < ballArr; i++) {
    ballArr[i] = i + 1;
  }

  int temp = 0; // 치환용 임시 변수
  int n = 0; // 인덱스 접근

  for (int i = 0; i < 6; i++) {
    n = (int) (Math.random() * 45);

    temp = ballArr[i];
    ballArr[i] = ballArr[n];
    ballArr[n] = temp;
  }

  for (int i = 0; i < 6; i++) {
    System.out.println("[" + ( i + 1) + "]" + " 자동으로 나온 로또 번호: " + ballArr[i];)
  }

  for (int i = 0; i < ballArr.length; i++) {
    System.out.print(ballArr[i] + " ");
  }
  ```

<br />

- 버블 정렬을 이용한 배열의 숫자 오름차순

  ```java
  int[] jumsuArr = { 70, 90, 50, 40, 100 };

  for (int i = 0; i < jumsuArr.length; i++) {
    System.out.print(jumsuArr[i] + " ");
  }

  System.out.println();

  int temp = 0;

  for (int i = 0; i < jumsuArr.length; i++) {
    for (int n = 0; n < jumsuArr.length - 1; n++) {
      if (jumsuArr[n] > jumsuArr[n + 1]) {
        temp = jumsuArr[n];
        jumsuArr[n] = jumsuArr[n + 1];
        jumsuArr[n + 1] = temp;
      }
    }
  }

  for (int i = 0; i < jumsuArr.length; i++) {
    System.out.print(jumsuArr[i] + " ");
  }
  ```

<br />

- 2차원 배열

  - 선언: 데이터타입[][] 변수명 = new 데이터타입[1차원배열크기][2차원배열크기]

  ```java
  int[][] nums = new int[4][3];

  scoreArr[0][0] = 1;
  ```

<br />

- 객체지향 개념

  - 객체지향프로그래밍(Object Oriented Programming)

  - 객체지향이론의 기본 개념

  ```
  - 실제 세계는 사물(객체)로 이루어져 있으며, 발생하는 모든 사건들은 사물 간의 상호작용임

  - 실제 사물의 속성과 기능을 분석한 다음, 데이터(변수)와 함수로 정의함으로써 실제 세계를 컴퓨터 속에 옮겨 놓은 것과 같은 가상 세계를 구현하고 이 가상 세계에서 모의실험을 함으로써 많은 시간과 비용 절약 가능
  ```

  <br />

- 객체지향의 주요 특징

  1. 코드의 재사용성이 높음

     - 새로운 코드를 작성할 때 기존의 코드를 이용해 쉽게 작성 가능

  2. 코드 관리 용이(유지보수)

     - 코드 간의 관계를 이용해 적은 노력으로 쉽게 코드 변경 가능

  3. 신뢰성 높은 프로그래밍

     - 제어자와 메서드를 이용해 데이터를 보호하고 올바른 값을 유지하도록 하며, 코드의 중복을 제거해 코드의 불일치로 인한 오동작 방지 가능

  <br />

- 코드 작성 방법

  - 객체 지향을 고민하기보단 기능적으로 완성해 보며 어떻게 하면 보다 객체지향적으로 코드를 개선할 수 있을까를 고민하여 점차 코드를 개선해 나가는 것이 좋음

  <br />

- 클래스(class)와 객체(object, instance)

  - 클래스(class)

    - 객체를 정의해 놓은 것, 객체의 설계도 || 툴

  - class의 용도

    - 객체 생성에 사용

  <br />

  - 객체(object)

    - 실제로 존재하는 것, 사물 또는 개념

  - 객체의 용도

    - 객체가 갖고 있는 기능과 속성에 따라 다름

  - 유형의 객체

    - 책상, 의자, 자동차, TV와 같은 사물

  - 무형의 객체

    - 수학 공식, 프로그램 에러, 시간 같은 논리나 개념

  ```
  ex)
  클래스          객체
  제품 설계도      제품
  TV 설계도       TV
  붕어빵틀        붕어빵
  ```

  <br />

- 객체와 인스턴스

  - 클래스로부터 객체를 만드는 과정을 클래스의 인스턴스화 라고 하며, 어떤 클래스로부터 만들어진 객체를 그 클래스의 인스턴스(instance)라고 함

<br />

- 객체의 구성 요소 => 속성과 기능

  ```
  - TV의 속성 : 크기, 길이, 높이, 색상, 볼륨, 채널 등

  - TV의 기능 : 켜기, 끄기, 볼륨 높이기, 볼륨 낮추기, 채널 변경하기 등
  ```

  - 속성(property)

    - 멤버(member) 변수, 특성(attribute), 필드 등

  <br />

  - 기능(function)

    - 메서드(method), 함수, 행위 등

  <br />

  - 사용자 정의 타입(user-defined type) => class

    - 기본적인 것들만 예측해 만들어 주고 자신에게 맞는 것이 필요하다면 스스로 타입을 만들어 사용하라고 제공해 준 것

<br />

- 인스턴스 생성 표현식

  - 클래스명 변수명;

  - 변수명 = new 클래스명();

  ```java
  TvVO tv1;
  tv1 = new TvVO();
  ```

  <br />

- class 생성 및 사용

  ```java
  package Test.java.model1;

  public class TvVO {
    private String color;
    private boolean power;
    private int channel;

    public void power() {
      power = !power;
    }

    public void setChannel(int channel) {
      this.channel = channel;
    }

    public int getChannel() {
      return this.channel;
    }

    public void setChannelUp() {
      channel++;
      System.out.println("채널 1 증가");
    }

    public void setChannelDown() {
      channel--;
      System.out.println("채널 1 감소");
    }
  }
  ```

  ```java
  package Test.java.part6;

  import Test.java.model1.TvVO;

  public class ObjectBasic1 {
    public static void main(String[] args) {
      TvVO tv = new TvVO();
      tv.setChannel(7);

      tv.setChannelUp();
      tv.setChannelDown();

      System.out.println("현재 채널은 " + tv.getChannel() + " 입니다.");
    }
  }
  ```
