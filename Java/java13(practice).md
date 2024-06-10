- 3개의 쇼핑 항목을 확인하는 프로그램

  - List: 의류, 잡화, 식품, 가전, 스포츠, 공구, 도서, 생필품

  - 키보드 입력을 받음

    - 입력받은 것을 리스트에 추가

  - 3개를 담은 경우, 사용자가 담은 항목을 출력한 후 프로그램 종료

  ```
  1. 쇼핑 항목에 없는 단어 입력 시
    잘못 입력했다는 메시지 출력 후 위의 내용 반복

  2. 물품이 담긴 상태에서 같은 물품 입력 시
    "이미 리스트에 있습니다. 다른 항목을 입력해 주세요." 출력 후 프로그램 반복
  ```

  ```java
  package Test.java.shoppingProject;

  import java.util.ArrayList;
  import java.util.Scanner;

  public class ShoppingListMain {
    public static void main(String[] args) {
      // 제품이 담긴 쇼핑 리스트
      ArrayList<String> shoppingList = new ArrayList<>();
      // 고객의 쇼핑 리스트
      ArrayList<String> userShoppingList = new ArrayList<>();
      Scanner scanner = new Scanner(System.in);
      shoppingList.add("의류");
      shoppingList.add("잡화");
      shoppingList.add("식품");
      shoppingList.add("가전");
      shoppingList.add("스포츠");
      shoppingList.add("공구");
      shoppingList.add("도서");
      shoppingList.add("생필품");

      String userInput; // 입력받을 변수
      boolean isValidItem; // 입력값이 유효한지 확인하는 용도의 변수
      boolean isDuplicateItem; // 중복값 체크를 위한 변수

      System.out.println("저희 매장을 찾아주셔서 감사합니다.");

      while (userShoppingList.size() < 3) {
        isValidItem = false;
        isDuplicateItem = false;

        System.out.println("현재 리스트에 추가 가능한 제품은 [의류, 잡화, 식품, 가전, 스포츠, 공구, 도서, 생필품] 입니다.");

        userInput = scanner.nextLine();

        for (int i = 0; i < shoppingList.size(); i++) {
          if (shoppingList.get(i).equals(userInput)) {
            isValidItem = true;
            break;
          }
        }

        if (!isValidItem) {
          System.out.println("리스트에 없는 제품을 입력하셨습니다. 다시 입력해 주세요.");
          System.out.println();
          continue;
        }

        for (int i = 0; i < userShoppingList.size(); i++) {
          isDuplicateItem = userShoppingList.get(i).equals(userInput);
        }

        if (isDuplicateItem) {
          System.out.println("이미 리스트에 추가한 제품입니다. 다시 입력해 주세요.");
          System.out.println();
          continue;
        }

        userShoppingList.add(userInput);
        System.out.println("현재 고객님께서 쇼핑 리스트에 추가하신 항목은 \"" + userInput + "\" 입니다.");
        System.out.println();

      }

      System.out.println("고객님의 쇼핑 리스트 목록은 " + userShoppingList + " 입니다.");
      System.out.println("프로그램 종료.");
      scanner.close();
    }
  }
  ```

  > 위 코드를 객체지향적으로 수정
