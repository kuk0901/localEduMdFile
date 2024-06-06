# 13일차

## java 13일차

- Collection Framework

  - 정의: 데이터군을 저장하는 클래스들을 표준화한 설계

  - 컬렉션(Collection): 다수의 데이터, 즉 데이터 그룹

  - 프레임워크(Framework): 표준화된 프로그래밍 방식

<br />

- 컬렉션의 핵심 인터페이스

  - 컬렉션(데이터 그룹)을 크게 3가지 타입이 존재한다고 인식하고 각 컬렉션을 다루는 데 필요한 기능을 가진 3개의 인터페이스를 정의해 놓음

  - 인터페이스 List와 Set을 구현한 컬렉션 클래스들은 서로 많은 공통부분이 있어서 공통된 부분을 다시 뽑아 Collection 인터페이스를 정의

  - Map 인터페이스는 이들과는 전혀 다른 형태로 컬렉션을 다루기 때문에 같은 상속 계층도에 포함되지 못함

<br />

- 상속 계층도

  ```
  List extends Collection

  Set extends Collection

  Map
  ```

<br />

- 인터페이스

  | 이름 | 설명                                                                                                                     | 예시                           | 구현 클래스                           |
  | ---- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------ | ------------------------------------- |
  | List | 순서가 있는 데이터의 집합, 데이터의 중복 허용                                                                            | 대기자 명단                    | **ArrayList**, Stack 등               |
  | Set  | 순서를 유지하지 않는 데이터의 집합, 중복 불허용                                                                          | 양의 정수 집합, 소수의 집합    | HashSet, TreeSet 등                   |
  | Map  | 키(key)와 값(value)의 쌍(pair)으로 이루어진 데이터의 집합 <br /> 순서 유지 X <br /> 키 중복 불허용 <br /> 값은 중복 허용 | 우편 번호, 지역 번호(전화번호) | **HashMap**, HashTable, Properties 등 |

<br />

- ArrayList

  ```java
  List list1 = new ArrayList(5);

  list1.add(1);
  list1.add(new Integer(2));
  list1.add(3);
  list1.add(4);
  list1.add(new Integer(5));

  int n = (int)list1.get(0);
  System.out.println(n);

  System.out.println(list1.get(1));
  System.out.println(list1.get(2));
  System.out.println(list1.get(3));
  System.out.println(list1.get(4));
  ```

<br />

- 제너릭스 or 지네릭스(Generics)

  - 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해주는 기능

  - 객체는 타입을 컴파일 시에 체크, 객체의 타입 안정성을 높이고 형 변환의 번거로움이 줄어듦

  - 사용 방법: <>(앵글 브래킷)

  ```java
  List<Integer> list1 = new ArrayList<Integer>(5);
  ```

  <br />

  - 제네릭 장점

    - 간결한 코드

<br />

- ArrayList 실습 1

  - 1~10까지 데이터 저장 및 순차적으로 출력 후 삭제

  ```java
  List<Integer> numList = new ArrayList<>();

  for (int i = 0; i < 10; i++) {
    numList.add(i + 1);
  }

  for (int num : numList) {
    System.out.println(num);
  }

  for (; numList.size() > 0;) {
    numList.remove(0);
  }

  System.out.println("리스트의 사이즈: " + numList.size());
  ```

  > **입력과 출력의 기능 분리**를 통해 하나의 기능만 함 => 재사용성이 높아짐

<br />

- ArrayList 실습 2

  - 리스트에 담긴 값: 100, 30, 50, 70, 40, 90

  - 총점과 평균을 구함

  - 평균은 소수점 3번째 자리에서 올림 처리

  ```java
  public class TestArrayList3 {
    public static void main(String[] args) {
      ArrayList<Integer> scoreList = new ArrayList<>();
      int totalScore = 0;
      double avg;

      scoreList.add(100);
      scoreList.add(30);
      scoreList.add(50);
      scoreList.add(70);
      scoreList.add(40);
      scoreList.add(90);

      for (int score: scoreList) {
        totalScore += score;
      }

      avg = Math.ceil(((double)totalScore / scoreList.size()) * 100) / 100.0;

      for (int i = 0; i < scoreList.size(); i++) {
        System.out.println((i + 1) + "번째 값: " + scoreList.get(i));
      }

      System.out.println("총점: " + totalScore);
      System.out.println("평균: " + avg);
    }
  }
  ```

<br />

- 확장된 for문(forEach문)

  - 일반적인 for문과는 다르게 출력용으로 사용

  ```java
  ArrayList<Integer> numList = new ArrayList<>();

  for (int i = 0; i < 5; i++) {
    numList.add(i);
  }

  for (int num : numList) {
    System.out.println(num);
  }
  ```

<br />

- Iterator

  - 컬렉션 프레임워크에서 컬렉션에 저장된 요소들을 **읽어오는 방법을 표준화**함

  - 컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator 인터페이스를 정의하고, Collection 인터페이스에는 <u>Iterator를 구현한 클래스의 인스턴스를 반환</u>하는 **iterator()** 를 정의하고 있음

  - Iterator를 얻은 다음 반복문 주로, **while문을 사용**해서 컬렉션 클래스의 요소들을 읽어올 수 있음

  ```java
  ArrayList<Integer> numList = new ArrayList<>();

  for (int i = 0; i < 5; i++) {
    numList.add(i);
  }

  Iterator it = numList.iterator();

  Object obj = null;

  while(it.hasNext()) {
    obj = it.next();

    System.out.println((int) obj);
  }
  ```

<br />

- Map

  - HashMap

    - 생성, 추가, 조회

    - put(key, value) : 생성, 추가

    - get(key) : key에 해당하는 value 반환

  ```java
  Map<String, Integer> humanMap = new HashMap<>();

  humanMap.put("Kim", 1);
  humanMap.put("Lee", 1);
  humanMap.put("Choi", 4);
  humanMap.put("Park", 5);
  humanMap.put("Roh", 7);

  int humanNumber = humanMap.get("Kim");

  System.out.println(humanNumber);

  System.out.println(humanMap.get("Lee"));
  System.out.println(humanMap.get("Park"));
  ```

<br />

- Map 실습

  - 4명의 이름과 별명을 작성하고 등록 후 별명 출력

    - 키: 이름

    - 값: 별명

  ```java
  Map<String, String> academy = new HashMap<>();

  academy.put("a", "hello");
  academy.put("b", "hi");
  academy.put("c", "youb");
  academy.put("dd", "nooo");

  for (String name: academy.keySet()) {
    System.out.println(name + "의 별명 " + academy.get(name));
  }
  ```
