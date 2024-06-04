# java

## java 11일차

- 인터페이스(Interface)

  - 몸통을 갖춘 일반 메서드, 멤버 변수를 구성원으로 가지는 일종의 추상 클래스

    > 추상 클래스보다 추상화 정도가 높음

  <br />

  - 멤버: **추상 메서드, 상수** => 그 외의 다른 요소 허용 X

    > 최근 자바 JDK 버전들은 몇 가지 추가 작성 가능

  <br />

  - 구현된 것은 아무것도 없고 밑그림만 그려져 있는 **기본 설계도**

  - 일반적으로 다른 클래스를 작성하는 데 도움을 줄 목적으로 작성됨

<br />

- 인터페이스 표현식

  ```
  interface 인터페이스명 {
    public static final 타입 상수명 = 값;
    public abstract 메서드명(매개변수 목록);
  }
  ```

  ```java
  // 예시
  interface A {
    public static final int MAX_NUMBER = 100;
    public abstract setName(String name);
  }
  ```

<br />

- 인터페이스 멤버의 제약사항

  - 모든 멤버 변수: public static final => 생략 가능

  - 모든 메서드: public abstract => 생략 가능

  - 단, static 메서드와 default 메서드는 예외(JDK 1.8 이상 버전)

<br />

- 인터페이스 상속

  - 인터페이스는 인터페이스로부터만 상속 가능

  - 다중 상속 지원

  - Object 클래스와 같은 최고 조상 없음

  - 키워드: extends

  ```java
  package Test.java.ch7Interface;

  public interface Video {
    void recordMovie();
  }
  ```

  ```java
  package Test.java.ch7Interface;

  public interface Mp3 {
    void sound();
  }
  ```

  ```java
  package Test.java.ch7Interface;

  public interface Tv extends Mp3, Video {
    public boolean POWER = false;
    public static final int CHANNEL = 0;

    public void power();
    public abstract void channelUp();
  }
  ```

<br />

- 인터페이스 구현

  - 상속 대신 구현이라는 용어 사용(구분을 위함)

  - 키워드: **implements**

  ```java
  package Test.java.ch7Interface;

  public class CaptionTv implements Tv {

    @Override
    public void power() {
  //    power = !power; // error;
      System.out.println(POWER);
    }

    @Override
    public void channelUp() {
  //    channelUp++; // error
      System.out.println(CHANNEL);
    }

    @Override
    public void sound() {
      System.out.println("소리가 남");
    }

    @Override
    public void recordMovie() {
      System.out.println("영상이 나옴");
    }
  }
  ```

<br />

- 인터페이스 장점

  - 개발 시간 단축

  - 표준화 가능

  - 서로 관계없는 클래스들에게 관계를 맺어줄 수 있음

  - 독립적 프로그래밍 가능(다형성)

<br />

