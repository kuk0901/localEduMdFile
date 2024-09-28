# Spring 15일차(logic)

## 회원 목록의 칼럼 클릭 시 각 칼럼을 기준으로 정렬

- default: 번호 칼럼 내림차순

  - 번호 칼럼 첫 번째 클릭: 오름차순

  - 번호 칼럼 두 번째 클릭: 내림차순

  ```
  1. 번호 칼럼 정렬을 위한 이벤트: onclick 함수

  2. 함수 내에서 서버로 전달할 데이터 필요: 칼럼 명, 정렬에 대한 boolean 자료형 데이터(true / false)

    - 해당 boolean 데이터는 함수를 기존값 유지: 전역 변수

  3. controller에서 service로 함수 호출(controller에서 default로 true 설정)

  4. service에서 데이터 가공 true일 땐 내림차순, false일 땐 오름차순으로 데이터 가공

    - 데이터 가공과 관련된 util package 따로 생성

      => 데이터 가공 관련된 util 함수 생성 가능

  5. service에서 데이터 가공후 dao 호출

  6. mapper로 이동, 최종적으로 반환된 데이터를 controller에 전달 후 화면 구현
  ```
