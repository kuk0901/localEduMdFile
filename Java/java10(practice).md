- 다형성 과제

  - Tv, 컴퓨터, 스마트폰, 책 구매 가능

  - 고객은 제품을 하나만 가질 수 있음

  - 고객은 구매한 제품 사용 가능

    - 책 use 시 "책을 읽는 중" 출력

    - 컴퓨터 use 시 "com을 사용 중" 출력

    - 스마트폰 use 시 "폰 사용 중" 출력

    - Tv use 시 "tv 시청 중" 출력

  <br />

  - 패키지 구분

    > 서로 관련 있는 클래스끼리 묶음

    1. customer

    2. main

    3. product

  <br />

  - PolyArgumentTest class

    - 사용자가 입력한 값에 따라 고객이 물건을 하나만 구매하며 구매한 후, 물건을 사용함

    ```
    구매할 물건의 번호를 입력하세요
    1번 tv
    2번 컴퓨터
    3번 스마트폰
    4번 책
    3

    SmartPhone을/를 구입하셨습니다.
    현재 남은 돈은 930만원입니다.
    현재 보너스점수는 7점입니다.

    폰 사용 중
    ```

  <br />

  - 주의할 점

    - 접근 제어자 사용 범위

    - 자동 형 변환 되는지 안 되는지 생각

    - 다형성은 무조건 상속이 전제조건

  <br />

- Product class

  - super 클래스

```java
package Test.java.ch7.polyAnswer.product;

public class Product {
  public int price; // 제품의 가격
  public int bonusPoint; // 제품 구매 시 제공하는 보너스 점수

  public Product(int price) {
    this.price = price;
    bonusPoint = (int) (price / 10.0); // 보너스 점수는 제품 가격의 10%
  }

  public void use() {
    System.out.println("사용 가능한 제품이 없습니다.");
  }
}
```

<br />

- Tv class

  - sub 클래스

```java
package Test.java.ch7.polyAnswer.product;

public class Tv extends Product {
  public Tv() {
    // 조상 클래스의 생성자 Product(int price) 호출
    super(100); // Tv의 가격: 100만원
  }

  public String toString() { // Object 클래스의 toString()을 오버라이딩
    return "Tv";
  }

  @Override
  public void use() {
    System.out.println("tv 시청 중");
  }
}
```

<br />

- Computer class

  - sub 클래스

```java
package Test.java.ch7.polyAnswer.product;

public class Computer extends Product {

  public Computer() {
    super(200);
  }

  public String toString() {
    return "Computer";
  }

  @Override
  public void use() {
    System.out.println("com을 사용 중");
  }
}
```

<br />

- SmartPhone class

  - sub 클래스

```java
package Test.java.ch7.polyAnswer.product;

public class SmartPhone extends Product {

  public SmartPhone() {
    super(70);
  }

  public String toString() {
    return "SmartPhone";
  }

  @Override
  public void use() {
    System.out.println("폰 사용 중");
  }
}
```

<br />

- Book class

  - sub 클래스

```java
package Test.java.ch7.polyAnswer.product;

public class Book extends Product {

  public Book() {
    super(1);
  }

  public String toString() {
    return "Book";
  }

  @Override
  public void use() {
    System.out.println("책을 읽는 중");
  }
}
```

<br />

- Customer class

```java
package Test.java.ch7.polyAnswer.customer;

import Test.java.ch7.polyAnswer.product.Product;

public class Customer {

  public int money = 1000; // 소유 금액
  public int bonusPoint = 0; // 보너스 점수

  public void buy(Product product) {
    if (money < product.price) {
      System.out.println("잔액이 부족하여 문걸은 살 수 없습니다.");
      return;
    }

    money -= product.price; // 가진 돈에서 구입한 제품의 가격을 뺌
    bonusPoint += product.price; // 제품의 보너스 점수 추가
    System.out.println(product + "을/를 구입하셨습니다.");
  }

  @Override
  public String toString() {
    String str = "현재 남은 돈은 " + money + "만원입니다.\n";
    str += "현재 보너스점수는 " + bonusPoint + "점입니다.";

    return str;
  }

  public void use(Product product) {
    product.use();
  }
}
```

<br />

- PolyArgumentTest class(main class)

```java
package Test.java.ch7.polyAnswer.main;

import Test.java.ch7.polyAnswer.customer.Customer;
import Test.java.ch7.polyAnswer.product.*;

import java.util.Scanner;

public class PolyArgumentTest {
  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);

    System.out.println("구매할 물건의 번호를 입력하세요.");
    System.out.println("1번 tv");
    System.out.println("2번 컴퓨터");
    System.out.println("3번 스마트폰");
    System.out.println("4번 책");

    // 키보드 입력
    int num = scanner.nextInt();

    System.out.println();

    Customer customer = new Customer();

    // switch문을 이용: num 값에 따라 다른 결과
    switch (num) {
      case 1: {
        Tv tv = new Tv();

        customer.buy(tv);

        System.out.println(customer);
        System.out.println();

        customer.use(tv);

        break;
      }

      case 2: {
        Computer computer = new Computer();

        customer.buy(computer);

        System.out.println(customer);
        System.out.println();

        customer.use(computer);

        break;
      }

      case 3: {
        SmartPhone smartPhone = new SmartPhone();

        customer.buy(smartPhone);

        System.out.println(customer);
        System.out.println();

        customer.use(smartPhone);

        break;
      }

      case 4: {
        Book book = new Book();

        customer.buy(book);

        System.out.println(customer);
        System.out.println();

        customer.use(book);

        break;
      }

      default:
        break;
    }
  }
}
```