- interface 실습

  - Animals interface 생성

  - cat, chicken, main, inter 패키지 생성

  - cat abstract class 생성

  <br />

  - Interface

  ```java
  // interface
  package Test.java.ch7.inter.exam.inter;

  public interface Animals {
    void eat(String bab);
    void work(String move);
    void sleep(String zzz);
  }
  ```

  <br />

  - Cat class (Abstract class) => Animals 구현한 추상 클래스

  ```java
  // Abstract class Cat
  package Test.java.ch7.inter.exam.cat;

  import Test.java.ch7.inter.exam.inter.Animals;

  public abstract class Cat implements Animals {
    abstract void call(); // 내가 기를 고양이의 이름

    void talk(String gr) {
      System.out.println("고양이는 " + gr + " 거려요");
    }

    public void metaData() {
      System.out.println("나는 고양이라고 부른다");
    }
  }
  ```

  <br />

  - FirstCat class

  ```java
  // FirstCat class
  package Test.java.ch7.inter.exam.cat;

  public class FirstCat extends Cat {

    @Override
    public void call() {
      System.out.println("리치");
    }

    @Override
    public void eat(String bab) {
      System.out.println("고양이가 " + bab + "을/를 먹어요");
    }

    @Override
    public void work(String move) {
      System.out.println("고양이가 " + move + "을/를 가지고 걸어요");
    }

    @Override
    public void sleep(String zzz) {
      System.out.println("고양이가 " + zzz + " 잔다");
    }

    @Override
    public String toString() {
      return "리치";
    }
  }
  ```

  <br />

  - SecondCat class

  ```java
  // SecondCat class
  package Test.java.ch7.inter.exam.cat;

  public class SecondCat extends Cat {

    @Override
    public void call() {
      System.out.println("로아");
    }

    @Override
    public void eat(String bab) {
      System.out.println("고양이가 " + bab + "을/를 먹어요");
    }

    @Override
    public void work(String move) {
      System.out.println("꽁꽁 얼어붙은 한강 위로 고양이가 " + move + "을/를 가지고 걸어 다닙니다");
    }

    @Override
    public void sleep(String zzz) {
      System.out.println("고양이가 " + zzz + " 잔다");
    }

    @Override
    public void talk(String gr) {
      System.out.println("고양이는 " + gr + " 거려요");
    }

    @Override
    public String toString() {
      return "로아";
    }
  }
  ```

  <br />

  - chicken class => Animals 구현

  ```java
  package Test.java.ch7.inter.exam.chicken;

  import Test.java.ch7.inter.exam.inter.Animals;

  public class Chicken implements Animals {
    @Override
    public void eat(String bab) {
      System.out.println("닭이 " + bab + "을/를 먹어요");
    }

    @Override
    public void work(String move) {
      System.out.println("닭이 " + move + "걸어요");
    }

    @Override
    public void sleep(String zzz) {
      System.out.println("닭이 " + zzz + " 잔다");
    }

    public void call() {
      System.out.println("꼬꼬");
    }

    @Override
    public String toString() {
      return "꼬꼬";
    }
  }
  ```

  <br />

  - MyPetRaisingTest class (main class)

  ```java
  package Test.java.ch7.inter.exam.main;

  import Test.java.ch7.inter.exam.cat.FirstCat;
  import Test.java.ch7.inter.exam.cat.SecondCat;
  import Test.java.ch7.inter.exam.chicken.Chicken;

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
    }
  }
  ```

<br />

- String

  - 문자열은 객체 데이터 타입

  ```java
  String str1 = "문자들";
  String str2 = "문자들";

  String str3 = new String("문자들");
  String str4 = new String("문자들");

  System.out.println("str1 == str2 ? " + (str1 == str2));
  System.out.println("str1 == str3 ? " + (str1 == str3));
  System.out.println("str1 == str4 ? " + (str1 == str4));

  System.out.println("str1.equals("문자들") ?" + str1.equals("문자들"))
  ```

  <br />

