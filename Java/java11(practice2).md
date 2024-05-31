# 과제2

- 2024/05/31

1. abstractExam 프로젝트 문제 풀어보기
2. 로또 생성기 만들어 보기

## abstractExam

- ContentSender

  - abstract class

  ```java
  package ch7.abstractExam;

  /**
   * @author user
   *
   */
  public abstract class ContentSender {
    String title;  // 제목
    String name;   // 보낸이
    String content = "";  // 내용
    String recipient = ""; // 받는 사람



    public ContentSender() {
      super();
    }


    public ContentSender(String title, String name, String content, String recipient) {
      super();
      this.title = title;
      this.name = name;
      this.content = content;
      this.recipient = recipient;
    }


    /**
     * @param sendMethod 보내는 방법 ex) 카카오톡 or Line or 네이버메일 or 다음메일 ...
     */
    abstract void sendMsg(String sendMethod);

  }
  ```

  <br />

  - MailSender

    - 추상 클래스를 상속

  ```java
  package ch7.abstractExam;

  public class MailSender extends ContentSender {


    public MailSender(String title, String name, String content, String recipient) {
      super(title, name, content, recipient);

    }

    // 아래의 내용이 출력되도록 구현하시오
    /*
    *	언제 어디서나 쉽고 빠르게!
      ====== ?????? ?? ======
      제목 = 짝에게 보낼 제목
      이름 = 자신의 이름
      내용 = 짝에게 전달한 나의 마음
      받는사람 = 짝의 이름
      ====== ?????? ?? ======
    *
    * */
    public void sendMsg(String sendMethod) {
      System.out.println("언제 어디서나 쉽고 빠르게!");
      System.out.println("===== " + sendMethod + " =====");
      System.out.println("제목 = " + title);
      System.out.println("이름 = " + name);
      System.out.println("내용 = " + content);
      System.out.println("받는사람 = " + recipient);
      System.out.println("===== " + sendMethod + " =====");
    }

  }
  ```

  <br />

  - MobileMessengerSender

    - 추상 클래스를 상속

  ```java
  package ch7.abstractExam;

  public class MobileMessengerSender extends ContentSender{

    public MobileMessengerSender(String title, String name, String content, String recipient) {
      super(title, name, content, recipient);
    }

    // 아래의 내용이 출력되도록 구현하시오
    /*
    * 	전세계 어디서나 친구와 1:1 또는 그룹채팅을 무료로!!
      ====== ???? ======
      제목 = 짝에게 보낼 제목
      이름 = 자신의 이름
      내용 = 짝에게 전달한 나의 마음
      받는사람 = 짝의 이름
      ====== ???? ======
    *
    *
    *
    * */
    public void sendMsg(String sendMethod) {
      System.out.println("전세계 어디서나 친구와 1:1 또는 그룹채팅을 무료로!!");
      System.out.println("===== " + sendMethod + " =====");
      System.out.println("제목 = " + title);
      System.out.println("이름 = " + name);
      System.out.println("내용 = " + content);
      System.out.println("받는사람 = " + recipient);
      System.out.println("===== " + sendMethod + " =====");
    }

  }
  ```

  <br />

  - MailSenderTest(main class)

    - 메인 클래스 실행 후 결과 확인

  ```java
  package ch7.abstractExam;

  public class MailSenderTest {
    public static void main(String[] args) {
      MailSender ms = new MailSender(
          "hello", "dobby", "hello everyone", "m"
      );

      ms.sendMsg("다음 메일");

      System.out.println();

      MobileMessengerSender mms = new MobileMessengerSender(
          "hello", "dobby", "hello everyone", "m"
      );
      mms.sendMsg("카카오톡");
    }
  }
  ```

  <br />

  - 결과

  ```
  언제 어디서나 쉽고 빠르게!
  ===== 다음 메일 =====
  제목 = hello
  이름 = dobby
  내용 = hello everyone
  받는사람 = m
  ===== 다음 메일 =====

  전세계 어디서나 친구와 1:1 또는 그룹채팅을 무료로!!
  ===== 카카오톡 =====
  제목 = hello
  이름 = dobby
  내용 = hello everyone
  받는사람 = m
  ===== 카카오톡 =====
  ```

<br />

## 로또 번호 생성기

