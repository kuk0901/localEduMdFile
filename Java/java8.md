# 8일차

## java 8일차

- 포함 관계(Composite)

  - 클래스의 멤버 변수로 다른 클래스 타입의 참조 변수가 선언돼 있는 것

```java
public class Point {

  int x;
  int y;

}
```

```java
public class Circle {

  Point point = new Point();
  int r;

}
```

```java
public class CircleTest {
  Circle c = new Circle();

  c.point.x = 2;
  c.point.y = 4;
  c.r = 3;

  System.out.println(c.point.x);
  System.out.println(c.point.y);
  System.out.println(c.r);
}
```

<br />

- CardCase class 만들어 사용 과제

  - 카드 섞는 메서드 생성

  - 카드케이스의 전체 카드 보여주는 메서드 생성 => 한 줄에 13개씩

```java
package Test.java.part7;

public class CardCase {

  // 전체 카드 수
  int numOfCards = Card.shape.length * Card.number.length;
  Card[] cardArr = new Card[numOfCards];

  CardCase() {

  }

  // 52장의 카드가 나란히 만들어짐
  void init() {
    int totalCnt = 0;

    for (int i = 0 ; i < Card.shape.length ; i++) {
      for (int j = 0 ; j < Card.number.length ; j++) {
        cardArr[totalCnt++] = new Card(i, j);
      }
    } // 전체 for end
  }

  Card pick() {
    int index = (int) (Math.random() * numOfCards);

    Card card = cardArr[index];

    return card;
  }

  // 카드 직접 뽑기
  Card pick(int index) {
    Card card = cardArr[index];

    return card;
  }

  // 카드 섞기
  void shuffle() {
    int[] intTempArr = new int[numOfCards];
    Card[] cardTempArr = new Card[numOfCards];

    // 중복 없는 랜덤 숫자 뽑기 반복문
    for (int i = 0; i < cardArr.length; i++) {
      intTempArr[i] = (int) (Math.random() * numOfCards);

      // 중복 제거를 위한 반복문
      for (int j = 0; j < i; j++) {
        if (intTempArr[j] == intTempArr[i]) {
          i--;
        }
      }
    }

    for (int n = 0; n < intTempArr.length; n++) {
      int tempIndex = intTempArr[n];
      cardTempArr[n] = cardArr[tempIndex];
    }

    for (int k = 0; k < cardTempArr.length; k++) {
      cardArr[k] = cardTempArr[k];
    }
  }

  // 카드케이스의 전체 카드 보기 => 13개씩 한 줄
  void cardLook() {

    for (int i = 0; i < cardArr.length; i += 13) {

      for (int j = i; j < i + 13; j++) {
        System.out.print(cardArr[j].getCard() + " ");
      }

      System.out.println();
    }
  }
}
```

<br />

- 오버라이딩(Overriding)

  - 부모 클래스로부터 상속받은 메서드의 내용을 변경하는 것

  - 상속받은 메서드를 그대로 사용하기도 하지만, 자식 클래스 자신에 맞게 변경해야 하는 경우 사용

<br />

- 오버라이딩의 조건

  > 자식 클래스에서 오버라이딩하는 메서드는 부모 클래스의 메서드와 아래 조건이 일치해야 함

  - **이름**이 같아야 함

  - **매개변수**가 같아야 함

  - **반환타입**이 같아야 함

  ```
  1. 접근 제어자는 부모 클래스의 메서드보다 좁은 범위로 변경 불가

  2. 부모 클래스의 메서드보다 많은 수의 예외 선언 불가

    - 단순히 선언된 예외의 개수 문제가 아닌, 적은 수의 예외라도 Exception 같은 최고 조상의 예외인 경우 가장 많은 개수의 예외를 던질 수 있도록 선언한 것이라는 의미를 기억해야 함

  3. 인스턴스 메서드를 static 메서드로 또는 그 반대로 변경 불가
  ```

  > 부모의 static 메서드는 자식 클래스에서 오버라이딩 불가능 <br /><br />
  > => 오버라이딩이 아닌 서로 구별되는 static 메서드로 동일 이름은 정의 가능하나, 오버라이딩의 개념으로 적용되지 않음

<br />

- super

  - 자식 클래스에서 부모 클래스로부터 상속받은 멤버를 참조하는 데 사용되는 참조 변수

  - 멤버 변수와 지역 변수의 이름이 같을 때 "this"를 사용해 구별하듯 상속받은 멤버와 자신의 클래스에 정의된 멤버의 이름이 같을 때 "super"를 사용해서 구별 가능

  > super와 this는 근본적으로 같지만 구별해서 사용해야 좋은 객체지향 프로그래밍 구성 가능 <br /> <br />
  > 둘 다 인스턴스 메서드에서만 사용 가능 => static 메서드에서는 사용 X

<br />

- super()

  - 부모 클래스의 생성자 호출

  - this()와 마찬가지로 super() 역시 생성자

  - this()는 같은 클래스의 다른 생성자를 호출하는 데 사용되지만, super()는 부모 클래스의 생성자를 호출하는 데 사용

  - 주의사항

    ```
    - 생성자의 첫 줄에서 부모 클래스의 생성자를 호출해야 함

    => 자식 클래스의 멤버가 부모 클래스의 멤버를 사용할 수 있으므로 부모의 멤버들이 먼저 초기화되어 있어야 하기 때문


    - Object 클래스를 제외한 모든 클래스의 생성자 첫 줄에서는 생성자, this() 도는 super()를 호출해야 함

    => 호출하지 않는 경우 컴파일러가 자동으로 super()를 생성자의 첫 줄에 삽입
    ```