- String method

  | String 클래스의 메서드                                                                                                                                                                                                                                             | 설명                                                                                                  | 예제                                                                                                                                                                                                                                                                         | 결과                                                                                           |
  | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
  | String(String s)                                                                                                                                                                                                                                                   | 주어진 문자열(s)를 갖는 String 인스턴스 생성                                                          | String s = new String("hello");                                                                                                                                                                                                                                              | s = "Hello"                                                                                    |
  | String(char[] value)                                                                                                                                                                                                                                               | 주어진 문자열(value)를 갖는 String 인스턴스를 생성                                                    | char[] c = {'H', 'e', 'l', 'l', 'o'}; <br /> String s = new String(c);                                                                                                                                                                                                       | s = "Hello"                                                                                    |
  | String(StringBuffer buf)                                                                                                                                                                                                                                           | StringBuffer 인스턴스가 갖고 있는 문자열과 같은 내용의 String 인스턴스를 생성                         | StringBuffer sb = new StringBuffer("Hello); <br /> String s = new String(sb);                                                                                                                                                                                                | s = "Hello"                                                                                    |
  | char charAt(int index)                                                                                                                                                                                                                                             | 저장된 위치(index)에 있는 문자를 알려줌                                                               | String s = "hello"; <br /> char c = s.charAt(1);                                                                                                                                                                                                                             | c = 'e'                                                                                        |
  | int compareTo(String str)                                                                                                                                                                                                                                          | 문자열(str)과 사전 순서로 비교 <br /> (같으면 0, 이전이면 음수, 이후면 양수)                          | int i1 = "aaa".compareTo("aaa"); <br /> int i2 = "aaa".compareTo("bbb"); <br /> int i3 = "bbb".compareTo("aaa");                                                                                                                                                             | i1 = 0 <br /> i2 = -1 <br /> i3 = 1                                                            |
  | String concat(String str)                                                                                                                                                                                                                                          | 문자열(str)을 뒤에 덧붙임                                                                             | String s = "Hello"; <br /> String s2 = s.concat(" World");                                                                                                                                                                                                                   | s2 = "Hello World"                                                                             |
  | boolean contains(CharSequence s)                                                                                                                                                                                                                                   | 지정된 문자열(s)이 포함되었는지 검사                                                                  | String s = "Hello"; <br /> boolean b = s.contains("He");                                                                                                                                                                                                                     | b = true                                                                                       |
  | boolean endsWith(String suffix)                                                                                                                                                                                                                                    | 지정된 문자열(suffix)로 끝나는지 검사                                                                 | String s = "Hello"; <br /> boolean b = s.endsWith("llo");                                                                                                                                                                                                                    | b = true                                                                                       |
  | boolean equals(Object obj)                                                                                                                                                                                                                                         | 문자열(obj)과 String 인스턴스의 문자열을 비교 <br /> (String이 아니거나 문자열이 다르면 false를 반환) | String s = "Hello"; <br /> boolean b1 = s.equals("Hello"); <br /> boolean b2 = s.equals("hello");                                                                                                                                                                            | b1 = true <br /> b2 = false                                                                    |
  | boolean equalsIgnoreCase(String str)                                                                                                                                                                                                                               | 문자열과 String 인스턴스의 문자열을 대소문자 구분없이 비교                                            | String s = "Hello"; <br /> boolean b1 = s.equalsIgnoreCase("Hello"); <br /> boolean b2 = s.equalsIgnoreCase("hello");                                                                                                                                                        | b1 = true <br /> b2 = true                                                                     |
  | int indexOf(char ch)                                                                                                                                                                                                                                               | 주어진 문자(ch)가 문자열에 존재하는지 확인하며, 위치(index)를 알려줌 <br /> (없는 경우, -1 반환)      | String s = "Hello"; <br /> int idx1 = s.indexOf('o'); <br /> int idx2 = s.indexOf('k');                                                                                                                                                                                      | idx1 = 4 <br /> idx2 = -1                                                                      |
  | int indexOf(char ch, int pos)                                                                                                                                                                                                                                      | 주어진 문자(ch)가 문자열에 존재하는지 지정한 위치(pos)부터 확인하여 위치(index)를 알려줌              | String s = "Hello"; <br /> int idx1 = s.indexOf('e', 0); <br /> int idx2 = s.indexOf('e', 2);                                                                                                                                                                                | idx1 = 1 <br /> idx2 = -1                                                                      |
  | int indexOf(String str)                                                                                                                                                                                                                                            | 주어진 문자열(str)이 존재하는지 확인하여 위치(index)를 알려줌                                         | String s = "Hello"; <br /> int idx = s.indexOf("He");                                                                                                                                                                                                                        | idx = 0                                                                                        |
  | String intern()                                                                                                                                                                                                                                                    | 문자열을 상수풀(constant pool)에 등록 <br /> 상수풀에 같은 문자열이 있을 경우, 주소값을 반환          | String s1 = new String("abc"); <br /> String s2 = new String("abc"); <br /> boolean b1 = (s1 == s2); <br /> boolean b2 = s1.equals(s1); <br /> boolean b3 = (s1.intern() == s2.intern());                                                                                    | b1 = false <br /> b2 = true <br /> b3 = true                                                   |
  | int lastIndexOf(char ch)                                                                                                                                                                                                                                           | 지정된 문자 또는 문자 코드를 문자열의 오른쪽 끝에서부터 찾아서 위치(index)를 알려줌                   | String s = "java.lang.java"; <br /> int idx1= s.lastIndexOf('.'); <br /> int idx2 = s.indexOf('.');                                                                                                                                                                          | idx1 = 9 <br /> idx2 = 4                                                                       |
  | int lastIndexOf(String str)                                                                                                                                                                                                                                        | 지정된 문자열을 인스턴스의 문자열 끝에서부터 찾아서 위치(index)를 알려줌                              | String s = "java.lang.java"; <br /> int idx1 = s.lastIndexOf("java") <br /> int idx2 = s.indexOf("java");                                                                                                                                                                    | idx1 = 10 <br /> idx2 = 0                                                                      |
  | int length()                                                                                                                                                                                                                                                       | 문자열의 길이를 알려줌                                                                                | String s = "Hello"; <br /> int length = s.length();                                                                                                                                                                                                                          | length = 5;                                                                                    |
  | String replace(char old, char nw)                                                                                                                                                                                                                                  | 문자열 중의 문자(old)를 새로운 문자(nw)로 바꾼 문자열을 반환                                          | String s1 = "Hello"; <br /> String s2 = s.replace('H', 'C');                                                                                                                                                                                                                 | s2 = "Cello"                                                                                   |
  | String replace(CharSequence old, CharSequence nw)                                                                                                                                                                                                                  | 문자열 중의 문자(old)를 새로운 문자(nw)로 모두 바꾼 문자열을 반환                                     | String s1 = "Hellollo"; <br /> String s2 = s.replace("ll", "LL");                                                                                                                                                                                                            | s2 = "HeLLoLLo"                                                                                |
  | String replaceAll(String regex, String replacement)                                                                                                                                                                                                                | 문자열 중에서 지정된 문자열(regex)과 일치하는 것을 새로운 문자열(replacement)로 모두 변경             | String s = "AABBAABB"; <br /> String r = s.replaceAll("BB", "bb");                                                                                                                                                                                                           | r = "AAbbAAbb"                                                                                 |
  | String replaceFirst(String regex, String replacement)                                                                                                                                                                                                              | 문자열 중에서 지정된 문자열(regex)과 일치하는 것 중, 첫 번째 것만 새로운 문자열(replacement)로 변경   | String s = "AABBAABB"; <br /> String r = s.replaceFirst("BB", "bb");                                                                                                                                                                                                         | r = "AAbbAABB"                                                                                 |
  | String[] split(String regex)                                                                                                                                                                                                                                       | 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환                                        | String animals = "dog, cat, bear"; <br /> String[] arr = animals.split(",");                                                                                                                                                                                                 | arr[0] = "dog" <br /> arr[1] = "cat" <br /> arr[2] = "bear"                                    |
  | String[] split(String regex, int limit)                                                                                                                                                                                                                            | 문자열을 지정된 수(limit)에서 지정된 분리자(regex)로 잘라 문자열 배열에 담아 반환                     | String animals = "dog, cat, bear"; <br /> String[] arr = animals.split(",", 2);                                                                                                                                                                                              | arr[0] = "dog" <br /> arr[1] = "cat, bear"                                                     |
  | boolean startsWith(String prefix)                                                                                                                                                                                                                                  | 주어진 문자열(prefix)로 시작하는지 검사                                                               | String s = "java.lang.java"; <br /> String c = s.startsWith("java"); <br /> String p = s.startsWith("lang");                                                                                                                                                                 | c = "java" <br /> p = "lang"                                                                   |
  | String substring(int begin) <br /> String substring(int begin, int end)                                                                                                                                                                                            | 주어진 시작 위치(begin)부터 끝 위치(end) 범위에 포함된 문자열을 얻음 <br /> (begin <= x < end)        | String s = "java.lang.java"; <br /> String c = s.substring(10); <br /> String p = s.substring(5, 9);                                                                                                                                                                         | c = "java" <br /> p = "lang"                                                                   |
  | String toLowerCase()                                                                                                                                                                                                                                               | String 인스턴스에 저장되어 있는 모든 문자열을 소문자로 변환하여 반환                                  | String s = "Hello"; <br /> String s2 = s1.toLowerCase();                                                                                                                                                                                                                     | s2 = "hello"                                                                                   |
  | String toString()                                                                                                                                                                                                                                                  | String 인스턴스에 저장되어 있는 문자열을 반환                                                         | String s = "Hello"; <br /> String s2 = s.toString();                                                                                                                                                                                                                         | s2 = "Hello"                                                                                   |
  | String toUpperCase()                                                                                                                                                                                                                                               | String 인스턴스에 저장되어 있는 모든 문자열을 대문자로 변환하여 반환                                  | String s = "Hello"; <br /> String s2 = s.toUpperCase();                                                                                                                                                                                                                      | s2 = "HELLO"                                                                                   |
  | String trim()                                                                                                                                                                                                                                                      | 문자열의 양쪽 끝에 있는 공백을 없앤 결과를 반환                                                       | String s = " Hello World "; <br /> String s2 = s.trim();                                                                                                                                                                                                                     | s2 = "Hello World"                                                                             |
  | static String valueOf(boolean b) <br /> static String valueOf(char c) <br /> static String valueOf(int i) <br /> static String valueOf(long l) <br /> static String valueOf(float f) <br /> static String valueOf(double d) <br /> static String valueOf(Object o) | 지정된 값을 문자열로 변환하여 반환                                                                    | String b = String.valueOf(true); <br /> String c = String.valueOf('a'); <br /> String i = String.valueOf(100); <br /> String l = String.valueOf(100); <br /> String l = String.valueOf(100L); <br /> String f = String.valueOf(10F); <br /> String d = String.valueOf(10.0); | b = "true <br /> c = "a" <br /> i = "100" <br /> l = "100" <br /> f = "10.0" <br /> d = "10.0" |

<br />