- 로또 번호 생성기

  ```
  Q. 외부에서 내부 state를 확인할 필요가 있는가
    - 로또 번호를 얻기만 하면 됨

  Q. 외부에서 setter를 호출할 필요가 있는가
    - 번호 뽑기만 누르면 자동으로 랜덤 숫자 7개를 알려줌

  => 내부 멤버 변수를 외부에서 사용할 필요 없음
  => 로또 번호를 얻는 메서드 이외의 메서드르를 외부에서 사용할 필요 없음
  => getter를 제외한 멤버 private
  ```

  <br />

  - 7개의 숫자 필요: 숫자 6개와 보너스 숫자 1개

  - 6개의 int를 담을 int[] lottoNums

  - 1개의 int를 담을 int bonusNum

  - setter: 외부에서 사용할 필요 없다 판단

  - getter: setter 호출 후 setter 결과 출력

  ```java
  public class Lotto {
    private int[] lottoNums = new int[6];
    private int bonusNum;

    // lotto number setter
    private void setLottoNums() {
      int count = 0;

      while (count < 6) {
        int num = (int) (Math.random() * 45) + 1;
        boolean isDuplicate = false;

        // 중복 확인
        for (int j = 0; j < count; j++) {
          if (lottoNums[j] == num) {
            isDuplicate = true;
            break;
          }
        }

        if (!isDuplicate) {
          lottoNums[count] = num;
          count++;
        }
      }
    }

    public void getLottoNums() {
      setLottoNums();

      System.out.print("로또 번호: ");

      for (int i = 0; i < 6; i++) {
        System.out.print(lottoNums[i] + " ");
      }
    }

    private void setBonusNum() {
      int count = 0;

      while (count < 6) {
        int num = (int) (Math.random() * 45) + 1;
        boolean isDuplicate = false;

        // 중복 확인
        for (int j = 0; j < count; j++) {
          if (lottoNums[j] == num) {
            isDuplicate = true;
            break;
          }
        }

        if (!isDuplicate) {
          bonusNum = num;
          count++;
        }
      }

    }

    // bonus number setter
    public void getBonusNum() {
      setBonusNum();

      System.out.println("보너스 번호: " + bonusNum);
    }
  }
  ```

  <br />

  - LottoTest(main class)

  ```java
  public class LottoTest {
    public static void main(String[] args) {
      Lotto lotto = new Lotto();

      lotto.getLottoNums();
      lotto.getBonusNum();
    }
  }
  ```

  <br />

  - 결과

  ```
  로또 번호: 7 8 24 32 12 41 보너스 번호: 25
  ```

  <br />

  - 의문점

  ```
  - lottoNums과 bonusNum의 setter, getter를 나눌 필요가 있는가

  => setter를 합치게 되면 하나의 메서드가 너무 무거워짐
  => 멤버 변수 두 가지를 한 번에 출력하는 메서드는 헤비한가
  ```

  <br />

  - Lotto class의 getLottoNums와 getBonusNum를 하나로 합침

    - setter의 위치를 붙여놓음

  ```java
  public class Lotto {
    private int[] lottoNums = new int[6];
    private int bonusNum;

    // lotto number setter
    private void setLottoNums() {
      int count = 0;

      while (count < 6) {
        int num = (int) (Math.random() * 45) + 1;
        boolean isDuplicate = false;

        // 중복 확인
        for (int j = 0; j < count; j++) {
          if (lottoNums[j] == num) {
            isDuplicate = true;
            break;
          }
        }

        if (!isDuplicate) {
          lottoNums[count] = num;
          count++;
        }
      }
    }

    private void setBonusNum() {
      int count = 0;

      while (count < 6) {
        int num = (int) (Math.random() * 45) + 1;
        boolean isDuplicate = false;

        // 중복 확인
        for (int j = 0; j < count; j++) {
          if (lottoNums[j] == num) {
            isDuplicate = true;
            break;
          }
        }

        if (!isDuplicate) {
          bonusNum = num;
          count++;
        }
      }

    }

    public void getLottoNumber() {
      setLottoNums();
      setBonusNum();

      System.out.print("로또 번호: ");

      for (int i = 0; i < 6; i++) {
        System.out.print(lottoNums[i] + " ");
      }

      System.out.println("보너스 번호: " + bonusNum);
    }

  }
  ```

  <br />

  - LottoTest(main class) 수정

  ```java
  public class LottoTest {
    public static void main(String[] args) {
      Lotto lotto = new Lotto();

      lotto.getLottoNumber();
    }
  }
  ```

  <br />

  - 결과

  ```
  로또 번호: 10 27 20 29 36 2 보너스 번호: 3
  ```

  <br />

  - 의문점 해결

  ```
  - getter 수정한 결과, 소스 코드가 한 줄 늘어남의 차이인 것을 확장프로그램 통해 확인함

  => 메서드를 나눈 것과 소스 코드 양이 일치
  => getter 하나를 사용하는 것이 더 효율적임을 확인(외부에서 getter 호출 횟수가 줄어듦)
  ```
