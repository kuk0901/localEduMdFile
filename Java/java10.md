# java

## java 10일차

- 다형성과 casting

  - Car class

  ```java
  package Test.java.model1;

  public class Car {
    public String color;
    public int door;

    public void drive() {
      System.out.println("운전 중");
    }

    public void stop() {
      System.out.println("stop!!");
    }
  }
  ```

  - FireEngine class

  ```java
  package Test.java.model1;

  public class FireEngine extends Car {
    public void water() {
      System.out.println("물대포 발사~");
    }
  }
  ```

  - Ambulance class

  ```java
  package Test.java.model1;

  public class Ambulance extends Car{
    public void siren() {
      System.out.println("siren~");
    }
  }
  ```

  <br />

  - CarTest

  ```java
  package Test.java.part8;

  import Test.java.model1.Car;
  import Test.java.model1.FireEngine;

  public class CarTest {
    public static void main(String[] args) {
      Car car = null;

      // 소방차 1대
      FireEngine fe1 = new FireEngine();
      FireEngine fe2 = null;

      fe1.water();

      car = (Car)fe1;
      // car.water();

      fe2 = (FireEngine)car;
      fe2.water();
    }
  }
  ```

  <br />

  - CarTest2

  ```java
  package Test.java.part8;

  import Test.java.model1.Ambulance;
  import Test.java.model1.Car;
  import Test.java.model1.FireEngine;

  public class CarTest2 {
    public static void main(String[] args) {

      Car car = new Car();

      // 소방차 1대
      FireEngine fe1;
      Ambulance am1 = null;

  //    System.out.println(fe1);

      fe1 = null;

      System.out.println(fe1);
  //    fe1.water();

      // 부모 객체는 자식 참조 변수에 할당 불가능
  //    fe1 = car;
  //    fe1.drive();
  //    fe1.stop();
  //    fe1.water();

      // 명시적 형 변환
      am1 = (Ambulance) new Car();
      am1.siren();
    }
  }
  ```

<br />

- 참조 변수의 형 변환

  - 기본형 변수와 같이 참조 변수도 형 변환 가능

  - 표현식: (변환하고자 하는 타입의 이름)

  - 서로 상속 관계에 있는 클래스 사이에서만 형 변환 가능

  > 자식 타입 -> 부모 타입(Up-casting) : 형 변환 생략 가능 <br /><br />
  > 자식 타입 <- 부모 타입(Down-casting) : 형 변환 생략 불가

<br />

- 형 변환 사용 예시

  ```java
  // Product class
  package Test.java.model2;

  public class Product {
    public int price; // 제품의 가격
    public int bonusPoint; // 제품 구매 시 제공하는 보너스 점수

    public Product(int price) {
      this.price = price;
      bonusPoint = (int) (price / 10.0); // 보너스 점수는 제품 가격의 10%
    }
  }
  ```

  ```java
  // Tv class
  package Test.java.model2;

  public class Tv extends Product {
    public Tv() {
      super(100);
    }

    @Override
    public String toString() {
      return "Tv";
    }
  }
  ```

  ```java
  // Com class
  package Test.java.model2;

  public class Com extends Product {
    public Com() {
      super(200);
    }

    @Override
    public String toString() {
      return "Computer";
    }
  }
  ```

  ```java
  // Buyer class
  package Test.java.model2;

  public class Buyer {
    public int money = 1000;
    public int bonusPoint = 0;

    public void buy(Tv t) {
      if (money < t.price) {
        System.out.println("잔액이 부족하여 물건을 살 수 없습니다.");
        return;
      }

      money -= t.price;
      bonusPoint += t.bonusPoint;

      System.out.println(t.toString() + "을/를 구입하셨습니다.");
    }

    public void buy(Com com) {

      if (money < com.price) {
        System.out.println("잔액이 부족하여 물건을 살 수 없습니다.");
        return;
      }

      money -= com.price;
      bonusPoint += com.bonusPoint;

      System.out.println(com.toString() + "을/를 구입하셨습니다.");
    }
  }
  ```

  ```java
  // ProductTest class
  package Test.java.part8;

  // import Test.java.model2.*;
  import Test.java.model2.Buyer;
  import Test.java.model2.Com;
  import Test.java.model2.Tv;

  public class ProductTest {
    public static void main(String[] args) {
      Buyer buyer = new Buyer();

      Tv t = new Tv();
      buyer.buy(t);

      System.out.println("현재 남은 돈은 " + buyer.money + "만원입니다.");
      System.out.println("현재 보너스 점수는 " + buyer.bonusPoint + "점입니다.");

      System.out.println();

      Com com = new Com();
      buyer.buy(com);

      System.out.println("현재 남은 돈은 " + buyer.money + "만원입니다.");
      System.out.println("현재 보너스 점수는 " + buyer.bonusPoint + "점입니다.");
    }
  }
  ```

  <br />

  - 다형성 인수 활용해 소스 코드 수정

  ```java
  // Buyer class
  package Test.java.model2;

  public class Buyer {
    public int money = 1000;
    public int bonusPoint = 0;

    public void buy(Product pro) {
      if (money < pro.price) {
        System.out.println("잔액이 부족하여 물건을 살 수 없습니다.");
        return;
      }

      money -= pro.price;
      bonusPoint += pro.bonusPoint;

      System.out.println(pro.toString() + "을/를 구입하셨습니다.");
    }
  }
  ```

<br />

- 추상 클래스(Abstract class)

  - 미완성 설계도에 비유 가능

  - 완성되지 못한 채로 남겨진 설계도

  - 클래스가 미완성이라는 것은 멤버의 개수와 관계 X, "미완성 메서드(추상 메서드)"를 포함하고 있다는 의미

  - 미완성 설계도로 완성된 제품을 만들 수 없듯 **추상 클래스로 인스턴스 생성 불가**

  - 추상 클래스는 상속 후 자식 클래스에 의해서만 완성될 수 있음

  - 키워드: abstract

<br />

- 추상 메서드

  - 메서드 선언부만 작성하고 구현부는 작성하지 않은 채로 남겨둔 것

  - 표현식: abstract 리턴타입 메서드명(); => 브레이스({}) 없음 주의

  ```
  - 메서드의 내용이 상속받는 클래스에 따라 달라질 수 있기 때문에 부모 클래스에서는 선언부만 작성 => 주석을 덧붙여 어떤 기능을 수행할 목적인지 알려줌

  - 실제 내용은 상속받는 클래스에서 구현하도록 비워둠
  ```

<br />

- 추상 클래스 작성

  > 추상: 낱낱의 구체적 표상이나 개념에서 공통된 성질을 뽑아 이를 일반적인 개념으로 파악하는 정신 작용

  - IT의 추상화: **클래스 간의 공통점**을 찾아내서 **공통의 부모**를 만드는 작업

  - IT의 구체화: **상속을 통해** 클래스를 **구현, 확장**하는 작업

<br />

- 추상 클래스 실습 : Animal

  - 어떤 동물인지 출력하는 메서드

  - 동물의 이름을 출력하는 추상 메서드

  - 추상 메서드 재정의

  ```java
  // Animal class
  public abstract class Animal {

    public abstract void name();

    public void kind() {

    }
  }
  ```

  ```java
  // Dog class
  public class Dog extends Animal {

    // 추상 메서드 구현부 작성
    public void name() {
      System.out.println("강아지");
    }

    // Overriding
    @Override
    public void kind() {
      System.out.println("동물");
    }
  }
  ```

  ```java
  // main class

  public class AnimalTest {
    public static void main(String[] args) {
      Dog dog = new Dog();

      dog.name();
      dog.kind();
    }
  }
  ```
