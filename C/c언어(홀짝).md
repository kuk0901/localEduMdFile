# C언어로 만든 홀짝 게임

## 홀짝 게임

- 프로그램의 단계: 일반적으로 3가지

  1. 서론: 준비 => 변수(저장 공간), 라이브러리 선언

  2. 본론: 서비스, 로직, 본문 => 출력 내용(인터페이스)

  3. 결론: 출력 => 메모리 회수, 종료

  > 치환의 원리 : ex) 1 -> 홀 / 2 -> 짝 || n -> no / y -> yes

<br />

- 사용자에게 입력받은 숫자의 구구단 출력

  - 20단 입력 시 20단 출력

  - -999 입력 시 구구단 프로그램 종료

  ```c
  #include <stdio.h>

  int main(void) {
    int userNum = 0;
    int i;

    while (1)
    {
      printf("원하는 숫자를 입력하세요. 해당 숫자의 구구단 결과를 알려드립니다.\n");
      printf("(숫자 -999 입력 시 프로그램을 종료합니다.)\n");
      printf("숫자 입력: ");
      scanf("%d", &userNum);
      getchar();
      printf("\n")
      i = 1;

      if (userNum != -999)
      {
        while (i <= 9)
        {
          printf("%d * %d = %d", userNum, i, userNum * i);
          printf("\n");
          i++;
        }
      } else
      {
        break;
      }
      printf("\n")
    }

    printf("구구단 프로그램을 종료합니다.\n");
    printf("이용해 주셔서 감사합니다.");

    return 0;
  }
  ```
