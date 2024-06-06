# 14일차

## java 14일차

- HashMap 수정, 삭제

  - put(key, 수정할 value) : 수정

  - remove(key) : key와 value 삭제

  > 입력과 수정의 메서드 동일: put

  ```java
  package Test.java.map;

  import java.util.HashMap;
  import java.util.Map;

  public class TestHashMap {
    public static void main(String[] args) {
      Map<String, Integer> academy = new HashMap<>();

      academy.put("a", 1);
      academy.put("b", 2);
      academy.put("c", 3);
      academy.put("dd", 4);

      System.out.println("현재 정보");
      System.out.println(academy);

      academy.put("b", 17);

      System.out.println("변경된 현재 정보");
      System.out.println(academy);

      academy.remove("b");

      System.out.println("지운 후 현재 정보");
      System.out.println(academy);
    }
  }
  ```

<br />

- HashMap을 이용해 id, password 일치 확인 프로그램

  ```java
  package Test.java.map;

  import java.util.HashMap;
  import java.util.Map;
  import java.util.Scanner;

  public class TestHashMap {
    public static void main(String[] args) {
      Map<String, String> userInfoMap = new HashMap<>();

      userInfoMap.put("myId", "1234");
      userInfoMap.put("log", "qwer");
      userInfoMap.put("dog", "asdf");

      Scanner scanner = new Scanner(System.in);

      while (true) {
        System.out.println("id와 password를 입력");
        System.out.print("id: ");
        String idStr = scanner.nextLine().trim();

        System.out.println("pwd: ");
        String pwd = scanner.nextLine().trim();
        System.out.println();

        if (userInfoMap.containsKey(idStr) == false) {
          System.out.println("입력하신 id는 존재하지 않습니다.");
          System.out.println("다시 입력해 주세요.\n");

          continue;
        } else {
          if (userInfoMap.get(idStr).equals(pwd) == false) {
            System.out.println("비밀번호가 일치하지 않습니다.");
            System.out.println("다시 입력해 주세요.\n");
          } else {
            System.out.println("id와 비밀번호가 일치합니다.");
            System.out.println("로그인 되었습니다.");

            break;
          }
        }
      }

    }
  }
  ```

<br />

- 호텔 프로그램 실습

  - 층, 객실, 손님이 정해져 있는 경우

  - 층과 객실, 손님을 나눠서 출력

    ex) 1층 101호 ㅇㅇㅇ 102호 ㅁㅁㅁ

  <br />

  - HotelProgramUtil class (util class)

  ```java
  public class HotelProgramUtil {
    public void printHotelRoomNumAnsGuestName(HashMap<Integer, ArrayList<String>> hotelInfoMap) {
      for(int i = 0; i < hotelInfoMap.size(); i++) {
        int groupNum = i + 1;

        List<String> studyGroupList = hotelInfoMap.get(groupNum);

        System.out.println("====\t" + groupNum + "층\t====");

        int number = 0;

        for (int n = 0; n < studyGroupList.size(); n++) {

          number = n + 1;
          System.out.println(groupNum + "0" + number + "호: " + studyGroupList.get(n));
        }
      }
    }
  }
  ```

  <br />

  - HotelProgramTest class (main class)

  ```java
  public class HotelProgramTest {

    public static void main(String[] args) {

      HashMap<Integer, ArrayList<String>> hotelRoomNumAndGuestName = new HashMap<>();

      ArrayList<String> hotelFirstFloor = new ArrayList<>();

      hotelFirstFloor.add("kim1");
      hotelFirstFloor.add("Lee1");

      hotelRoomNumAndGuestName.put(1, hotelFirstFloor);

      ArrayList<String> hotelSecondFloor = new ArrayList<>();

      hotelSecondFloor.add("park1");
      hotelSecondFloor.add("Lee2");

      hotelRoomNumAndGuestName.put(2, hotelSecondFloor);

      ArrayList<String> hotelThirdFloor = new ArrayList<>();

      hotelThirdFloor.add("kim2");
      hotelThirdFloor.add("park2");

      hotelRoomNumAndGuestName.put(3, hotelThirdFloor);

      HotelProgramUtil hotelProgramUtil = new HotelProgramUtil();

      hotelProgramUtil.printHotelRoomNumAnsGuestName(hotelRoomNumAndGuestName);
    }
  }
  ```

<br />

<br />

- 날짜와 시간(Calendar, Date)

  - Date

  - 날짜와 시간을 다룰 목적으로 제공되는 클래스

  - 1995년 즈음에 만들어져 문제가 많지만 아직도 사용되고 있음

  - 이후에 만들어진 것이 Calendar 인터페이스

  > Calendar 이후 만들어진 것이 java.time 패키지

<br />

