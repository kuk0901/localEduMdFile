## 주사위 게임

- 주사위를 굴리고 그 합을 더해 누적값이 먼저 21에 도달하면 승리

```java
public class Dice {

  void diceRoll() {
    int acc = 0;
    int num = 0;

    for (int i = 1; acc < 21; i++) {
      acc += (int) (Math.random() * 6) + 1;
      num = i;
    }

    System.out.println("최종 주사위 굴린 횟수: " + num);
    System.out.println("주사위 누적합: " + acc + "\n");
  }
}
```

<br />

## 숫자 입력 후 출력

- 10개의 숫자 입력 후 출력(Scanner 없이 사용)

- 총합, 평균 출력

```java
public class AccAvg {

  void accAndAvg() {
    int acc = 0;
    int num = 0;

    for (int i = 1; i <= 10; i++) {
     int a = (int)(Math.random() * 101) ;
     System.out.println(i + ". 숫자 입력: " + a);
     acc += a;
     num = i;
    }

    System.out.println("\n각 숫자들의 총합: " + acc);
    System.out.println("각 숫자들의 평균: " + (double)acc / num);
  }
}
```

<br />

## 로또 번호 자동 출력기

- 6개의 로또 번호를 오름차순으로 정렬 후 출력

```java
import java.util.Arrays;

public class Lotto {
  void lotto() {
    int[] nums = new int[6];

    for (int i = 0; i < 6; i++) {
      nums[i] = (int)(Math.random() * 45) + 1;

      // 중복 제거를 위한 반복문(j)
      // j를 증가시키며 nums[i]와 일치하는지 확인 후 중복 시 i--
      // 상단 반복문(i)이 다시 돌며 nums[i]에 새로운 값 할당
      for (int j = 0; j < i; j++) {
        if (nums[i] == nums[j]) {
          i--;
        }
      }
    }

    // 배열 오름차순 정렬
    Arrays.sort(nums);

    for (int i = 0; i < nums.length; i++) {
      System.out.print(nums[i] + "\t");
    }
  }
}
```
