# 2일차

## Java 2일차

- boolean

  - 기본 자료형(primitive type)

  - 참(true), 거짓(false)을 나타내는 자료형 타입

  ```java
  public class PrimitiveTypeBasic1 {
    public static void main(String[] args) {
      boolean b = false;

      System.out.println(b);

      if (b) {
        System.out.println("조건문 실행?");
      }
    }
  }
  ```

<br />

- Casting(형 변환)

  - booleand을 제외한 나머지 7개의 기본형은 서로 형 변환 가능

  - 기본형과 참조형은 서로 형 변환 불가능

  - 서로 다른 타입 간의 연산은 형 변환하는 것이 원칙이나, 값의 범위가 작은 타입에서 큰 타입으로 형 변환 생략 가능

  <br />

  - 총점과 평균 구하기

  ```java
  public class CastingBasic1 {
    public static void main(String[] args) {
      int firstNum = 100;
      int secondNum  = 43;
      int totalNum = firstNum + secondNum;
      double avg = (double)totalNum / 2;

      System.out.println(totalNum);
      System.out.println(avg);
    }
  }
  ```

  <br />

  - 소수점 올림, 반올림

  ```java
  public class CastingBasic1 {
    public static void main(String[] args) {

      int firstNum = 10;
      int secondNum  = 3;
      int totalNum = firstNum + secondNum;


      double avg = (double)totalNum / 3;

      // 소수점 셋째 자리에서 내림
      System.out.println(avg);

      // 소수점 셋째 자리에서 내림
      double avg = (int)((double)totalNum / 3 * 100) / 100.0;

      System.out.println(avg);

    }
  }
  ```

<br />

- switch문

  - 제어문 중 하나

  - 변수의 값에 따라 실행문을 선택하게 되는 구문

  ```java
  import java.util.Scanner;

  public class SwitchBasic1 {
    public static void main(String[] args) {
      Systme.out.println("점수를 입력하세요.");
      Scanner sc = new Scanner(System.in);

      int score = sc.nextInt();
      String grade = "";

      switch (score / 10) {
        case 9:
          grade = "A";
          break;
        case 8:
          garde = "B";
          break;
        case 7:
          garde = "C";
          break;
        case 6:
          garde = "D";
          break;
        default :
          garde = "F";
          break;
      }

      System.out.println("학점 : " + grade);
    }
  }
  ```

  <br />

  - switch문을 이용한 계절

  ```java
  public class SwitchBasic2 {
    public static void main(String[] args) {

      int month = 5;
      String monthStr = "";

      switch (month) {
        case 3: case 4: case 5:
          monthStr = "봄";
          break;

        case 6: case 7: case 8:
          monthStr = "여름";
          break;

        case 9: case 10: case 11:
          monthStr = "가을";
          break;

        case 12: case 1: case 2:
          monthStr = "겨울";
          break;

        default:
          monthStr = "특이한 계절";
          break;
      }
      System.out.println("학점 : " + monthStr);
    }
  }
  ```

  <br />

  - switch문 + 반복문

    - 사용자가 1 || 2 입력

    - 입력에 따라 구구단 결과 출력 : 1이면 세로, 2면 가로 출력

  ```java
  import java.util.Scanner;

  public class SwitchTest2 {
    public static void main(String[] args) {
      System.out.println("숫자를 입력하세요.(1 또는 2)");
      Scanner sc = new Scanner(System.in);

      int userNum = sc.nextInt();
      int firNum = 2;
      int secNum = 1;

      switch (userNum) {
        case 1:
          while (secNum <= 9) {
            firNum = 2;

            if (secNum > 9) {
              break;
            }

            while (firNum <= 9) {
              System.out.printf("%d * %d = %d \t", firNm, secNum, firNum * secNum)
            }
          }
      }
    }
  }
  ```

<br />