- 날짜 형식

  - SimpleDateFormat

  - 날짜 패턴 기호

  | 기호           | 의미                                     | 출력 결과                      |
  | -------------- | ---------------------------------------- | ------------------------------ |
  | y              | 년도                                     | 2024                           |
  | Y              | 년도                                     | 2024(일반적으로 소문자 사용함) |
  | M              | 월(1\~12 또는 1월\~12월)                 | 10 또는 10월, OCT              |
  | d              | 월의 몇 번째 일(1~31)                    | 5                              |
  | E              | 요일                                     | 수                             |
  | a              | 오전/오후(AM, PM)                        | PM                             |
  | H              | 시간(0~23)                               | 20                             |
  | h              | 시간(1~12)                               | 11                             |
  | m              | 분(0~59)                                 | 35                             |
  | s              | 초(0~59)                                 | 55                             |
  | '(싱글 따옴표) | escape 문자(특수 문자를 표현하는데 사용) | 없음                           |

  <br />

  ```java
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 E요일 H시 m분 s초");

  String todayStr = sdf.format(now);

  System.out.println(todayStr);
  ```

  <br />

- Date 실습

  - "2024년 06월 05일 수요일 10시 50분 49초" 형태로 출력

  - TestDateUtil 클래스에 메서드를 만들어 현재 날짜 출력

  - 요일을 한글로 구현하는 메서드

  - 현재 날짜를 문자열로 반환하는 메서드 구현

  - main class에서 현재 날짜 출력

  ```java
  public class TestDateUtil {

    public String getDayKoreaVersion() {
      date date = new Date();

      int dayNum = date.getDay();

      switch (dayNum) {
        case 0: {
          return "일";
        }
        case 1: {
          return "월";
        }
        case 2: {
          return "화";
        }
        case 3: {
          return "";
        }
        case 4: {
          return "화";
        }
        case 5: {
          return "화";
        }
        case 6: {
          return "화";
        }
        default: {
          return "해당 요일 없음";
        }
      }
    }

    public String getTodayDate() {
      Date now = new Date();

      String today;

      int year = now.getYear() + 1900;
      String month = this.getStringVersionDate(now.getMonth() + 1);
      String date = this.getStringVersionDate(now.getDate());

      String day = this.getDayKoreaVersion();

      int hours = now.getHours();
      int minutes = now.getMinutes();
      int seconds = new.getSeconds();

      today = year + "년 " + month + "월 " + date + "일 " + day + "요일 "
              + hours + "시 " + minutes + "분 " + seconds "초";

      return today;
    }

    public String getStringVersionDate(int num) {

      String stringVersionDate = "0";

      if (num < 10) {

        stringVersionDate += num;
        return stringVersionDate;

      } else {
        return num + "";
      }
    }
  }
  ```

  <br />

  - TestDateMain

  ```java
  public static void main(String[] args) {

    TestDateUtil dateUtils = new TestDateUtil();

    System.out.println(dateUtils.getTodayDate());
  }
  ```

<br />

- Date set

  ```java
  Date myChoiceDate = new Date();

  myChoiceDate.setYear(2024);
  myChoiceDate.setMonth(7);
  myChoiceDate.setDate(16);
  myChoiceDate.setHours(8);
  myChoiceDate.setMinutes(30);
  myChoiceDate.setSeconds(10);

  System.out.println(myChoiceDate);
  ```

  > 출력 시 년도 부분 논리적 에러

  ```java
  Date myChoiceDate = new Date();

  int year = 2024 - 1900;

  myChoiceDate.setYear(year);
  myChoiceDate.setMonth(7);
  myChoiceDate.setDate(16);
  myChoiceDate.setHours(8);
  myChoiceDate.setMinutes(30);
  myChoiceDate.setSeconds(10);

  System.out.println(myChoiceDate);
  ```

<br />

- 본인의 생년월일 표현

  - SimpleDateFormat 사용

  - 년도월일 ㅇㅇㅇ아 생일 축하한다~

  ```java
  public static void main(String[] args) {
    SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd");

    Date date = new Date();

    int year = 1990 - 1900;

    date.setYear(year);
    date.setMonth(8);
    date.setDate(1);

    String myDay = sdf.format(date);

    System.out.println(myDay + " ㅇㅇㅇ아 생일 축하한다~")
  }
  ```

<br />

- Calendar

  ```java
  package five29.basic;

  import java.util.Calendar;

  public class BasicDateMain3 {


    public static void main(String[] args) {

        Calendar today = Calendar.getInstance();

        int year = today.get(Calendar.YEAR);
        System.out.println("올 해의 년도: " + year);

        int month = today.get(Calendar.MONTH);
        System.out.println("월: " + month);

        System.out.println("이 해의 몇 째주: " + today.get(Calendar.WEEK_OF_YEAR));

        System.out.println("이 달의 몇 째주: " + today.get(Calendar.WEEK_OF_MONTH));

        int date = today.get(Calendar.DATE);
        System.out.println("이 달의 몇 일: " + date);

        System.out.println("이 달의 몇 일: " + today.get(Calendar.DAY_OF_MONTH));

        System.out.println("이 해의 몇 일: " + today.get(Calendar.DAY_OF_YEAR));

        System.out.println("요일(1~7, 일요일=1): " + today.get(Calendar.DAY_OF_WEEK));

        System.out.println("이 달의 몇 째 요일: "
          + today.get(Calendar.DAY_OF_WEEK_IN_MONTH));

        int hour = today.get(Calendar.HOUR);
        System.out.println(hour);

        int minute = today.get(Calendar.MINUTE);
        System.out.println(minute);

        int second = today.get(Calendar.SECOND);
        System.out.println(second);

    }

  }
  ```
