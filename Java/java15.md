# 15일차

## java 15일차

- 달 하나를 보여주는 코드

  ```java
  String weekTitle = "일\t월\t화\t수\t목\t금\t토\t";

  Calendar cal = Calendar.getInstance();

  int year = 2024;
  int month = 7;

  cal.set(year, month - 1, 1);

  System.out.printf("\t\t%d년\t %d월\n", year, month);
  System.out.println(weekTitle);

  int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);

  int lastDat = cal.getActualMaximun(Calendar.DAY_OF_MONTH);

  for (int i = 1; i < datOfWeek; i++) {
    System.out.print("\t");
  }

  for (int i = 1; i <= lastDay; i++) {
    System.out.print(i + "\t");

    if ((i + dayOfWeek - 1) % 7 == 0) {
      System.out.println();
    }
  }

  System.out.println();
  System.out.println();
  ```

<br />

- 달 하나를 보여주는 코드를 객체 지향적으로 변경

  - class의 method

  ```java
  public void printCal(int year, int month) {
    String weekTitle = "일\t월\t화\t수\t목\t금\t토\t";

    Calendar cal = Calendar.getInstance();

    cal.set(year, month - 1, 1);

    System.out.printf("\t\t%d년\t %d월\n", year, month);
    System.out.println(weekTitle);

    int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);

    int lastDat = cal.getActualMaximun(Calendar.DAY_OF_MONTH);

    for (int i = 1; i < datOfWeek; i++) {
      System.out.print("\t");
    }

    for (int i = 1; i <= lastDay; i++) {
      System.out.print(i + "\t");

      if ((i + dayOfWeek - 1) % 7 == 0) {
        System.out.println();
      }
    }

    System.out.println();
    System.out.println();
  }
  ```

<br />

- 한 해의 달력을 보여주는 메서드

  ```java
  public void printCal(int year) {
    for (int i = 1; i <= 12; i++) {
      printCal(year, i);
    }
  }
  ```
