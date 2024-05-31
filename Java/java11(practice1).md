# 과제1

- 2024/05/31

1. 싱글톤 getInstance
2. 인터페이스 원숭이, 동물주인 작성

## Singtone Pattern

- 싱글톤 패턴

  - 특정 클래스가 단 하나만의 인스턴스를 생성하여 사용하기 위한 디자인 패턴

  - 단순한 정의: 객체의 인스턴스가 오직 1개만 생성되는 패턴

  <br />

  - Singletone class

  ```java
  package Test.java.SingletonePractice;

  public class Singletone {
    // static 변수의 접근 제어자 private => 외부에서 접근 불가
    // static 변수 : 공유 변수 => 데이터 공유 가능
    private static Singletone instance = new Singletone();

    // 생성자의 접근 제어자가 private => 객체 생성 불가
    private Singletone() {
      System.out.println("객체 생성");
    }

    public static Singletone getInstance() {
      System.out.println("객체 반환");

      return instance;
    }
  }
  ```

  <br />

  - SingletoneTest class => main class

  ```java
  package Test.java.SingletonePractice;

  public class SingletoneTest {
    public static void main(String[] args) {
      Singletone i1 = Singletone.getInstance();
      Singletone i2 = Singletone.getInstance();
      Singletone i3 = Singletone.getInstance();

      System.out.println();

      // 주소값 확인 코드
      System.out.println("주소값 확인");
      System.out.println(i3);
      System.out.println(i2);
      System.out.println(i3);
      System.out.println();

      // 객체의 주소 비교 코드
      System.out.println("객체의 주소 비교");
      System.out.println("i1.equals(i2) : " + i1.equals(i2));
      System.out.println("i2.equals(i3) : " + i2.equals(i3));
      System.out.println("i1.equals(i3) : " + i1.equals(i3));
    }
  }
  ```

  <br />

  - main class 실행 결과

  ```
  객체 생성
  객체 반환
  객체 반환
  객체 반환

  주소값 확인
  Test.java.SingletonePractice.Singletone@24d46ca6
  Test.java.SingletonePractice.Singletone@24d46ca6
  Test.java.SingletonePractice.Singletone@24d46ca6

  객체의 주소 비교
  i1.equals(i2) : true
  i2.equals(i3) : true
  i1.equals(i3) : true
  ```

<br />