- 삼항 연산자, 조건 연산자

  - 조건 ? 식1 : 식2

  - 조건 true == 식1 실행

  - 조건 false == 식2 실행

  ```java
  int rslt = 0;

  rslt = 5 > 0 ? 10 : -10;
  System.out.println("rslt"); // 10

  rslt = 5 < 0 ? 10 : -10;
  System.out.println("rslt"); // -10
  ```

<br />

- for문

  - 표현식(expression)

  ```
  for (초기식; 조건식; 증감식) {
    조건이 참일 때 실행될 문장들 작성
  }
  ```

  <br />

  - 반복 횟수를 알고 있을 때 적합, 구조가 직관적이라 이해하기 쉬움

  ```java
  public class ForBasic {
    public static void main(String[] args) {

      for (int i = 2; i <= 9; i++) {

        for (int j = 1; j <= 9; j++) {
          System.out.println(i + " * " + j + " = " + (i * j));
        }

        System.out.println();

      }

    }
  }
  ```

  <br />

  - for문으로 10부터 -5까지 출력

  ```java
  public class ForTest3 {
    public static void main(String[] args) {

      for (int i = 10; i >= -5; i--) {

        if (i == 4 | i == -1) {
          System.out.println();
        }

        System.out.printf("%d ", i);
      }

    }
  }
  ```

  <br />

  - 주사위 눈 6이 나올 때까지 반복, 6이 되면 지금까지 주사위를 몇 번 굴리었는지 출력

  ```java
  public class ForTest4 {

    public static void main(String[] args) {

      int diceNum = 0;
      int i = 0;

      for (;;) {

        diceNum = (int)(Math.random() * 6) + 1;
        i++;
        System.out.println("(" + diceNum + ")");

        if (diceNum == 6) {
          break;
        }
      }

      System.out.println("주사위를 굴린 총 횟수: " + i);
    }
  }
  ```

  <br />

  - for문을 이용해서 구구단 출력

    - 출력문으로 print() 메서드 사용

  ```java
  public class ForTest5 {
    public static void main(String[] args) {

      // row
      for (int i = 2; i <= 9; i++) {
        // column
        for (int j = 1; j <= 9; j++) {
          System.out.print(i + " * " + j + " = " + (i * j) + "\t");
        }

        System.out.println();
      }

      // row
      for (int i = 1; i <= 9; i++) {
        // column
        for (int j = 2; j <= 9; j++) {
          System.out.print(j + " * " + i + " = " + (j * i) + "\t");
        }

        System.out.println();

      }
    }
  }
  ```

  <br />

