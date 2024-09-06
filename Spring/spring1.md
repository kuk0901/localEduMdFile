# Spring 1일차

## **구축**

- Spring tool install

- mybatipse install

- tern install

- jsp -> eclipse enterprise java and web developer tools install

<br />

- 스프링(Spring)

  - EJB를 주 프레임워크로 사용할 때 불편했던 점들을 해소(스트럿츠)

<br />

## **spring에서의 객체 지향**

> 의존성 주입(dependency injection)

- 의존(dependency)

  - 하나의 객체에서 다른 객체를 사용하는 상태

  ```java
  // Score class
  package test1;

  public class Score {
    private int kor = 0;
    private int eng = 0;

    public Score() {
      super();
    }

    public Score(int kor, int eng) {
      this.kor = kor;
      this.eng = eng;
    }

    public int getKor() {
      return kor;
    }

    public void setKor(int kor) {
      this.kor = kor;
    }

    public int getEng() {
      return eng;
    }

    public void setEng(int eng) {
      this.eng = eng;
    }

    public void showScore() {
      System.out.println("국어: " + kor);
      System.out.println("영어: " + eng);
    }
  }
  ```

  ```java
  // Student class
  package test1;

  public class Student {
    String name = "";
    Score score;

    public Student() {
      super();
      this.score = new Score();
    }

    public void showStudent() {
      System.out.println("이름: " + name);
      score.showScore();
    }
  }
  ```

  ```java
  // main class
  package test1;

  public class Main {
    public static void main(String[] args) {
      Student st1 = new Student();
      Student st2 = new Student();

      st1.score.setKor(100);
      st1.score.setEng(100);
      st1.name = "javascript";
      st1.showStudent();


      st2.score.setKor(100);
      st2.score.setEng(100);
      st2.name = "typescript";
      st2.showStudent();
    }
  }
  ```

<br />

- 주입(injection)

  > Score class는 동일

  ```java
  // Student class
  package test2;

  public class Student {
    String name = "";
    Score score;

    // 생성자
    public Student(Score score) {
      super();
      this.score = score();
    }

    // setter
    public void setScore(Score score) {
      this.score = score;
    }

    public void showStudent() {
      System.out.println("이름: " + name);
      score.showScore();
    }
  }
  ```

  ```java
  // main class
  package test2;

  public class Main {
    public static void main(String[] args) {
      Score st1Score = new Score(100, 100);
      Student st1 = new Student(st1Score);
      st1.name = "Verna";
      st1.showStudent();

      Score st2Score = new Score(100, 100);
      Student st2 = new Student(st1Score);
      st2.name = "Gabriel";
      st2.showStudent();
    }
  }
  ```

<br />

- Spring framework

  - java 엔터프라이즈 개발을 편하게 해주는 오픈 소스 경량급 애플리케이션 프레임워크

  ```
  - 애플리케이션 프레임워크

    - 특정 계층이나, 기술, 업무 분야에 국한되지 않고 애플리케이션의 전 영역을
      포괄하는 범용적인 프레임워크


  - 경량급 프레임워크

    - 단순한 웹 컨테이너에서도 엔터프라이즈 개발의 고급 기술을 대부분 사용할 수 있음
  ```

  <br />

  - 엔터프라이즈 개발 용이

    - 개발자가 복잡하고 실수하기 쉬운 Low Level에 많이 신경 쓰지 않으면서 Business Logic 개발에 전념할 수 있도록 해 줌

  <br />

  - 오픈 소스

    - Spring은 OpenSource의 장점을 충분히 취하면서 동시에 OpenSource 제품의 단점과 한계를 잘 극복함

  <br />

  - 특징

    1. 컨테이너 역할

    - Spring container는 Java 객체의 Life Cycle을 관리하며, Spring container로부터 필요한 객체를 가져와 사용 가능

    <br />

    2. DI(Dependency Injection) 지원

    - Spring은 설정 파일이나 어노테이션을 통해서 객체 간의 의존관계를 설정할 수 있도록 함

    <br />

    3. AOP(Aspect Oriented Programming) 지원

    - Spring은 트랜잭션이나 로깅, 보안과 같이 공통적으로 필요로 하는 모듈들을 실제 핵심 모듈에서 분리해 적용할 수 있음

    <br />

    4. POJO(Plain Old Java Object) 지원

    - Spring container에 저장되는 Java 객체는 특정한 인터페이스를 구현하거나, 특정 클래스를 상속받지 않아도 됨

    <br />

    5. 트랜잭션 처리를 위한 일관된 방법 지원

    - JDBC, JPA 등 어떤 트랜잭션을 사용하던 설정을 통해 정보를 관리하므로 트랜잭션 구현에 상관없이 동일한 코드 사용 가능

    <br />

    6. 영속성(Persistence)과 관련된 다양한 API 지원

    - MyBatis, Hibernate 등 데이터베이스 처리를 위한 ORM(Objet Relational Mapping) 프레임워크들과의 연동을 지원

<br />

## **`Maven`**

> 스프링을 사용하기 위해 Maven Build가 필요함

- Maven

  - 자바 개발의 사실상 표준 빌드 틀

  - 이전에는 ANT를 많이 사용

  - XML 설정 파일을 사용

  - groovy라는 언어로 설정하는 gradle이 등장

<br />

- Convention

  - 일반적으로 개발자들이 따르는 일련의 규칙, 패턴, 관습을 의미

  - 스프링 프레임워크에서는 이러한 컨벤션들이 개발을 효율적으로 하고 코드의 일관성을 유지하는 데 중요한 역할을 함

  ```
  1. 이름 규칙: class, method, parameter 등의 명명 규칙

  2. 디렉토리 구조: controller, service, repository 등의 패키지

  3. 빈 설정과 관리 등
  ```

  <br />

  - 컨벤션의 중요성

    ```
    - 일관된 코드 스타일은 코드를 더 쉽게 이해하고 유지보수 할 수 있게 해 줌

    - 개발자들이 공통된 패턴을 따르게 되면, 팀 간 협업이 쉬워지고 개발 속도가 빨라짐

    - 표준화를 통해 학습 곡선이 완만해지고 새로운 기술이나 프레임워크를 도입할 때의 어려움이 줄어듦


    => 개발을 돕는 일종의 가이드라인 역할

    => 스프링은 위의 방법을 도입하고 있음
    ```

<br />

- 의존성 관리 자동으로 수행

  - Maven 중앙 저장소(Central Repository)를 제공해 자바 라이브러리에 대한 생태계 조성

<br />

- Pom.xml 메이븐의 메인 설정 파일

  - 프로젝트 루트 경로에 위치

  - 메이븐 프로젝트를 의미, IDE에서 불러오기 쉬움

<br />

- 메이븐 프로젝트 설정 시 필수사항

  ```
  1. 프로젝트명

  - artifact ID로 사용

  2. 그룹 아이디

  - 주로 프로젝트 생성 조직이나 기관의 도메인명 역순으로 표기

  - ex) kr.co.company, Top-level package 명으로 사용됨

  3. 버전

  - 개발 버전을 의미하는 SNAPSHOT 버전 사용

  - 배포 버전의 경우 Release
  ```

<br />

- Maven 설치

  - 로컬에 설치 필요

  - IDE에 포함된 경우 별도 설치 필요 없음
