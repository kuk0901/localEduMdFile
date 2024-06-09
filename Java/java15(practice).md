# 팀 프로젝트(간접 체험)

## 커피 매장 프로젝트 구현

- 주제: 커피 매장 프로젝트 구현하기

<br />

- 인원수: 3인(각자 역할 분담)

  - Kiosk

  - Client(본인)

  - Drink line

<br />

- 무조건 구현해야 할 객체(필요하면 추가, 단 남발 X)

  1. 고객(client)

     - 이름, 나이, 연락처, 소지금, 쿠폰

  <br />

  2. 아이스 라떼(iceLatte), 카페 라떼(cafeLatte), 자몽 에이드(grapeFruitAde)

     - 종류, 이름, 가격

     > 위의 정보는 변하지 않으므로 상수로 사용

  <br />

  3. 메뉴판(키오스크, Kiosk)

     - 고민해서 설계

     ```
     1. 키오스크는 음료들을 가짐 => 음료의 모든 정보

     2. 키오스크에서 주문을 함 => 수량, 가격
     ```

  <br />

  4. 커피 매장

     - 매장의 기본 소지금: 100,000원

     - 무엇을 얼마나 팔았는지 확인 가능

     - 손님이 누가 왔는지 확인 가능

     - 메뉴 재고 확인 가능

     - 음료는 각각 10개 있으며, 0개인 경우 품절이라고 메뉴판에 표시

     - 모든 품목이 품절이면 매장의 총수익을 알리고 프로그램은 종료

<br />

- 프로그램 수행 방법: **일반적인 고객이 커피를 주문하는 절차** 를 파악하고 구현

<br />

- package 구조 & class 이름

  ```
  cafe

    - coffeeShop: CoffeeShop, Kiosk

    - menu: Drink(abstract class), CafeLatte, IceLatte, GrapeFruitAde

    - client: Client

    - main: CoffeeShopProgram
  ```

<br />

- class

  - 공통 속성을 갖는 클래스(음료들)를 묶어 다형성을 활용하기 위해 추상 클래스(Drink) 사용

    > 아직 다형성 활용 부분 미완성

  - CoffeeShop은 Kiosk를 가짐

  - Kiosk는 mene들을 가짐

<br />

