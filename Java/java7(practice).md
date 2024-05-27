## 정렬 알고리즘을 이용한 로또 번호 자동 출력기

> 단순 선택법(선택 정렬), 단순 교환법(버블 정렬), 단순 삽입법(삽입 정렬) 중 삽입 정렬의 속도가 평균적으로 가장 빠르다고 알려져 선택

<br />

- 중복되지 않은 무작위 6개의 숫자를 뽑는 메서드

```java
// 중복되지 않은 무작위 6개의 숫자를 뽑는 메서드
void randomLottoNums() {
  int[] arr = new int[6];

  for (int i = 0; i < 6; i++) {
    arr[i] = (int)(Math.random() * 45) + 1;

    // 중복 제거를 위한 반복문(j)
    // j를 증가시키며 nums[i]와 일치하는지 확인 후 중복 시 i--
    // 상단 반복문(i)이 다시 돌며 nums[i]에 새로운 값 할당
    for (int j = 0; j < i; j++) {
      if (arr[i] == arr[j]) {
        i--;
      }
    }
  }

  for (int i = 0; i < arr.length; i++) {
    lottoNumber[i] = arr[i];
  }
}
```

<br />

- 무작위 6개 숫자를 오름차순하는 정렬 메서드

```java
// 오름차순 정렬(단순 삽입법, 삽입 정렬) => 차례대로 올바른 위치에 삽입 => 선택한 값을 대소 관계가 올바른 위치에 삽입
// 배열에서 대소 관계 비교 후 작은 숫자가 앞으로 감
void ascendingArray() {
  int[] arr = this.lottoNumber;
  int temp; // arr[i]를 담을 임시 변수
  int k; // 인덱스 컨트롤을 위한 임시 변수

  for (int i = 1; i < arr.length; i++) {
    temp = arr[i];
    k = i;

    // ? ex) 0번째 값이 1번째보다 크다면 0번째 값을 1번째에 할당
    while (k > 0 && arr[k - 1] > temp) {
      arr[k] = arr[k - 1]; // 큰 값을 현재 index 값에 할당
      k -= 1; // k를 줄여가며 arr[i]와 arr[i - 1]을 비교
    }

    // 조건문을 돌았다면 k가 줄었기 때문에 작은 숫자가 arr[i]의 값보다 앞에 할당됨
    // 뒤의 값보다 앞의 값이 작다면 그대로 arr[i]의 값을 arr[i]에 할당
    arr[k] = temp;
  }

  for (int i = 0; i < arr.length; i++) {
    this.lottoNumber[i] = arr[i];
  }
}
```

<br />

- 무작위 6개 숫자를 내림차순하는 정렬 메서드

```java
// 내림차순 정렬(단순 삽입법, 삽입 정렬) => 차례대로 올바른 위치에 삽입 => 선택한 값을 대소 관계가 올바른 위치에 삽입
// 배열에서 대소 관계 비교 후 큰 숫자가 앞으로 감
void descendingArray() {
  int[] arr = this.lottoNumber;
  int temp; // arr[i]를 담을 임시 변수
  int k; // 인덱스와 값의 컨트롤을 위한 임시 변수

  for (int i = 1; i < arr.length; i++) {
    temp = arr[i];
    k = i;

    // ? ex) 0번째 값이 1번째 값보다 작다면 0번째 값을 1번째로 할당
    while (k > 0 && arr[k - 1] < temp) {
      arr[k] = arr[k - 1]; // 작은 값을 현재 index 값에 할당
      k -= 1; // k를 줄여가며 arr[i]와 arr[i - 1]을 비교
    }

    // 조건문을 돌았다면 k가 줄었기 때문에 큰 숫자가 arr[i]의 값보다 앞에 할당됨
    // 뒤의 값보다 앞의 값이 크다면 그대로 arr[i]의 값을 arr[i]에 할당
    arr[k] = temp;
  }

  for (int i = 0; i < arr.length; i++) {
    this.lottoNumber[i] = arr[i];
  }
}
```

<br />

- class 전체 코드

```java
public class Lotto {

  int[] lottoNumber = new int[6];

  // 중복되지 않은 무작위 6개의 숫자를 뽑는 메서드
  void randomLottoNums() {
    int[] arr = new int[6];

    for (int i = 0; i < 6; i++) {
      arr[i] = (int)(Math.random() * 45) + 1;

      // 중복 제거를 위한 반복문(j)
      // j를 증가시키며 nums[i]와 일치하는지 확인 후 중복 시 i--
      // 상단 반복문(i)이 다시 돌며 nums[i]에 새로운 값 할당
      for (int j = 0; j < i; j++) {
        if (arr[i] == arr[j]) {
          i--;
        }
      }
    }

    for (int i = 0; i < arr.length; i++) {
      lottoNumber[i] = arr[i];
    }
  }

  // 오름차순 정렬(단순 삽입법, 삽입 정렬) => 차례대로 올바른 위치에 삽입 => 선택한 값을 대소 관계가 올바른 위치에 삽입
  // 배열에서 대소 관계 비교 후 작은 숫자가 앞으로 감
  void ascendingArray() {
    int[] arr = this.lottoNumber;
    int temp; // arr[i]를 담을 임시 변수
    int k; // 인덱스 컨트롤을 위한 임시 변수

    for (int i = 1; i < arr.length; i++) {
      temp = arr[i];
      k = i;

      // ? ex) 0번째 값이 1번째보다 크다면 0번째 값을 1번째에 할당
      while (k > 0 && arr[k - 1] > temp) {
        arr[k] = arr[k - 1]; // 큰 값을 현재 index 값에 할당
        k -= 1; // k를 줄여가며 arr[i]와 arr[i - 1]을 비교
      }

      // 조건문을 돌았다면 k가 줄었기 때문에 작은 숫자가 arr[i]의 값보다 앞에 할당됨
      // 뒤의 값보다 앞의 값이 작다면 그대로 arr[i]의 값을 arr[i]에 할당
      arr[k] = temp;
    }

    for (int i = 0; i < arr.length; i++) {
      this.lottoNumber[i] = arr[i];
    }
  }

  // 내림차순 정렬(단순 삽입법, 삽입 정렬) => 차례대로 올바른 위치에 삽입 => 선택한 값을 대소 관계가 올바른 위치에 삽입
  // 배열에서 대소 관계 비교 후 큰 숫자가 앞으로 감
  void descendingArray() {
    int[] arr = this.lottoNumber;
    int temp; // arr[i]를 담을 임시 변수
    int k; // 인덱스와 값의 컨트롤을 위한 임시 변수

    for (int i = 1; i < arr.length; i++) {
      temp = arr[i];
      k = i;

      // ? ex) 0번째 값이 1번째 값보다 작다면 0번째 값을 1번째로 할당
      while (k > 0 && arr[k - 1] < temp) {
        arr[k] = arr[k - 1]; // 작은 값을 현재 index 값에 할당
        k -= 1; // k를 줄여가며 arr[i]와 arr[i - 1]을 비교
      }

      // 조건문을 돌았다면 k가 줄었기 때문에 큰 숫자가 arr[i]의 값보다 앞에 할당됨
      // 뒤의 값보다 앞의 값이 크다면 그대로 arr[i]의 값을 arr[i]에 할당
      arr[k] = temp;
    }

    for (int i = 0; i < arr.length; i++) {
      this.lottoNumber[i] = arr[i];
    }
  }
}

```
