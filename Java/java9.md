# 9일차

## java 9일차

- 패키지(package)

  - 서로 관련된 클래스끼리 그룹 단위로 묶어 놓음으로써 클래스를 효율적으로 관리 가능

  - 다른 개발자가 개발한 클래스 라이브러리의 클래스와 이름 충돌 방지

  <br />

  ```java
  - 하나의 소스 파일에는 첫 번째 문장으로 단 한 번의 패키지 선언만을 허용

  - 모든 클래스는 반드시 하나의 패키지에 속해야 함

  - 패키지는 점(.)을 구분자로 하여 계층 구조(계단식)로 구성 가능

  -  패키지는 물리적으로 클래스 파일(.class)을 포함하는 하나의 디렉토리
  ```

  > 클래스가 물리적으로 하나의 클래스 파일(.class)인 것과 같이 패키지는 **물리적으로 하나의 디렉토리**

<br />

- 클래스 풀네임(full name)

  - 클래스의 실제 이름(full name)은 패키지명을 포함한 것

  - 같은 이름의 클래스일지라도 서로 다른 패키지에 속하면 패키지명으로 구별 가능

<br />

- 패키지 선언

  - 표현식 : package 패키지명;

  - 패키지 선언문은 반드시 소스 파일 첫 번째 문장에 작성

  - 하나의 소스 파일에 단 한 번만 선언

  - 패키지명은 대소문자를 모두 허용하나 클래스명과 구분하기 위해 소문자로 하는 것이 원칙

  - 이름 없는 패키지(default package)는 사용 X

<br />

- import문

  - 소스 코드를 작성할 때 다른 패키지의 클래스를 사용하려면 패키지명이 포함된 클래스 이름 사용

  - **import문으로 사용하고자 하는 클래스의 패키지를 미리 명시**할 경우, 소스 코드에 사용되는 클래스 이름에서 패키지명 생략 가능

<br />

- import문 선언

  - 일반적으로 소스 파일(\*.java)의 구성은 다음 순서

  ```
  1. package문

  2. import문

    - 하나의 클래스만 불러오고 싶은 경우: import 패키지명.클래스명;

    - 패키지에 속한 모든 클래스를 불러오고 싶은 경우: import 패키지명.*;

  3. 클래스 선언문
  ```

<br />

- 캡슐화(encapsulation)

  - 정보 은닉, 보안

<br />

- 제어자(Modifier)

  - 클래스, 변수, 메서드의 선언부에 함께 사용되어 부가적인 의미 부여

<br />

- 제어자의 종류

  - 접근 제어자: public, protected, default, private

  - 그 외: static, final, abstract, native, transient, synchronized, volatile.. 등

  - 제어자는 클래스나 멤버에 주로 사용되며, 하나의 대상에 대해 여러 제어자를 조합해 사용 가능

  - 접근 제어자는 하나만 선택해서 사용 가능

  ```
  - static

    - 인스턴스를 생성하지 않고 호출이 가능한 메서드가 됨


  - final

    - 사용할 수 있는 곳: 클래스, 메서드, 멤버 변수, 지역 변수

    1. 클래스: 변경될 수 없는 클래스, "확장될 수 없는 클래스"가 됨

      => 다른 클래스의 부모가 될 수 없음

    2. 메서드: 변경될 수 없는 메서드

      => 오버라이딩을 통한 "재정의 불가"

    3. 멤버 및 지역 변수: 값을 변경할 수 없는 "상수"가 됨
  ```

<br />

- 접근 제어자

  - 사용할 수 있는 곳: 클래스, 멤버 변수, 메서드, 생성자

  ```
  - private

    - "같은 클래스" 내에서만 접근 가능


  - default

    - "같은 패키지" 내에서만 접근 가능


  - protected

    - 같은 패키지 내에서, 다른 패키지의 하위 클래스(상속 관계)에서 접근 가능


  - public

    - 접근 제헌 없음
  ```

<br />

- ???에 사용할 수 있는 접근 제어자

  - class: public, default

  - method, 멤버 변수: public, default, protected, private

  - 지역 변수: 전부 사용 불가

<br />

- 접근 제어자 사용 이유

  - 외부로부터 데이터 보호

  - 외부에는 불필요한 내부적으로만 사용되는 부분을 감춤 => 캡슐화 => 정보 은닉