- class 구현

  - menu - Drink

  ```java
  package cafe.menu;

  public abstract class Drink {

    public String menuType = "";
    public String menuName = "";
    public int menuPrice = 0;

    public abstract String menuType();
    public abstract String menuName();
    public abstract int menuPrice();
  }
  ```

  <br />

  - menu - CafeLatte

  ```java
  package cafe.menu;

  public class CafeLatte extends Drink {

    private final String menuType = "coffee";
    private final String menuName = "카페라떼";
    private final int menuPrice = 4500;

    @Override
    public String menuType() {
      // TODO Auto-generated method stub
      return menuType;
    }

    @Override
    public String menuName() {
      // TODO Auto-generated method stub
      return menuName;
    }

    @Override
    public int menuPrice() {
      // TODO Auto-generated method stub
      return menuPrice;
    }

  }
  ```

  <br />

  - menu - IceLatte

  ```java
  package cafe.menu;

  public class IceLatte extends cafe.menu.Drink {

    private final String menuType = "coffee";
    private final String menuName = "아이스라떼";
    private final int menuPrice = 5000;

    @Override
    public String menuType() {
      // TODO Auto-generated method stub
      return menuType;
    }

    @Override
    public String menuName() {
      // TODO Auto-generated method stub
      return menuName;
    }

    @Override
    public int menuPrice() {
      // TODO Auto-generated method stub
      return menuPrice;
    }

  }
  ```

  <br />

  - menu - GrapeFruitAde

  ```java
  package cafe.menu;

  public class GrapeFruitAde extends cafe.menu.Drink {

    private final String menuType = "Ade";
    private final String menuName = "자몽에이드";
    private final int menuPrice = 6000;


    @Override
    public String menuType() {
      // TODO Auto-generated method stub
      return menuType;
    }

    @Override
    public String menuName() {
      // TODO Auto-generated method stub
      return menuName;
    }

    @Override
    public int menuPrice() {
      // TODO Auto-generated method stub
      return menuPrice;
    }

  }
  ```

  <br />

  - client - Client

    - 고객 객체

  ```java
  package client;

  public class Client {
    private String name;
    private int age;
    private String phoneNumber;
    private int amount;
    private int coupon;

    public Client() {

    }

    public Client(String name, int age, String phoneNumber, int amount, int coupon) {
      this.name = name;
      this.age = age;
      this.phoneNumber = phoneNumber;
      this.amount = amount;
      this.coupon = coupon * 10;
    }

    public String getName() {
      return name;
    }

    public void setName(String name) {
      this.name = name;
    }

    public int getAge() {
      return age;
    }

    public void setAge(int age) {
      this.age = age;
    }

    public String getPhoneNumber() {
      return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
      this.phoneNumber = phoneNumber;
    }

    public int getAmount() {
      return amount;
    }

    public void setAmount(int amount) {
      this.amount -= amount;
    }

    public int getCoupon() {
      return coupon;
    }

    public void setCoupon(int coupon) {
      this.coupon = coupon;
    }

    @Override
    public String toString() {
      return "Client [name=" + name + ", age=" + age + ", phoneNumber=" + phoneNumber + ", amount=" + amount
          + ", coupon=" + coupon + "]";
    }

  }
  ```

  <br />

  - coffeeShop - Kiosk

  ```java
  package cafe.coffeeShop;

  import cafe.menu.CafeLatte;
  import cafe.menu.GrapeFruitAde;
  import cafe.menu.IceLatte;

  import java.util.HashMap;

  public class Kiosk {

    private IceLatte iceLatte = new IceLatte();
    private CafeLatte cafeLatte = new CafeLatte();
    private GrapeFruitAde grapeFruit = new GrapeFruitAde();

    // menu의 정보들을 담을 변수
    public HashMap<Integer, String> menuInfo = new HashMap<>();

    // coffee menu의 정보를 담음
    public void coffeeMenu() {
      String iceLatteMenu = iceLatte.menuName() + "(" + iceLatte.menuPrice() + "원" + ")";
      String cafeLatteMenu = cafeLatte.menuName() + "(" + cafeLatte.menuPrice() + "원" + ")";

      menuInfo.put(1, iceLatteMenu);
      menuInfo.put(2, cafeLatteMenu);
    }

    // ade menu의 정보를 담음
    public void adeMenu() {
      String grapeFruitMenu = grapeFruit.menuName() + "(" + grapeFruit.menuPrice() + "원" + ")";

      menuInfo.put(3, grapeFruitMenu);
    }

    // 메뉴를 선택할 경우 수량과 가격을 곱해서 반환
    public int selectIceLatte(int num) {

      return iceLatte.menuPrice() * num;
    }

    public int selectCafeLatte(int num) {

      return cafeLatte.menuPrice() * num;
    }

    public int selectGrapeFruit(int num) {

      return grapeFruit.menuPrice() * num;
    }

  }
  ```

  <br />

  - cafe - CoffeeShop

  ```java
  package cafe.coffeeShop;

  import java.util.ArrayList;

  public class CoffeeShop {

    // 매장은 키오스를 가짐
    private Kiosk kiosk = new Kiosk();

    // 수량과 해당 음료의 총 값
    public int iceLatteCount = 10;
    public int totalIceLatteAmount;

    public int cafeLatteCount = 10;
    public int totalCafeLatteAmount;

    public int grapeFruitCount = 10;
    public int totalGrapeFruitAmount;

    // 매장의 기본 소지금
    public int funds = 100000;

    // 고객이 결제해야 하는 최종 비용
    private int totalAmount;

    // 매장을 방문한 고객 이름을 담을 변수
    private ArrayList<String> clientList = new ArrayList<>();

    // 음료 주문 함수
    public void iOrder(int num) {

      if (iceLatteCount == 0) {
        System.out.println("아이스라떼는 품절되었습니다.");

        return;
      }

      totalIceLatteAmount = kiosk.selectIceLatte(num);

      iceLatteCount -= num;

    }

    public void cOrder(int num) {

      if (cafeLatteCount == 0) {
        System.out.println("카페라떼는 품절되었습니다.");

        return;
      }

      totalCafeLatteAmount = kiosk.selectCafeLatte(num);

      cafeLatteCount -= num;

    }

    public void gOrder(int num) {

      if (grapeFruitCount == 0) {
        System.out.println("자몽에이드는 품절되었습니다.");

        return;
      }

      totalGrapeFruitAmount = kiosk.selectGrapeFruit(num);

      grapeFruitCount -= num;

    }

    // 고객이 결제해야 할 최종 결제 금액을 계산 후 반환
    public int totalAmount() {

      totalAmount = totalIceLatteAmount + totalCafeLatteAmount + totalGrapeFruitAmount;

      return totalAmount;
    }

    // 결제 후 기본 소지금에 가격을 더함
    public void getClientMoney() {
      funds += totalAmount;
    }

    // 메뉴 보여줌
    public void menu() {
      System.out.println(" 【 메뉴판 】 ");
      kiosk.coffeeMenu();
      kiosk.adeMenu();

      // 음료들의 정보와 수량을 합친 String 변수들
      String iceLatteMenu = kiosk.menuInfo.get(1) + iceLatteCount;
      String cafeLatteMenu = kiosk.menuInfo.get(2) + cafeLatteCount;
      String grapeFruitAdeMenu = kiosk.menuInfo.get(3) + grapeFruitCount;

      System.out.println(" 『Coffee』 ");
      System.out.println(iceLatteMenu + "개" + " || " + cafeLatteMenu + "개");
      System.out.println(" 『Ade』 ");
      System.out.println(grapeFruitAdeMenu + "개");

    }

    // 방문한 고객의 이름을 ArrayList 변수에 담음
    public void setClientList(String name) {
      clientList.add(name);
    }

    // 방문한 고객 확인을 위해 고객리스트 반환
    public ArrayList<String> getClientList() {
      return clientList;
    }

    // 매장의 소지금 반환
    public int getFunds() {
      return funds;
    }

  }
  ```

  <br />

  - main - CoffeeShopProgram

  ```java
  package main;

  import cafe.coffeeShop.CoffeeShop;
  import client.Client;

  import java.util.Scanner;

  public class CoffeeShopProgram {

    public static void main(String[] args) {

      Scanner sc = new Scanner(System.in);
      CoffeeShop coffeeShop = new CoffeeShop();

      // 고객의 정보를 입력받을 변수들
      String name;
      int age;
      String phoneNumber;
      int amount;
      int coupon;

      // 음료 수량을 받을 변수들
      int iceLatteInput;
      int cafeLatteInput;
      int grapeFruitAdeInput;

  //		boolean as = true;
  //		int i = 0;
      while (true) {

        if (coffeeShop.iceLatteCount == 0 && coffeeShop.cafeLatteCount == 0 && coffeeShop.grapeFruitCount == 0) {
          System.out.println("모든 음료가 품절되었습니다.");
          break;
        }

        System.out.println("저희 매장을 찾아주셔서 감사합니다.");
        System.out.println("사용자 정보를 입력해 주세요.");
        System.out.println();

        // 사용자 정보 입력
        System.out.println("이름을 입력해 주세요.");
        name = sc.nextLine();

        System.out.println("나이를 입력해 주세요.");
        age = sc.nextInt();

        sc.nextLine();

        System.out.println("휴대폰 번호를 입력해 주세요.");
        phoneNumber = sc.nextLine();

        System.out.println("소지하고 계신 금액을 입력해 주세요.");
        amount = sc.nextInt();

        // 코드 추가 필요
        System.out.println("소지하고 계신 쿠폰의 값을 입력해 주세요.");
        System.out.println("10%는 1, 20%는 2, 없으면 0");
        coupon = sc.nextInt();

        // 사용자 생성
        Client client = new Client(name, age, phoneNumber, amount, coupon);

        System.out.println();
        // 카페 영업 시작
        System.out.println("메뉴판을 보여드리겠습니다.");
        coffeeShop.menu(); // 메뉴 보여줌

        System.out.println();
        System.out.println("원하시는 음료의 수량을 작성해 주세요.");
        System.out.println("(ex: 아이스라떼: 3, 카페라떼: 0)");

        System.out.println();

        // 음료 수량 받기
        System.out.print("아이스라떼: ");
        iceLatteInput = sc.nextInt();

        System.out.print("카페라떼: ");
        cafeLatteInput = sc.nextInt();

        System.out.print("자몽에이드: ");
        grapeFruitAdeInput = sc.nextInt();

        // 음료 주문
        coffeeShop.iOrder(iceLatteInput);
        coffeeShop.cOrder(cafeLatteInput);
        coffeeShop.gOrder(grapeFruitAdeInput);

        System.out.println();
        System.out.println("【 주문 확인 】");
        System.out.println("아이스라떼: " + iceLatteInput + "잔, 금액: " + coffeeShop.totalIceLatteAmount);
        System.out.println("카페라떼: " + cafeLatteInput + "잔, 금액: " + coffeeShop.totalCafeLatteAmount);
        System.out.println("자몽에이드: " + grapeFruitAdeInput + "잔, 금액: " + coffeeShop.totalGrapeFruitAmount);

        System.out.println();
        System.out.println("총 주문 금액: " + coffeeShop.totalAmount());

        client.setAmount(coffeeShop.totalAmount());

        System.out.println("고객님의 남은 금액: " + client.getAmount() + "원");

        coffeeShop.getClientMoney();
        coffeeShop.setClientList(client.getName());

        System.out.println();
        sc.nextLine();
        System.out.println();
  //			i++;

      }

      System.out.println("매장의 총수익: " + coffeeShop.funds);
      System.out.println("매장을 방문한 손님: " + coffeeShop.getClientList());
    }

  }
  ```

<br />

- 결과

  ```
  * 문제점

    - 미완성


    - 다형성 인수(Drink)를 활용하지 못함


    - 고객의 coupon 부분을 현재 사용하지 못함(할인 쿠폰)


    - 현재 무엇을 얼마나 팔았는지 확인 불가

      - 메뉴판에 나온 상태에서 자체적인 계산으로만 확인 가능


    - 음료의 수량이 0개인 경우 품절이라고 메뉴판에 표시되는 부분 구현 못함


    - 프로그램 동작을 위해 임시로 main class에 기능(method) 분리 가능한 코드가 작성되어 있음

    - 프로그램 동작을 위해 몇몇 field의 접근 제어자가 public

  ===============================================================

  * 원하는 결과?

    - 최대한 객체 지향적으로 작성하기 위해 객체 구현 후 해당 객체들을 서로가 갖게 함


    - class들의 method와 field를 최대한 활용하게 함


      - ex) Kiosk class 안에서 IceLatte class의 method 사용 등


    - 불필요한 인스턴스 변수가 될 수 있는 경우 지역 변수로 대체


    - 팀원 역할 분담 후 구현한 각 class 조립

      - 부족한 부분은 모여서 의견 정리 후 추가 로직 작성
  ```