- 배열(Array)

  - 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것

  ```java
  package Test.java.part5;

  public class ArrayBasic1 {
    public static void main(String[] args) {
      int a1 = 1;
      int a2 = 2;
      int a3 = 3;

      System.out.println(a1);
      System.out.println(a2);
      System.out.println(a3);

      System.out.println("======================");

      int[] numArr = new int[3];
      numArr[0] = 10;
      numArr[1] = 20;
      numArr[2] = 30;

      System.out.println(numArr[0]);
      System.out.println(numArr[1]);
      System.out.println(numArr[2]);

      int[] numArr2 = {11, 22, 33};

      System.out.println(numArr2[0]);
      System.out.println(numArr2[1]);
      System.out.println(numArr2[2]);
    }
  }
  ```

  <br />

  - 배열없이 국어 점수 여러 개 만들어 출력

  ```java
  package Test.java.part5;

  public class ArrayBasic2 {
    public static void main(String[] args) {
      int kor1 = 10;
      int kor2 = 20;
      int kor3 = 30;
      int kor4 = 40;
      int kor5 = 50;

      System.out.println("1번 국어점수: " + kor1);
      System.out.println("2번 국어점수: " + kor2);
      System.out.println("3번 국어점수: " + kor3);
      System.out.println("4번 국어점수: " + kor4);
      System.out.println("5번 국어점수: " + kor5);

      int korNum = 10;
      System.out.println("1번 국어점수: " + korNum);

      korNum = 20;
      System.out.println("2번 국어점수: " + korNum);

      korNum = 30;
      System.out.println("3번 국어점수: " + korNum);

      korNum = 40;
      System.out.println("4번 국어점수: " + korNum);

      korNum = 50;
      System.out.println("5번 국어점수: " + korNum);
    }
  }
  ```

  <br />

  - 배열에 2, 4, 6, 8, 10 저장 후 출력

  ```java
  public class ArrayTest1 {
    public static void main(String[] args) {
      int[] arr1 = new int[5];

      // 반복문 안에서 할당 및 출력
      for (int i = 0; i < arr1.length; i++) {
        arr1[i] = (i + 1) * 2;
        System.out.print(arr1[i] + "\t");
      }

      // 배열에 값 할당 후 출력
      arr1[0] = 2;
      arr1[1] = 4;
      arr1[2] = 6;
      arr1[3] = 8;
      arr1[4] = 10;

      for (int i = 0; i < arr1.length; i++) {
        System.out.print(arr[i] + "\t");
      }

      // 짝수를 이용한 배열에 값 저장 및 출력
      int[] arr2 = new int[5];
      int count = 0; // 짝수 개수를 저장하는 변수

      for (int i = 1; i <= 10; i++) {

        if (i % 2 == 0) { // 짝수인지 판단
          if (count < arr2.length) { // 배열에 공간이 있는지 확인
            arr2[count] = i;
            count++;
          }
        }
      }

      System.out.println(count);
      System.out.println();

      for (int i = 0; i < count; i++) {
        System.out.print(arr2[i] + "\t");
      }

      // 배열 선언 및 초기화
      int[] arr3 = {2, 4, 6, 8, 10};

      for (int a: arr3) {
        System.out.print(a + "\t")
      }
    }
  }
  ```

  <br />

  - 숫자를 입력받고 누적값 출력, 0 입력할 경우 종료

  ```java
  package Test.java.part5;

  import java.util.Scanner;

  public class IfTest1 {
    public static void main(String[] args) {
      // 누적값 출력
      Scanner sc = new Scanner(System.in);
      int acc = 0;

      System.out.print("숫자를 입력해 주세요(0 입력 시 프로그램 종료): ");

      while (true) {
        int userNum = sc.nextInt();
        acc += userNum;

        if (userNum == 0) {
          break;
        }
        System.out.print("누적 합계: " + acc + "\n");
      }
      System.out.print("최종 합계: " + acc + "\n");
      System.out.println("프로그램 종료");
    }
  }
  ```

  <br />

  - 소수 여부 구하기

  ```java
  package Test.java.part5;

  import java.util.Scanner;

  public class IfTest2 {
    public static void main(String[] args) {
      // 소수 출력
      Scanner sc = new Scanner(System.in);
      System.out.print("숫자를 입력해 주세요: ");
      int num = sc.nextInt();

      if (num == 0 || num == 1) {
        System.out.print(num + "은(는) 소수가 아닙니다.\n");
      } else if ((num > 2 && num % 2 == 0) || (num > 3 && num % 3 == 0)) {
        System.out.print(num + "은(는) 소수가 아닙니다.\n");
      } else if ((num > 2 && num % 2 != 0) || (num > 3 && num % 3 != 0)) {
        System.out.print(num + "은(는) 소수입니다.\n");
      }

      System.out.println("프로그램 종료");
    }
  }
  ```

  <br />

  - 윤년 구하기

  ```java
  package Test.java.part5;

  import java.util.Scanner;

  public class IfTest3 {
    public static void main(String[] args) {
      // 윤년 출력
      Scanner sc =  new Scanner(System.in);
      System.out.print("년도를 입력해 주세요: ");
      int year = sc.nextInt();

      if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) {
        System.out.print(year + "년은 윤년입니다." + "\n");
      } else if (year % 4 != 0 && year % 100 == 0 || year % 400 != 0) {
        System.out.print(year + "년은 윤년이 아닙니다." + "\n");
      }

      System.out.println("프로그램 종료");
    }
  }
  ```