<br />

- getter & setter

  > 예약어 아님

  - set

    - 값을 넣음

    - 대입을 함

    - 세팅을 함

  - get

    - 값을 반환

    - 가져옴

    - 내용을 봄

  <br />

  - getter 사용 규칙

    - 반환형이 멤버 변수와 같은 형식

    - 매개변수 없음

    ```java
    int num;

    int getNum() {
      return num;
    }
    ```

  <br />

  - setter 사용 규칙

    - 반환형 void

    - 멤버 변수와 동일한 형식의 매개변수 존재

    ```java
    int num;

    void setNum(int num) {
      this.num = num;
    }
    ```

<br />

- 도서관리 프로그램 실습

  - 클래스명: Book

  - 멤버 변수: title, author, publisher, price, publishedDate

  - 메서드: getter, setter

  > 실제 존재하는 책 2권의 정보 출력 => getter 이용 <br />
  > default constructor && overloading

  ```java
  // class
  package Test.java.model1;

  public class Book {
    private String title;
    private String author;
    private String publisher;
    private int price;
    private String publishedDate;

    public Book() {

    }

    public Book(String title, String author, String publisher, int price, String publishedDate) {
      this.title = title;
      this.author = author;
      this.publisher = publisher;
      this.price = price;
      this.publishedDate = publishedDate;
    }

    public String getTitle() {
      return title;
    }

    public void setTitle(String title) {
      this.title = title;
    }

    public String getAuthor() {
      return author;
    }

    public void setAuthor(String author) {
      this.author = author;
    }

    public String getPublisher() {
      return publisher;
    }

    public void setPublisher(String publisher) {
      this.publisher = publisher;
    }

    public int getPrice() {
      return price;
    }

    public void setPrice(int price) {
      this.price = price;
    }

    public String getPublishedDate() {
      return publishedDate;
    }

    public void setPublishedDate(String publishedDate) {
      this.publishedDate = publishedDate;
    }
  }
  ```

  ```java
  // main class
  package Test.java.part8;

  import Test.java.model1.Book;

  public class BookTest {
    public static void main(String[] args) {
      Book book1 = new Book();

      // book1 setter use
      book1.setTitle("숨결이 바람 될 때");
      book1.setAuthor("폴 칼라니티");
      book1.setPublisher("흐름출판");
      book1.setPrice(14000);
      book1.setPublishedDate("2016.08.22");

      // book1 getter use
      System.out.println("제목: " + book1.getTitle());
      System.out.println("저자: " + book1.getAuthor());
      System.out.println("출판사: " + book1.getPublisher());
      System.out.println("가격: " + book1.getPrice() + "원");
      System.out.println("발행: " + book1.getPublishedDate());

      System.out.println(); // 개행용

      Book book2 = new Book("마법의 순간", "파울로 코엘료", "자음과모음", 4800, "2013.05.09");

      // book2 getter use
      System.out.println("제목: " + book2.getTitle());
      System.out.println("저자: " + book2.getAuthor());
      System.out.println("출판사: " + book2.getPublisher());
      System.out.println("가격: " + book2.getPrice() + "원");
      System.out.println("발행: " + book2.getPublishedDate());
    }
  }
  ```

<br />

- 다형성(polymorphism)

  - 여러 가지 형태를 가질 수 있는 능력 => 중요 특징 중 하나

  > 주의사항: 상속과 깊은 관계가 있어 상속 선행학습 필요

  ```
  자바에서는 "한 타입의 참조 변수로 여러 타입의 객체를 참조할 수 있도록 함"

  또는

  부모 클래스 타입이 참조 변수로 자식 클래스의 인스턴스 참조 가능
  ```

  <br />

  - 부모 타입은 자식 타입의 객체 할당 가능

  ```java
  import basic.CaptionTv;
  import basic.Tv;

  Tv t = new CaptionTv();

  CaptionTv capTv = new Tv(); // error

  CaptionTv capTv2 = new CaptionTv();
  ```

  ```java
  package basic;

  public class Tv {
    public boolean power;

    public void power() {
      power = !power;
    }
  }
  ```

  ```java
  package basic;

  public class CaptionTv extends Tv {
    public String text;

    public void caption() {

    }
  }
  ```