- 인터페이스 원숭이, 동물 주인 작성

  > java11.md interface 실습 내용 참고

  - monkey : animals implements 해서 작성

  - 동물 주인 : petOwner class 생성

  - main class : MyPetRaisingTest

  <br />

  - Monkey class

  ```java
  package Test.java.ch7.inter.exam.monkey;

  import Test.java.ch7.inter.exam.inter.Animals;

  public class Monkey implements Animals {

    // 추상 메서드 구현
    @Override
    public void eat(String bab) {
      System.out.println("원숭이가 " + bab + "을/를 먹어요");
    }

    @Override
    public void work(String move) {
      System.out.println("원숭이가 " + move + " 걸어요");
    }

    @Override
    public void sleep(String zzz) {
      System.out.println("원숭이가 " + zzz + " 잔다");
    }

    // 원숭이의 특징
    public void use(String using) {
      System.out.println("원숭이가 " + using + "을/를 사용해요");
    }

    public void lean(String leaning) {
      System.out.println("원숭이가 " + leaning + "을/를 배워요");
    }

    public void call() {
      System.out.println("Monkey");
    }

    @Override
    public String toString() {
      return "바나나킬러";
    }
  }
  ```

  <br />

  - MyPetRaisingTest(main class) 실행해 결과 확인

  ```java
  package Test.java.ch7.inter.exam.main;

  import Test.java.ch7.inter.exam.cat.FirstCat;
  import Test.java.ch7.inter.exam.cat.SecondCat;
  import Test.java.ch7.inter.exam.chicken.Chicken;
  import Test.java.ch7.inter.exam.monkey.Monkey;

  public class MyPetRaisingTest {
    public static void main(String[] args) {

      FirstCat fc = new FirstCat();
      fc.call();
      fc.eat("사료");
      fc.work("장난감 공");
      fc.sleep("누워서");
      System.out.println("=============================");

      SecondCat sc = new SecondCat();
      sc.call();
      sc.eat("사료");
      sc.work("장난감 낚시대");
      sc.talk("으르렁");
      sc.sleep("발라당");
      System.out.println("=============================");


      Chicken ch = new Chicken();
      ch.call();
      ch.eat("사료");
      ch.work("고개를 까딱이며");
      ch.sleep("앉아서");
      System.out.println("=============================");

      Monkey m = new Monkey();
      m.call();
      m.eat("바나나");
      m.work("나무 위를");
      m.sleep("나무 위에서");
      m.use("막대기");
      m.lean("막대기로 벌레 잡는 법");

    }
  }
  ```

  ```
  리치
  고양이가 사료을/를 먹어요
  고양이가 장난감 공을/를 가지고 걸어요
  고양이가 누워서 잔다
  =============================
  로아
  고양이가 사료을/를 먹어요
  꽁꽁 얼어붙은 한강 위로 고양이가 장난감 낚시대을/를 가지고 걸어다닙니다
  고양이는 으르렁 거려요
  고양이가 발라당 잔다
  =============================
  꼬꼬
  닭이 사료을/를 먹어요
  닭이 고개를 까딱이며걸어요
  닭이 앉아서 잔다
  =============================
  바나나킬러
  원숭이가 바나나을/를 먹어요
  원숭이가 나무 위를 걸어요
  원숭이가 나무 위에서 잔다
  원숭이가 막대기을/를 사용해요
  원숭이가 막대기로 벌레 잡는 법을/를 배워요
  ```

  > Monkey class의 이상 없음 확인 가능

  <br />

  - petOwner class

  ```java
  package Test.java.ch7.inter.exam.human;

  import Test.java.ch7.inter.exam.cat.Cat;
  import Test.java.ch7.inter.exam.cat.SecondCat;
  import Test.java.ch7.inter.exam.chicken.Chicken;
  import Test.java.ch7.inter.exam.inter.Animals;
  import Test.java.ch7.inter.exam.monkey.Monkey;

  public class PetOwner {
    private String name;
    private int age;
    private Animals[] animals = new Animals[2];

    public PetOwner(String name, int age) {
      this.name = name;
      this.age = age;
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

    public void getAnimals() {

      // 가독성을 위해 향상된 for문 사용
      for (Animals a : animals) {
        System.out.println(a);
      }
    }

    public void setAnimals(Animals animals, int index) {
      // instanceof를 이용해 객체 타입 확인
      if (animals instanceof Cat) {
        this.animals[index] = new SecondCat();
      } else if (animals instanceof Chicken) {
        this.animals[index] = new Chicken();
      } else if (animals instanceof Monkey) {
        this.animals[index] = new Monkey();
      }
    }

    // Animals interface 추상 메서드 사용 : Down-casting 필요없음
    public void animalsEat() {
      for (Animals a : animals) {
        if (a instanceof SecondCat) {
          a.eat("사료");
        } else if (a instanceof Chicken) {
          a.eat("사료");
        } else if (a instanceof Monkey) {
          a.eat("바나나");
        }
      }
    }

    public void animalsWork() {
      for (Animals a : animals) {
        if (a instanceof SecondCat) {
          a.work("장난감 낚시대");
        } else if (a instanceof Chicken) {
          a.work("고개를 까딱이며");
        } else if (a instanceof Monkey) {
          a.work("나무 위를");
        }
      }
    }

    public void animalsSleep() {
      for (Animals a : animals) {
        if (a instanceof SecondCat) {
          a.sleep("발라당");
        } else if (a instanceof Chicken) {
          a.sleep("앉아서");
        } else if (a instanceof Monkey) {
          a.sleep("나무 위에서");
        }
      }
    }

    // call()은 Animals interface 추상 메서드 X : Down-casting 후 사용
    public void animalsCall() {
      for (Animals a : animals) {
        if (a instanceof SecondCat) {
          ((SecondCat) a).call();
        } else if (a instanceof Chicken) {
          ((Chicken) a).call();
        } else if (a instanceof Monkey) {
          ((Monkey) a).call();
        }
      }
    }

    // 고양이만 갖고 있는 메서드 호출
    public void animalsTalk() {
      for (Animals a : animals) {
        if (a instanceof SecondCat) {
          ((SecondCat) a).talk("으르렁");
          break;
        }
      }
    }

    // 원숭이만 갖고 있는 메서드 호출
    public void animalsUse() {
      for (Animals a : animals) {
        if (a instanceof Monkey) {
          ((Monkey) a).use("막대기");
          break;
        }
      }
    }

    // 원숭이만 갖고 있는 메서드 호출
    public void animalsLean() {
      for (Animals a : animals) {
        if (a instanceof Monkey) {
          ((Monkey) a).lean("막대기로 벌레 잡는 법");
          break;
        }
      }
    }
  }
  ```

  > 각각의 동물 클래스가 개별로 갖고 있는 메서드 호출하는 perOwner의 메서드의 문제점 확인 필요

  <br />

  - PetOwnerTest(main class) 실행해 결과 확인

  ```java
  package Test.java.ch7.inter.exam.main;

  import Test.java.ch7.inter.exam.cat.SecondCat;
  import Test.java.ch7.inter.exam.chicken.Chicken;
  import Test.java.ch7.inter.exam.human.PetOwner;
  import Test.java.ch7.inter.exam.monkey.Monkey;

  import java.util.Scanner;

  public class PetOwnerTest {
    public static void main(String[] args) {
      Scanner scanner = new Scanner(System.in);

      // 주인의 정보를 입력받음
      System.out.print("PetOwner의 이름을 작성해 주세요: ");
      String name = scanner.nextLine();

      System.out.print("PetOwner의 나이를 작성해 주세요: ");
      int age = scanner.nextInt();

      System.out.println();

      // 입력받은 정보를 토대로 객체 생성
      PetOwner petOwner = new PetOwner(name, age);

      // while 반복문을 위한 변수
      int i = 0;

      while (i <= 1) {
        System.out.println("입양하고 싶은 동물의 번호를 선택하세요.");
        System.out.println("1번 고양이");
        System.out.println("2번 닭");
        System.out.println("3번 원숭이");

        int selectedAnimal = scanner.nextInt();

        // switch문: 입력받은 숫자에 따라 객체를 다르게 생성
        switch (selectedAnimal) {
          case 1:
            SecondCat sc = new SecondCat();
            petOwner.setAnimals(sc, i);
            break;
          case 2:
            Chicken c = new Chicken();
            petOwner.setAnimals(c, i);
            break;
          case 3:
            Monkey m = new Monkey();
            petOwner.setAnimals(m, i);
            break;
          default:
            break;
        }
        System.out.println();
        i++;
      }

      // 동물을 불러옴
      petOwner.animalsCall();

      // Animals 를 통해 공통으로 구현한 추상 메서드
      petOwner.animalsEat();
      petOwner.animalsWork();
      petOwner.animalsSleep();

      /* 제어문 사용 || 클래스 내부 메서드 수정해 최적화 필요
      // petOwner가 입양한 동물 중 고양이가 있는 경우 print문 출력
      petOwner.animalsTalk();

      // petOwner가 입양한 동물 중 원숭이가 있는 경우 print문 출력
      petOwner.animalsUse();
      petOwner.animalsLean();
      */
    }
  }
  ```

  <br />

  - 결과

    - 입력한 값: 1, 2

  > 입력한 값에 따라 결과가 달라짐

  ```
  PetOwner의 이름을 작성해 주세요: dobby
  PetOwner의 나이를 작성해 주세요: 13

  입양하고 싶은 동물의 번호를 선택하세요.
  1번 고양이
  2번 닭
  3번 원숭이
  1

  입양하고 싶은 동물의 번호를 선택하세요.
  1번 고양이
  2번 닭
  3번 원숭이
  2

  로아
  꼬꼬
  고양이가 사료을/를 먹어요
  닭이 사료을/를 먹어요
  꽁꽁 얼어붙은 한강 위로 고양이가 장난감 낚시대을/를 가지고 걸어다닙니다
  닭이 고개를 까딱이며걸어요
  고양이가 발라당 잔다
  닭이 앉아서 잔다
  ```
